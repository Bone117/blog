# scrapy中间件中发送邮件

***
背景介绍：之前写过通过通过scrapy的扩展发送邮件，在爬虫关闭的时候发送邮件。那个时候有个问题就是`MailSender`对象需要`return`出去。这次需要在中间件中发送邮件，但是中间件中不能随便使用`return`了。
```python
import json
import random
import scrapy
from scrapy.http import Response
from scrapy.mail import MailSender
from scrapy.exceptions import IgnoreRequest

from order_spider.databases.connections import redis_db

class LoginTokenMiddleware(object):

    def __init__(self,mailer):
        self.mailer = mailer

    @classmethod
    def from_crawler(cls, crawler):
        smtphost = crawler.settings.get('MAIL_HOST')  # 发送邮件的服务器
        mail_port = crawler.settings.get('MAIL_PORT')  # 邮件发送者
        mailfrom = crawler.settings.get('MAIL_USER')  # 邮件发送者
        smtppass = crawler.settings.get('MAIL_PASS')  # 发送邮箱的密码不是你注册时的密码，而是授权码！！！切记！
        mailer = MailSender(smtphost, mailfrom, mailfrom, smtppass, smtpport=mail_port)
        return cls(mailer)

    def _send_mail(self,subject,body):
        return self.mailer.send(to={'feijun.zheng@huijie-inc.com'}, subject=subject, body=body)

    def process_request(self, request:scrapy.Request, spider):
        #从数据库获取所有的用户session
        tokens = redis_db.hgetall("order:xxx")
        users = []
        for k,v in tokens.items():
            #如果用户value有0，代表过期
            if "0" not in v:
                users.append(k)
        if not users:
            try:
                #通过end_signal判断爬虫是否继续执行
                if spider.end_signal:
                    raise IgnoreRequest
                # 设置为True，避免重复发送邮件
                spider.end_signal = True
                spider.logger.warning("session全部过期请重新添加")
                body = 'xxxxx全部过期'
                subject = '没有可用的账号，请重新添加'
                #mail添加回调，避免出现`exceptions.AttributeError: 'NoneType' object has no attribute 'bio_read'`
                self._send_mail(body,subject).addCallback(lambda x: x)
            except Exception as e:
                spider.logger.exception(e)
            finally:
                # 没有可用账号，关闭爬虫
                spider.crawler.engine.close_spider(spider, "爬虫关闭")
                # 忽略后续的请求
                raise IgnoreRequest

        session_id = random.choice(users)
        request.cookies = {"JSESSIONID":session_id}
        return None

    def process_response(self, request, response:Response, spider):
        res = json.loads(response.text)

        if res['code'] != 1:
            session_id = request.cookies['JSESSIONID']
            user = redis_db.hmget("order:xxxx",session_id)[0]
            redis_db.hset("order:xxxx",session_id,user+'_0')

            spider.logger.info("登录失败，失败原因:%s" %(res['msg']))
            body = 'session[%s] 可能已过期\n 失败原因%s'%(session_id,res['msg'])
            subject = '账号登录失败提醒'
            self._send_mail(body,subject).addCallback(lambda x: x)
        return response

```

**推荐**还是在扩展中使用发送邮件的功能，可以参考：
[scrapy通过扩展发送邮件](https://www.cnblogs.com/mangM/p/10790591.html)

> 还有一个小问题就是：阿里云上默认不能使用25端口，所以你需要使用456端口进行发送，456端口需要使用SSL，需要在原来的基础上做个小修改：
> `mailer = MailSender(mail_host, mail_user, mail_user, mail_pass, mail_port, smtptls=True, smtpssl=True)`

具体参数参考官方文档：
[scrapy文档](https://www.osgeo.cn/scrapy/topics/email.html)
