# 一次使用scrapy的问题记录

前景描述:

> 需要获取某APP的全国订单量，及抢单量。由于没有全国的选项所以只能分别对每一个城市进行订单的遍历。爬虫每天运行一次，一次获取48小时内的订单，从数据库中取出昨天的数据进行对比，有订单被抢则更新，无则不操作。(更新逻辑在这里不重要，重要的是爬取逻辑)。每个订单有发布时间，**根据发布时间判断，在48小时外的就停止爬取，开始爬取下一个城市。**

**先看第一版:**

```python
#spider

# 构造一些请求参数，此处省略
# 从配置中读取所有城市列表
cities = self.settings['CITY_CH']

# end_signal为某个城市爬取完毕的信号，
self.end_signal = False

for city in cities:
    # 通过for循环对每个城市进行订单爬取
    post_data.update({'locationName':city})
    count = 1
    while not self.end_signas:
        post_data.update({'pageNum':str(count)})
        data = ''.join(json.dumps(post_data, ensure_ascii=False).split())
        sign = MD5Util.hex_digest(api_key + data + salt).upper()
        params = {
            'apiKey':api_key,
            'data':data,
            'system':system,
            'sign':sign
        }
        meta = {'page':count}
        yield scrapy.Request(url=url, method='POST', body=json.dumps(params, ensure_ascii=False),
                             headers=self.headers, callback=self.parse,meta=meta, dont_filter=True)
        count+=1
    self.end_signal = False

def parse(self,response):
    # 略
```

```python
# 在spiderMiddleware中根据返回的item中的订单时间进行判断(此处不详写)

def process_spider_output(self, response, result, spider):
    result_bkp = []
    for res in result:
        if res['order_time'] < before_date(2): #before_date为自定义的时间函数
            logger.info("{%s}爬取完毕，开始爬取下一个城市" % (res['city_name']))
            spider.end_signal = True
            break
        result_bkp.append(res.copy())
    return result_bkp
```

> 乍一看没有问题，遍历每个城市，再到解析 解析完后返回item到spiderMiddleware中进行判断订单是否超过48小时，超过就设置`self.end_signal为True`跳出spider中的while循环，注意while循环后面又将这个参数设置`False`然后下个城市的循环就开始了。
> **问题来了：**
> spider中将request返回出去添加到队列中，**这里有一个队列**，当response下载好返回回来通过parse函数去处理的时候也有一个队列，众所周知运气不好的人总会偶尔遇到一点网络问题，来举个栗子就清楚了
> **栗子：**spider中将**城市A**的1、2、3订单页(2、3为超过48小时的订单页)，添加到队列中，下载器去下载的时候可能**第2页**代理挂了，第三页超过48小时，中间件判断成功设置`self.end_signal=True`进行下一个城市的爬取。**城市B**添加了1、2、3(都在48小时内)，**这个时候城市A的第二页订单下载完成了在中间件中判断又将**`self.end_signal=True` ，于是城市B后面的订单也就都没了，都没了。。。，直接开始了下一个城市的订单！

一版总结:

> 不要在一个异步的程序中通过一个全局变量去控制整个程序的流程。(总结的不好，可以帮我总结一下)

第二版:

> 既然不能通过全局变量来控制，那能不能让每个城市带一个标识来指明订单爬取结束。
> 先看代码

```python
#spider
cities = self.settings['CITY_CH']

# end_signal为某个城市爬取完毕的信号，
self.end_signal = False

for city in cities:
    # 通过for循环对每个城市进行订单爬取
    post_data.update({'locationName':city})
    count = 1
    print(cities)
    print(city)
    while in cities:
        post_data.update({'pageNum':str(count)})
        data = ''.join(json.dumps(post_data, ensure_ascii=False).split())
        sign = MD5Util.hex_digest(api_key + data + salt).upper()
        params = {
            'apiKey':api_key,
            'data':data,
            'system':system,
            'sign':sign
        }
        meta = {'page':count}
        yield scrapy.Request(url=url, method='POST', body=json.dumps(params, ensure_ascii=False),
                             headers=self.headers, callback=self.parse,meta=meta, dont_filter=True)
        count+=1
    self.end_signal = False

def parse(self,response):
    # 略

```

```python
# 在spiderMiddleware中根据返回的item中的订单时间进行判断(此处不详写)

def process_spider_output(self, response, result, spider):
    result_bkp = []
    for res in result:
        if res['order_time'] < before_date(2): #before_date为自定义的时间函数
            if res['city_name'] in spider.cities:
                spider.cities.remove(res['city_name'])
                logger.info("{%s}爬取完毕，开始爬取下一个城市" % (res['city_name']))
            break
        result_bkp.append(res.copy())
    return result_bkp

```

> 看逻辑也有点意思，判断这个城市是否在列表中，在的话说明还没爬取完毕，爬取完毕了就删除这个城市。嗯！运行一下！

> 有意思的来了，第一个城市爬取正常，第二个城市不见了,上诉代码中打印的城市没有显示第二个城市，直接跳到了最后一个(设就三个城市) 怎么被吞了呢。
> 敏感数据就不截图了。
> ![](https://blogimg-1256896917.cos.ap-shanghai.myqcloud.com/iPic/2019-09-12-134243.png)
> ![](https://blogimg-1256896917.cos.ap-shanghai.myqcloud.com/iPic/2019-09-12-134428.png)

> 可以看到 打印的城市列表中明明还有北京的没有被删除，为啥直接到最后一个城市了呢？
> 可能有大佬已经看出来了，我是生生打断点调试了半天，甚至怀疑是for循环内部有什么bug。
> 最后灵机一动(滑稽)，难倒是因为城市列表的问题？我for循环它，然后又在他内部去删除它里面的元素，可以这样吗？
> 写个demo测试一下

```python
cities = ['鞍山', '北京', '昆玉',]

for city in cities:
    cities.remove('鞍山')
    print(city)
```

```python
# 错误就来了！ 果然不能在循环它的时候再对它进行删除操作
ValueError: list.remove(x): x not in list
```

> 至于在运行scrapy的时候为什么没有报这个错误，可能是在别的地方做了异常处理，但是有这个问题在，我们先去修复它一下。
> 将`for city in cities`改为`for city in cities.copy()`,完美解决！！！
> 还有一个小点就是python的值传递和地址传递，在处理item的时候要注意。
