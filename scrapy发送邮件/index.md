# scrapy发送邮件

## scrapy发送邮件

> 应用场景：在爬虫关闭或者爬虫空闲时可以通过发送邮件的提醒。
>
> 通过twisted的非阻塞IO实现,可以直接写在spider中，也可以写在中间件或者扩展中，看你具体的需求。

> 在网上找了很多教程，都是很多年前的或者就是官网搬运的，一点实际的代码都没有，所以就自己尝试了一下，由于本人也是爬虫新手，轻喷，轻喷！

> 看下面的示例代码前，先看下官网，熟悉基本的属性。
>
> 官网地址sending e-mail：`<https://docs.scrapy.org/en/latest/topics/email.html?highlight=MailSender>`

1. 首先在`settings`同级的目录下创建`extendions`(扩展)文件夹，

   代码如下：

   ```python
   import logging
   from scrapy import signals
   from scrapy.exceptions import NotConfigured
   from scrapy.mail import MailSender
   logger = logging.getLogger(__name__)
   class SendEmail(object):
   
       def __init__(self,sender,crawler):
           self.sender = sender
           crawler.signals.connect(self.spider_idle, signal=signals.spider_idle)
           crawler.signals.connect(self.spider_closed, signal=signals.spider_closed)
   
       @classmethod
       def from_crawler(cls,crawler):
           if not crawler.settings.getbool('MYEXT_ENABLED'):
               raise NotConfigured
   
           mail_host = crawler.settings.get('MAIL_HOST') # 发送邮件的服务器
           mail_port = crawler.settings.get('MAIL_PORT') # 邮件发送者
           mail_user = crawler.settings.get('MAIL_USER') # 邮件发送者
           mail_pass = crawler.settings.get('MAIL_PASS') # 发送邮箱的密码不是你注册时的密码，而是授权码！！！切记！
   
           sender = MailSender(mail_host,mail_user,mail_user,mail_pass,mail_port) #由于这里邮件的发送者和邮件账户是同一个就都写了mail_user了
           h = cls(sender,crawler)
   
           return h
   
       def spider_idle(self,spider):
           logger.info('idle spider %s' % spider.name)
   
       def spider_closed(self, spider):
           logger.info("closed spider %s", spider.name)
           body = 'spider[%s] is closed' %spider.name
           subject = '[%s] good!!!' %spider.name
           # self.sender.send(to={'zfeijun@foxmail.com'}, subject=subject, body=body)
           return self.sender.send(to={'zfeijun@foxmail.com'}, subject=subject, body=body)
   ```

   > 这里为什么是`return self.sender.send`，是因为直接用`sender.send`会报`builtins.AttributeError: 'NoneType' object has no attribute 'bio_read'`的错误（邮件会发送成功），具体原因不是很懂，有大牛知道的可以指导一下。
   >
   > 解决方法参考：`<https://github.com/scrapy/scrapy/issues/3478>`
   >
   > 在`sender.send`前加`return`就好了。

2. 在扩展中写好代码后，需要在`settings`中启用

```python
EXTENSIONS = {
    # 'scrapy.extensions.telnet.TelnetConsole': 300,
    'bukalapak.extendions.sendmail.SendEmail': 300,
}
MYEXT_ENABLED = True
```

转载请注明出处！
