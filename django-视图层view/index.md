# Django-视图层(view)

## 视图层(view)

​	视图函数，简称视图，本质上是一个简单的Python函数，它接受Web请求并且返回Web响应。响应的内容可以是HTML网页，重定向，404错误，图片等任何东西，但本质是返回**响应对象HttpResponse**。

​	视图函数的代码写哪里都可以，但一般约定俗成设置在项目或应用程序目录中的**views.py**文件中

**视图案例：**

```python
from django.shortcuts import render, HttpResponse, HttpResponseRedirect, redirect
import datetime

def current_datetime(request):
    now = datetime.datetime.now()
    html = "<html><body>It is now %s.</body></html>" % now
    return HttpResponse(html)
```

解析：

- 从 `django.shortcuts`模块导入了`HttpResponse`类，以及Python的`datetime`库。
- 定义了`current_datetime`函数。它就是视图函数。每个视图函数都使用`HttpRequest`对象作为第一个参数，并且通常称之为`request`。 视图函数的名字能反映除它的功能即可
- 视图函数最后返回`HttpResponse`对象，其中包含生成的响应。每个视图函数都负责返回一个`HttpResponse`对象。

### 一、HttpRequest对象

#### 请求对象(request)的属性：

```
django将请求报文中的请求行、首部信息、内容主题封装成HttpRequest类中的属性。除特殊说明外，其他均为只读
```

```
1. request.POST    # 前台Post传过来的数据,包装到POST字典中
2. request.GET     # 前台浏览器窗口里携带的数据,包装到GET字典中
3. request.method  # 前台请求的方式
4. request.body    # post提交的数据,body体的内容,前台会封装成:name=lqz&age=18&sex=1
5. request.path  # 取出请求的路径,取不到数据部分
6. request.encoding  #一个字符串，表示提交的数据的编码方式,默认'utf-8'
7. request.META  #一个标准的Python 字典，包含所有的HTTP 首部
        CONTENT_LENGTH —— 请求的正文的长度（是一个字符串）。
        CONTENT_TYPE —— 请求的正文的MIME 类型。
        HTTP_ACCEPT —— 响应可接收的Content-Type。
        HTTP_ACCEPT_ENCODING —— 响应可接收的编码。
        HTTP_ACCEPT_LANGUAGE —— 响应可接收的语言。
        HTTP_HOST —— 客服端发送的HTTP Host 头部。
        HTTP_REFERER —— Referring 页面。
        HTTP_USER_AGENT —— 客户端的user-agent 字符串。
        QUERY_STRING —— 单个字符串形式的查询字符串（未解析过的形式）。
        REMOTE_ADDR —— 客户端的IP 地址。
        REMOTE_HOST —— 客户端的主机名。
        REMOTE_USER —— 服务器认证后的用户。
        REQUEST_METHOD —— 一个字符串，例如"GET" 或"POST"。
        SERVER_NAME —— 服务器的主机名。
        SERVER_PORT —— 服务器的端口（是一个字符串）。
   --------------------------------
    除CONTENT_LENGTH 和 CONTENT_TYPE 之外，请求中的任何 HTTP 首部转换为 META 的键时，
    都会将所有字母大写并将连接符替换为下划线最后加上 HTTP_  前缀。
   ---------------------------------
8. request.FILES #包含所有的上传文件信息。
9. request.COOKIES  #字典格式，键和只都是字符串，包含所有的cookie
10. request.session  #当前会话，只有当django启用会话时才可用
11. request.user(用户认证组件)
	一个 AUTH_USER_MODEL 类型的对象，表示当前登录的用户。

　　	如果用户当前没有登录，user 将设置为 django.contrib.auth.models.AnonymousUser 的一个实例。       你可以通过 is_authenticated() 区分它们。

    例如：

    if request.user.is_authenticated():
        # Do something for logged-in users.
    else:
        # Do something for anonymous users.


     　　user 只有当Django 启用 AuthenticationMiddleware 中间件时才可用。

     -----------------------------------------------------------------------------

    匿名用户
    class models.AnonymousUser

    django.contrib.auth.models.AnonymousUser 类实现了django.contrib.auth.models.User 接     口，但具有下面几个不同点：

    id 永远为None。
    username 永远为空字符串。
    get_username() 永远返回空字符串。
    is_staff 和 is_superuser 永远为False。
    is_active 永远为 False。
    groups 和 user_permissions 永远为空。
    is_anonymous() 返回True 而不是False。
    is_authenticated() 返回False 而不是True。
    set_password()、check_password()、save() 和delete() 引发 NotImplementedError。
    New in Django 1.8:
    新增 AnonymousUser.get_username() 以更好地模拟 django.contrib.auth.models.User。

```

