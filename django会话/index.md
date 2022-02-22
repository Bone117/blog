# django会话

# django会话

可以把会话理解为客户端与服务器之间的一次会晤，在一次会话过程中有多次请求和响应，但是由于HTTP协议的特性-->无状态，每次浏览器的请求都是无状态的，无法保存状态信息，也就是说后台服务器不知道当前请求是否和上一次的请求是来自同一个用户的，试想一下，淘宝京东，无法识别用户并保存用户的状态是致命的。

## 一、cookie的原理

为了保持连接状态，便有了cookie的由来，cookie是存储在本地服务器上的一个**key-value结构**的数据，类似于python中的字典，通过cookie除了可以保存连接状态，还可以保存用户信息等数据。

```
客户端发起请求，服务端生成cookie响应浏览器，这时客户端会将cookie保存起来，当下一次访问服务器的时候会将cookie一起发送给服务器，服务器通过cookie判断是谁的访问
```

**注：如果服务端发送重复的cookie，会覆盖原有的cookie**

## 二、django中操作cookie

### 1.启用会话

Django通过一个内置中间件来实现会话功能

编辑settings.py中的MIDDLEWARE设置

```python
django.contrib.sessions.middleware.SessionMiddlewar
```

### 2.配置会话引擎

```python
1. 数据库Session
SESSION_ENGINE = 'django.contrib.sessions.backends.db'   # 引擎（默认）
然后运行  manage.py migrate  在数据库内创建sessions表

2. 缓存Session
SESSION_ENGINE = 'django.contrib.sessions.backends.cache'  # 引擎
SESSION_CACHE_ALIAS = 'default'                            # 使用的缓存别名（默认内存缓存，也可以是memcache），此处别名依赖缓存的设置

3. 文件Session
SESSION_ENGINE = 'django.contrib.sessions.backends.file'    # 引擎
SESSION_FILE_PATH = None                                    # 缓存文件路径，如果为None，则使用tempfile模块获取一个临时地址tempfile.gettempdir() 

4. 缓存+数据库
SESSION_ENGINE = 'django.contrib.sessions.backends.cached_db'        # 引擎

5. 加密Cookie Session
SESSION_ENGINE = 'django.contrib.sessions.backends.signed_cookies'   # 引擎

其他公用设置项：
SESSION_COOKIE_NAME ＝ "sessionid"                     # Session的cookie保存在浏览器上时的key，即：sessionid＝随机字符串（默认）
SESSION_COOKIE_PATH ＝ "/"                             # Session的cookie保存的路径（默认）
SESSION_COOKIE_DOMAIN = None                           # Session的cookie保存的域名（默认）
SESSION_COOKIE_SECURE = False                          # 是否Https传输cookie（默认）
SESSION_COOKIE_HTTPONLY = True                         # 是否Session的cookie只支持http传输（默认）
SESSION_COOKIE_AGE = 1209600                           # Session的cookie失效日期（2周）（默认）
SESSION_EXPIRE_AT_BROWSER_CLOSE = False                # 是否关闭浏览器使得Session过期（默认）
SESSION_SAVE_EVERY_REQUEST = False                     # 是否每次请求都保存Session，默认修改之后才保存（默认）
```

### 3.在视图中操作cookie

#### 3.1设置cookie

```python
def set_cookie(request):
    obj = HttpResponse('ok')    #obj=render(request,...)
    obj.set_cookie('name', 'ABC')
    # obj.set_signed_cookie(key,value,salt='加密盐')
    obj.set_signed_cookie('name','lqz',salt='123')#加盐,123是个密码,解cookie的时候需要它,
    return obj
```

**参数：**

- key--->键
- value--->值
- max_age=None--->超时时间，cookie延续的时间，以秒为单位，
- 超时时间expires,传一个datatime对象	
- path='/',可以设置路径,设置路径之后,path='/index/',只有访问index的时候,才会携带cookie过来
- domain 设置域名下有效domain='map.baidu.com'
- secure=False, (默认是false,设置成True浏览器将通过HTTPS来回传cookie)
- httponly=True 只能https协议传输，阻止javascript对会话数据的访问，提高安全性。（不是绝对，底层抓包可以获取到也可以被覆盖)

