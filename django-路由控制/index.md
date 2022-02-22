# Django-路由控制

## Django-路由控制

### 一、URL路由基础

```
	URL是web服务的路口，用户通过浏览器发送过来的任何请求都会被发送到一个指定的URL地址里，然后被响应。
    在django项目中编写路由就是向外暴露我们接收哪些URL的请求，除此之外任何的URL都不会被处理，URL路由就是web服务对外暴露的API
```

### 二、Django处理请求

1. 确定要使用的`URLconf`模块，通常是settings中`ROOT_URLCONF`设置的值，如果传入的`HttpRequest`对象具有`urlconf`属性(中间件设置)，则使用其值代替settings中`ROOT_URLCONF`
2. Django加载模块并查找可用的`urlpatterns`,它是`django.conf.urls.url()`实例的一个列表
3. 按顺序运行每个URL模式，匹配成功就停下来，所以**顺序很关键**
4. 匹配成功导入给定的视图，它是一个python函数，或基于类的视图，视图将获得如下参数
    - 一个`HttpRequest`实例
    - 如果匹配的正则表达式返回了无名分组，那么它将作为位置参数提供给视图
    - 关键字参数由正则的有名分组组成，但是可以被`django.conf.urls.url()`的可选参数kwargs覆盖
5. 如果没有URL模式匹配，或者过程出错了，将调用错误处理视图

### 三、简单的路由配置

```python
from django.conf.urls import url

urlpatterns=[
    url(正则表达式，view视图函数，参数，别名)
]
```

示例的URLconf：

```django
from django.urls import url

from . import views

urlpatterns = [
    url(r'^articles/2003/$', views.special_case_2003),
    url(r'^articles/([0-9]{4})/$', views.year_archive),
    url(r'^articles/([0-9]{4})/([0-9]{2})/$', views.month_archive),
    url(r'^articles/([0-9]{4})/([0-9]{2})/([0-9]+)/$', views.article_detail),
]
```

**注：**

- 从URL中捕获一个值，可以加园括号或者尖括号

- 不要添加前导的防斜杠，因为每个URL都有，例如，应该是`^articles`而不是`^/articles`
- 每个正则表达式前面的'r'是可选的，建议添加上，它告诉python这个字符串中的任何字符都不应该被转义

```django
请求的例子及匹配的url

/articles/2005/03/将匹配列表中的第三个模式。Django将调用函数views.month_archive(request, '2005', '03')。
/articles/2005/3/不匹配任何URL模式，因为列表中的第三个模式要求月份是两个数字。
/articles/2003/将匹配列表中的第一个模式不是第二个，因为模式按顺序从上往下匹配，第一个会首先被匹配。Django会调用函数views.special_case_2003(request)
/articles/2003不匹配任何一个模式，因为每个模式都要求URL以一个斜杠结尾。
/articles/2003/03/03/将匹配最后一个模式。Django将调用函数views.article_detail(request, '2003', '03', '03')。
```

是否开启URL访问地址后面  不为/跳转至带有/路径的配置项

```
APPEND_SLASH=True
Django settings.py配置文件中默认没有 APPEND_SLASH 这个参数，但 Django 默认这个参数为 APPEND_SLASH = True。 其作用就是自动在网址结尾加'/'。
```

### 四、有名分组

有名分组的语法是`(?P<name>pattern)`,其中name是组的名字，pattern是匹配的模式

使用有名分组重写上面的URLconf：

```django
from django.conf.urls import url

from . import views

urlpatterns = [
    url(r'^articles/2003/$', views.special_case_2003),
    url(r'^articles/(?P<year>[0-9]{4})/$', views.year_archive),
    url(r'^articles/(?P<year>[0-9]{4})/(?P<month>[0-9]{2})/$', views.month_archive),
    url(r'^articles/(?P<year>[0-9]{4})/(?P<month>[0-9]{2})/(?P<day>[0-9]{2})/$', views.article_detail),
]
```

**注：**捕获的值作为关键字参数而不是位置参数传递给视图函数。 

`/articles/2005/03/`请求将调用`views.month_archive(request, year='2005', month='03')`函数，而不是`views.month_archive(request, '2005', '03')`。

### 五、无名分组有名分组总结

#### 1.无名分组

- 按位置传参

- 分组之后，将分组好的数据当做位置传参到视图函数，所以视图函数需要定义形参

    示例：

    ​	url:`(r'^articles/([0-9]{4})/([0-9]{2})$', views.article_detail)`

    ​	视图函数：`def article_detail(request,*args)`