**注：**FILES 只有在请求的方法为POST 且提交的<form> 带有enctype="multipart/form-data" 的情况下才会
   包含数据。否则，FILES 将为一个空的类似于字典的对象。

#### request的常用方法：

```
1. request.get_full_path()
取出请求的路径,能取到数据部分,request.path取不到数据
2. request.is_ajax()
如果请求是通过XMLHttpRequest生成的，则返回True。这个方法的作用就是判断，当前请求是否通过ajax机制发送过来的。
3. request.is_secure()
如果使用的是Https，则返回True，表示连接是安全的。
```

### 二、HttpResponse对象

响应对象主要有三种形式：

- HttpResponse()
- render()
- redirect()

#### 1.HttpResponse

HttpResponse()括号内直接跟一个具体的字符串作为响应体。

#### 2.render

```
render(request,template_name,[,context])
结合一个
```

参数：

- request：用于生成响应的请求对象。
- template_name：要使用的模板名称。
- context：添加到模板上下文的一个字典。默认空字典，如果字典中某个值是可调用的，视图将在渲染模板前调用它。

render方法就是将一个模板页面中的模板语法进行渲染，最终渲染成一个html页面作为响应体。

#### 3.redirect

传递要重定向的一个硬编码的URL

```
def my_view(request):
	...
	return redirect('some/url')
```

也可以是一个完整的URL：

```
def my_view(request):
	...
	return redirect('http://www.baidu.com')
```

**重定向301和302的区别**

```
1）301和302的区别。

　　301和302状态码都表示重定向，就是说浏览器在拿到服务器返回的这个状态码后会自动跳转到一个新的URL地址，这个地址可以从响应的Location首部中获取
  （用户看到的效果就是他输入的地址A瞬间变成了另一个地址B）——这是它们的共同点。

　　他们的不同在于。301表示旧地址A的资源已经被永久地移除了（这个资源不可访问了），搜索引擎在抓取新内容的同时也将旧的网址交换为重定向之后的网址；

　　302表示旧地址A的资源还在（仍然可以访问），这个重定向只是临时地从旧地址A跳转到地址B，搜索引擎会抓取新的内容而保存旧的网址。 SEO302好于301

 

2）重定向原因：
（1）网站调整（如改变网页目录结构）；
（2）网页被移到一个新地址；
（3）网页扩展名改变(如应用需要把.php改成.Html或.shtml)。
        这种情况下，如果不做重定向，则用户收藏夹或搜索引擎数据库中旧地址只能让访问客户得到一个404页面错误信息，访问流量白白丧失；再者某些注册了多个域名的
    网站，也需要通过重定向让访问这些域名的用户自动跳转到主站点等。
```

### 三、JsonResponse对象

向前端返回一个json格式字符串的两种方式

```python
# 第一种方式
# import json
# data={'name':'lqz','age':18}
# data1=['lqz','egon']
# return HttpResponse(json.dumps(data1))
# 第二种方式
from django.http import JsonResponse
# data = {'name': 'lqz', 'age': 18}
data1 = ['lqz', 'egon']
# return JsonResponse(data)
return JsonResponse(data1,safe=False)
```

### 四、CBV和FBV

CBV是基于类的视图(Class base view)和FBV基于函数的视图(Function base view)

```python
from django.views import View
class AddPublish(View):
    def dispatch(self, request, *args, **kwargs):
        print(request)
        print(args)
        print(kwargs)
        # 可以写类似装饰器的东西，在前后加代码
        obj=super().dispatch(request, *args, **kwargs)
        return obj

    def get(self,request):
        return render(request,'index.html')
    def post(self,request):
        request
        return HttpResponse('post')
```

### 五、简单文件上传

```
def fileupload(request):
    if request.method=='GET':
        return render(request,'fileupload.html')
    if request.method=='POST':
        # FILES
        print(request.FILES)
        print(type(request.FILES.get('myfile')))
        # 从字典里根据名字,把文件取出来
        myfile=request.FILES.get('myfile')
        from django.core.files.uploadedfile import InMemoryUploadedFile
        # 文件名字
        name=myfile.name
        # 打开文件,把上传过来的文件存到本地
        with open(name,'wb') as f:
            # for line in myfile.chunks():
            for line in myfile:
                f.write(line)
        return HttpResponse('ok')
```
