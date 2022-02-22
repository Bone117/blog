# Django学习笔记(http协议与django安装)

# Django入门

## HTTP协议

HTTP协议是Hyper Text Transfer Protocol（超文本传输协议）的缩写,是用于万维网（WWW:World Wide Web ）服务器与本地浏览器之间传输超文本的传送协议。

### http协议的特性

- 基于tcp/ip协议之上的应用层协议
- 基于请求-响应模式

```
请求是先由客户端发出,服务端响应并返回，服务端在没有收到请求的情况下不好发送响应
```

- 无状态保存

```
HTTP协议不保存状态，自身不对请求和响应之间的通信状态进行保存。也就是说，协议对发送的请求和响应都不做持久化处理。
但是很多网站当前页面跳转别的页面之后仍需要保持登录状态，这是就引入了cookie技术，有了cookie再用http协议通信就可以管理状态了
```

- 无连接

```
无连接的意思是限制每次连接只处理一个请求。服务端处理完请求就即刻断开连接，这种方式可以节约传输时间。
```

### http请求协议与响应协议

http协议包含浏览器发送数据给服务器所需的请求协议**与**服务器发送数据到浏览器的请求协议。

请求端(客户端)的hppt报文称为**请求报文**，响应端(服务器端)的称为**响应报文**

```
# 请求首行
# GET / HTTP/1.1\r\n
# # 请求头
# Host: 127.0.0.1:8001\r\n
# Connection: keep-alive\r\n
# Cache-Control: max-age=0\r\n
# Upgrade-Insecure-Requests: 1\r\n
# User-Agent: Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/68.0.3440.106 Safari/537.36\r\n
# Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8\r\n
# Accept-Encoding: gzip, deflate, br\r\nAccept-Language: zh-CN,zh;q=0.9\r\n\r\n'
# # 请求体(get请求，请求体为空)

POST请求
# 请求首行
POST /?name=lqz&age=18 HTTP/1.1\r\n
# 请求头
Host: 127.0.0.1:8008\r\nConnection: keep-alive\r\nContent-Length: 21\r\nCache-Control: max-age=0\r\nOrigin: http://127.0.0.1:8008\r\nUpgrade-Insecure-Requests: 1\r\nContent-Type: application/x-www-form-urlencoded\r\nUser-Agent: Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.181 Safari/537.36\r\nAccept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8\r\nReferer: http://127.0.0.1:8008/?name=lqz&age=18\r\nAccept-Encoding: gzip, deflate, br\r\nAccept-Language: zh-CN,zh;q=0.9\r\nCookie: csrftoken=7xx6BxQDJ6KB0PM7qS8uTA892ACtooNbnnF4LDwlYk1Y7S7nTS81FBqwruizHsxF\r\n\r\n
# 请求体
name=abc&password=123'
```

![](https://img2018.cnblogs.com/blog/1285461/201811/1285461-20181106094151747-1175090451.jpg)

![](https://img2018.cnblogs.com/blog/1285461/201811/1285461-20181106094200706-1683678595.png)

#### 请求方式：get与post请求

- GET提交的数据会放在URL后，以？分割URL和传输的数据，参数之间用&相连。POST是把提交的数据放在HTTP包的请求体中
- GET提交的数据大小有限制(URL长度限制)，POST提交的数据没有限制
- GET与POST请求在服务端获取请求数据方式不同。

#### 响应状态码

![](https://img2018.cnblogs.com/blog/1285461/201811/1285461-20181106094218787-1026413200.png)

### URL简介

统一资源定位符是互联网上标准资源的地址，互联网上的每个文件都有一个唯一的URL。                                                                                                                                                                                                                                                                                                

```
协议：//IP:端口(80)/路径?name=abc&age=123
？之前的是请求路径，？之后的是请求数据部分
```

## Django框架

### 一、django简介

djangon使用的是MTV模式他与MVC模式本质相同，只是定义上有点不同。

#### MVC

```
MVC就是将应用分为模型(M)，视图(V),控制器(C)三层，他们之间以一种插件式、松耦合的方式连接在一起，模型（M）负责业务对象与数据库的映射（ORM），视图（V）赋值与用户的交互，控制器接受用户的输入
```

![](https://img2018.cnblogs.com/blog/1285461/201811/1285461-20181106094259204-285920506.png)

#### MTV

- 模型（Model）：负责业务对象和数据库的关系映射(ORM).
- 模板（Template）：负责如何把页面展示给用户(html)
- 视图（View）：负责业务逻辑，并在适当时候调用Model和Template

除了以上三层之外，还需要一个URL分发器，它的作用是将一个个URL的页面请求分发给不同的View处理，View再调用相应的Model和Template，MTV的响应模式如下所示：

![](https://img2018.cnblogs.com/blog/1285461/201811/1285461-20181106094238610-1433967340.png)

```
用户通过浏览器向服务器发起一个请求(request),这个请求访问视图函数(如果不涉及数据调用，视图函数返回一个模板)，视图函数调用模型，模型去数据库查找数据，如何逐级返回，视图函数把返回的数据填充到模板空格中，最后返回页面给用户。
```

### 二、Django安装

##### 1.安装

```
方式一：在命令行输入：pip3 install django

pip install django==1.11.9 -i http://pypi.hustunique.org/simple   指定版本号，指定国内镜像

方式二：使用pycharm
```

##### 2.创建一个django project

```
命令创建：django-admin.py startproject mysite
创建app:python3 manage.py startapp app01
```

##### 3.文件目录介绍

```
-manage.py---项目入口,执行一些命令
-项目名
	-settings:全局配置信息
	-urls:总路由,请求地址跟视图函数的映射关系
-app名字
	-migrations:数据库迁移的记录
	-models.py  数据库表模型
	-views  视图函数
```
