# Python重试模块retrying

# Python重试模块retrying

> 工作中经常碰到的问题就是，某个方法出现了异常，重试几次。循环重复一个方法是很常见的。比如爬虫中的获取代理，对获取失败的情况进行重试。
> 刚开始搜的几个博客讲的有点问题，建议看官方文档，还有自己动手实验。

参考：
https://segmentfault.com/a/1190000004085023
https://pypi.org/project/retrying/

最初的版本

```python
import requests

class ProxyUtil:

    def __init__(self):
        self._get_proxy_count = 0

    def get_proxies(self):
        try:
            r = requests.get('代理服务器地址')
            # print('正在获取')
            # raise Exception("异常")
            # print('获取到最新代理 = %s' % r.text)
            params = dict()
            if r and r.status_code == 200:
                proxy = str(r.content, encoding='utf-8')
                params['http'] = 'http://' + proxy
                params['https'] = 'https://' + proxy
            else:
                raise Exception("获取代理失败,状态码%s"%(r.status_code))
    
            return params
        except Exception:
            if self._get_proxy_count < 5:
                print('第%d次获取代理失败，准备重试' % self._get_proxy_count)
                self._get_proxy_count += 1
                self.get_proxies()
            else:
                print('第%d次获取代理失败，退出' % self._get_proxy_count)
                self._get_proxy_count = 0
                return dict()
if __name__ == '__main__':
    proxy = ProxyUtil()
    proxy.get_proxies()
```

> 以上代码通过`try...except...`捕获异常，并通过一个计数器判断获取代理的次数，获取失败递归调用自己，直到达到最大次数为止。
> 为了模拟失败，可以解开抛出异常的注释

下面来试试retrying模块
安装
`pip install retrying`

> retrying提供一个装饰器函数retry，被装饰的函数会在运行失败的情况下重新执行，默认一直报错就一直重试。

```python
import requests
from retrying import retry

class ProxyUtil:

    def __init__(self):
        self._get_proxy_count = 0

    @retry
    def get_proxies(self):

        r = requests.get('代理地址')
        print('正在获取')
        raise Exception("异常")
        print('获取到最新代理 = %s' % r.text)
        params = dict()
        if r and r.status_code == 200:
            proxy = str(r.content, encoding='utf-8')
            params['http'] = 'http://' + proxy
            params['https'] = 'https://' + proxy

if __name__ == '__main__':
    proxy = ProxyUtil()
    proxy.get_proxies()
```

结果：

> 正在获取
> 正在获取
> 正在获取
> ...
> 正在获取(一直重复下去)
> 没有添加任何参数，默认情况下会一直重试，没有等待时间

```python
# 设置最大重试次数
@retry(stop_max_attempt_number=5)
def get_proxies(self):
    r = requests.get('代理地址')
    print('正在获取')
    raise Exception("异常")
    print('获取到最新代理 = %s' % r.text)
    params = dict()
    if r and r.status_code == 200:
        proxy = str(r.content, encoding='utf-8')
        params['http'] = 'http://' + proxy
        params['https'] = 'https://' + proxy
```

```python
# 设置方法的最大延迟时间，默认为100毫秒(是执行这个方法重试的总时间)
@retry(stop_max_attempt_number=5,stop_max_delay=50)
# 通过设置为50，我们会发现，任务并没有执行5次才结束！
```

```python
# 添加每次方法执行之间的等待时间
@retry(stop_max_attempt_number=5,wait_fixed=2000)
# 随机的等待时间
@retry(stop_max_attempt_number=5,wait_random_min=100,wait_random_max=2000)
# 每调用一次增加固定时长
@retry(stop_max_attempt_number=5,wait_incrementing_increment=1000)
```

```python
# 根据异常重试，先看个简单的例子
def retry_if_io_error(exception):
    return isinstance(exception, IOError)

@retry(retry_on_exception=retry_if_io_error)
def read_a_file():
    with open("file", "r") as f:
        return f.read()
```

> `read_a_file`函数如果抛出了异常，会去`retry_on_exception`指向的函数去判断返回的是`True`还是`False`，如果是`True`则运行指定的重试次数后，抛出异常，`False`的话直接抛出异常。
> 当时自己测试的时候网上一大堆抄来抄去的，意思是`retry_on_exception`指定一个函数，函数返回指定异常，会重试，不是异常会退出。真坑人啊！
> 来看看获取代理的应用(仅仅是为了测试retrying模块)

```python
# 定义一个函数用于判断返回的是否是IOError
def wraper(args):
    return isinstance(args,IOError)

class ProxyUtil:
    def get_proxies(self):
        r = requests.get('http://47.98.163.40:17000/get?country=local')
        print('正在获取')
        raise IOError
        # raise IndexError
        print('获取到最新代理 = %s' % r.text)
        params = dict()
        if r and r.status_code == 200:
            proxy = str(r.content, encoding='utf-8')
            params['http'] = 'http://' + proxy
            params['https'] = 'https://' + proxy

    # @retry_handler(retry_time=2, retry_interval=5, retry_on_exception=[IOError,IndexError])
    @retry(stop_max_attempt_number=5,retry_on_exception=wraper)
    def retry_test(self):
        self.get_proxies()
        print('io')
```

> 这种方法只能判断单一的异常，而且扩展性不够高

```python
# 通过返回值判断是否重试
    def retry_if_result_none(result):
        """Return True if we should retry (in this case when result is None), False otherwise"""
        # return result is None
        if result =="111":
            return True

    @retry(stop_max_attempt_number=5,retry_on_result=retry_if_result_none)
    def might_return_none():
        print("Retry forever ignoring Exceptions with no wait if return value is None")
        return "111"

    might_return_none()
```

> `might_return_none`函数的返回值传递给`retry_if_result_none`的`result`，通过判断result,返回`Treu`或者`None`表示需要重试，重试结束后抛出`RetryError`，返回`False`表示不重试。
> 扩展默认的retry装饰器：

```python
def retry_handler(retry_time: int, retry_interval: float, retry_on_exception: [BaseException], *args, **kwargs):

    def is_exception(exception: [BaseException]):
        for exp in retry_on_exception:
            if isinstance(exception,exp):
                return True
        return False
        # return isinstance(exception, retry_on_exception)

    def _retry(*args, **kwargs):
        return Retrying(wait_fixed=retry_interval * 1000).fixed_sleep(*args, **kwargs)

    return retry(
        wait_func=_retry,
        stop_max_attempt_number=retry_time,
        retry_on_exception=is_exception
    )

class ProxyUtil:
    def get_proxies(self):
        r = requests.get('代理地址')
        print('正在获取')
        raise IOError
        # raise IndexError
        print('获取到最新代理 = %s' % r.text)
        params = dict()
        if r and r.status_code == 200:
            proxy = str(r.content, encoding='utf-8')
            params['http'] = 'http://' + proxy
            params['https'] = 'https://' + proxy

    @retry_handler(retry_time=2, retry_interval=5, retry_on_exception=[IOError,IndexError])
    # @retry(stop_max_attempt_number=5,retry_on_exception=wraper)
    def retry_test(self):
        self.get_proxies()
        print('io')

if __name__ == '__main__':
    proxy = ProxyUtil()
    proxy.retry_test()
```