####   2.有名分组

- 按关键字传参

- 分组后，会把分组出来的数据当做关键字参数传到视图函数，所以视图函数需要定义形参，形参名字和分组的名字相对应，与顺序无关

    示例：

    ​	url：`(r'^articles/(?P<year>[0-9]{4})/(?P<mounth>[0-9]{2})/$', views.article_detail),`

    ​	视图函数：`def article_detail(request,mounth,year)`

    **注：**有名分组和无名分组最好不要混用

### 六、反向解析

```
在django项目中，一个常见的需求是获得URL的最终形式，以用于嵌入到生成的内容中(视图中和显示给用户的URL等)或者用于处理服务器端的导航(重定向)。不希望通过硬编码URL
```

Django提供了一种解决方案，只需在URL中提供一个name参数，并赋值一个你自定义的、好记的、直观的字符串。

- 在模板中：使用url模板标签
- 在python代码中，使用reverse()函数

示例：

url配置：

1. 无参数：`url(r'^publishadd111/$',views.publishadd,name='ddd'),`
2. 无名分组:`url(r'^publishadd/([0-9]{4})/([0-9]{2})/$', views.publishadd,name='ddd'),`
3. 有名分组：`url(r'^publishadd/(?P<year>[0-9]{4})/(?P<mounth>[0-9]{2})/$',views.publishadd,name='ddd'),`

模板层：

1. 无参数：`{% url 'ddd' %}`
2. 无名分组：`{% url 'ddd' 2018 12 %}`  空格隔开，传多个值
3. 有名分组：`{% url 'ddd' 2018 12 %}`  还可以 `{% url 'ddd' year=2018 mounth=12 %}`

视图层：

from django.shortcuts import reverse

1. 无参数：`url=reverse('ddd')`
2. 无名分组：`url=reverse('ddd',args=(2018,12,))`
3. 有名分组：`url=reverse('ddd',args=(2018,12,)) `还可以` url=reverse('ddd',kwargs={'year':2018,'mounth':12})`

### 七、路由分发

在每个app里各自创建一个urls.py路由模块，然后从根路由出发，将app所属的url请求全部转发到相应的urls.py模块中。

Django1.1版本的分发

```
from django.conf.urls import url,include
```

例子：

```
总路由：
-from django.conf.urls import include 
-url(r'^blog/',include('blog.urls')),
-url(r'^app01/',include('app01.urls')),
各自路由配置url
app01--url(r'^publish/$', views.publish,name='app01_test'),
blog--url(r'^blogtest/$', views.test,name='blog_test'),
```

路由分发使用的是include()方法，需要提前导入，他的参数是转发目的地地路径的字符串。

**重点：**总路由后面不能加`$`

两个不同的app，在各自的urlconf中为某一条url取了相同的name，这就会带来麻烦。为了解决这个问题，又引出了下面的命名空间。

### 八、命名空间

由于name没有作用域，Django在反解URL时，会在项目全局顺序搜索，当查找到第一个name指定URL时，立即返回。URL命名空间可以保证反查到唯一的URL，即使不同的app使用相同的URL名称。

**示例：**

urls.py

```
url(r'^blog/',include('blog.urls')),
url(r'^app01/',include('app01.urls')),
```

blog的urls.py

```
url(r'^blogtest/$', views.test,name='test'),
```

app01的urls.py

```
url(r'^publish/$', views.publish,name='test'),
```

blog的视图函数

```
def test(request):
	url=reverse('test')
	return HttpResponse('blog test)
```

app01的视图函数

```
def test(request):
	url=reverse('test')
	return HttpResponse('app01 test)
```

无论如何找index都是找的app01的index。

解决方法：在总路由分发的时候指定名称空间，实现命名空间的做法很简单，在urlconf文件中添加`namespace='xxx'`即可。

```
url(r'^blog/',include('blog.urls',namespace='blog')),
url(r'^app01/',include('app01.urls',namespace='app01')),
```

在视图函数反向解析的时候，指定是哪个名称空间下的

```
url = reverse('blog:test')
```

在模板里也指定是

```
{% url 'blog:test'%}
```

不是很推荐使用名称空间，推荐的是在子路由的name中加入app的前缀

```
url(r'^publish/$', views.publish,name='app01_test'),
```

### 九、伪静态

和真静态URL类似。他是通过伪静态规则把动态URL伪装成静态网址。

在urls.py文件中自己添加匹配`.html`

`url(r'^book/(?P<id>\d+.html)',views.book),`