#### 3.2获取cookie

```python
def get_cookie(request):
    print(type(request.COOKIES))
    # 取cookie的值
    print(request.COOKIES)
    name=request.COOKIES.get('name')
    
    #加盐,123是个密码,解cookie的时候需要它,
	# request.get_signed_cookie(key, default=RAISE_ERROR, salt='123', max_age=None)
    
    obj = HttpResponse('get_cookie')
    return obj
```

#### 3.3删除cookie

```python
def logout(request):
    rep = redirect("/login/")
    rep.delete_cookie("user")  # 删除用户浏览器上之前设置的usercookie值
    return rep
```

## 三、session

cookie本身保存在客户端，可能被拦截或窃取，所以网站设计通常将Cookie用来保存一些不重要的内容，实际的用户数据和状态还是以Session会话的方式保存在服务器端。

Session就是在**服务器端**的‘Cookie’，**Session依赖Cookie！**

```python
给每个客户端的Cookie分配一个唯一的id，这样用户在访问时，通过Cookie，服务器就知道来的人是“谁”。然后我们再根据不同的Cookie的id，在服务器上保存一段时间的私密资料，如“账号密码”等等。
```

![](C:\Users\Mine\Desktop\django\img\session.png)

### 1.设置session

```python
def set_session(request):
    # 写session,干了三件事(每个浏览器会生成一个随机字符串)
    # 随机字符串是跟浏览器相关的,数据是跟账号相关的
    request.session['name']='ABC'
    request.session['age']='18'
    request.session['sex']='男'
    
    '''
    非django的步骤(django已经封装了下面的操作)
    1 生成随机字符串:dfasfasdfa
	2 去数据库存储
	    随机字符串     值  (字典形式)                            超时时间
	    dfasfasdfa   {'name':'ABC','age':18,'sex':'男'}       超时时间
	3 写入set_cookie(set_cookie('sessionid','dfasfasdfa'))  发送给客户端
    '''
    return HttpResponse('ok')
```

### 2.获取session

```python
def get_session(request):
    # 取session   取name这个字段对应的值
    name=request.session['name']
    
    # 正常流程  非django的步骤(django已经封装了下面的操作)
    # 去cookie中取出随机字符串
    # 去session那个表去查询,取出session_data的数据,解密成字典,然后取name的值返回
    # print(name)
```

### 3.删除session

```python
# 删除当前会话的所有Session数据(只删数据库)
request.session.delete()
　　
# 删除当前的会话数据并删除会话的Cookie（数据库和cookie都删）。
request.session.flush() 
    这用于确保前面的会话数据不可以再次被用户的浏览器访问
    例如，django.contrib.auth.logout() 函数中就会调用它。
```

## 四、session的相关方法

```python
# 所有 键、值、键值对
request.session.keys()
request.session.values()
request.session.items()
request.session.iterkeys()
request.session.itervalues()
request.session.iteritems()

# 会话session的key
request.session.session_key

# 将所有Session失效日期小于当前日期的数据删除
request.session.clear_expired()

# 检查会话session的key在数据库中是否存在
request.session.exists("session_key")

# 设置会话Session和Cookie的超时时间
request.session.set_expiry(value)
    * 如果value是个整数，session会在些秒数后失效。
    * 如果value是个datatime或timedelta，session就会在这个时间后失效。
    * 如果value是0,用户关闭浏览器session就会失效。
    * 如果value是None,session会依赖全局session失效策略。
```

### 序列化会话

Django默认使用JSON序列化会话数据。你可以在`SESSION_SERIALIZER`设置中自定义序列化格式，甚至写入警告说明。但是强烈建议你还是使用JSON，尤其是以cookie的方式进行会话时。
