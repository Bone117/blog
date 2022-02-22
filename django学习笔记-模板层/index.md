# django学习笔记-模板层

## 模板层

```
将Python嵌入到HTML中。
```

### 模板简介

将HTML硬解码到视图并不是那么完美原因如下：

- 对页面设计时也需要对python代码进行相应的修改，模板可以不就行python代码修改的情况下变更设计。
- 编写python和HTML设计是两项不同的工作，应该明确分工。
- 两项同时进行效率最高。

**模板：HTML代码+模板语法**

```
def current_time(req):
    # ================================原始的视图函数
    # import datetime
    # now=datetime.datetime.now()
    # html="<html><body>现在时刻：<h1>%s.</h1></body></html>" %now
    # return HttpResponse(html)
    
    #另一种写法(推荐)
    import datetime
    now=datetime.datetime.now()
    return render(req, 'current_datetime.html', {'current_date':str(now)[:19]})
```

**模板语法的重点：**

```
变量：{{ 变量名 }}
	1.深度查询用句点符
	2.过滤器
标签：{%  %}
```

## 二、模板语言的注释

```
{# this won't be rendered #}  单行注释
```

注释多行使用comment标签

## 三、模板语法之变量

在django模板中遍历复杂数据结构的关键是句点字符，语法：{{ 变量名 }}。变量类似于python中的变量。

```
变量的命名包括任何字母数字以及下划线("_")的组合。点(".")也有可能会在变量名中出现，不过它有特殊的含义。最重要的是，变量名称中不能有空格或标点符号。
```

**views.py:**

```python
name='abc'
age=18
li=[1,2,'3','4']
dic={'name':'abc','age':18,''li:[1,2,4]}
def test():  #函数
    print('abc')
    return 'abchahahah'
class Person():
	def __init__(self,name,age)
    	self.name=name
        self.age=age
    def get_name(self):
        return self.name
    @classmethod
    def cls_test(cls):
        return 'cls'
    @staticmethod
    def static_test():
        return 'static'
    ---------
    # 模板里不支持带参数
    def get_name_cs(self,ttt):
        return self.name
    ----------
    
zfj=Person('zfj',18)
qer=Person('qwe',28)
person_list=[zfj,qwe]
```

**html：**

**相当于print了该变量**

```html
<p>字符串:{{ name }}</p>
<p>数字:{{ age }}</p>
<p>列表:{{ li }}</p>
<p>元祖:{{ tu }}</p>
<p>字典:{{ dic }}</p>
<p>函数:{{ test }}</p>  {#只写函数名:相当于函数名(),返回函数执行结果#}
<p>对象:{{ zfj }}</p>   {#对象内存地址#}
```

**深度查询：**

```html
<p>列表第1个值:{{ ll.0 }}</p>
<p>列表第4个值:{{ ll.3 }}</p>
<p>字典取值:{{ dic.name }}</p>
<p>字典取列表值:{{ dic.li }}</p>
<p>对象取数据属性:{{ zfj.name }}</p>
<p>对象取绑定给对象的函数属性:{{ zfj.get_name }}</p>
<p>对象取绑定给类的函数属性:{{ zfj.cls_test }}</p>
<p>对象取静态方法:{{ zfj.static_test }}</p>
```

## 四、模板语言之过滤器

**语法：**

```
{{变量名|过滤器名称：属性值}}
```

过滤器可以“链接”，一个过滤器的输出应用于下一个过滤器`{{text|escape|linebreaks}}`,先转移文本内容，然后把文本转成`<p>`标签。

**注：过滤器参数包含空格，必须用引号抱起来。**

### 1.过滤器总览

| 过滤器             | 说明                                                  |
| ------------------ | ----------------------------------------------------- |
| add                | 加法                                                  |
| addslashes         | 添加斜杠                                              |
| capfirst           | 首字母大写                                            |
| center             | 文本居中                                              |
| cut                | 切除字符                                              |
| date               | 日期格式化                                            |
| default            | 设置默认值                                            |
| default_if_none    | 为None设置默认值                                      |
| dictsort           | 字典排序                                              |
| dictsortreversed   | 字典反向排序                                          |
| divisibleby        | 整除判断                                              |
| escape             | 转义                                                  |
| escapejs           | 转义js代码                                            |
| filesizeformat     | 文件尺寸人性化显示                                    |
| first              | 第一个元素                                            |
| floatformat        | 浮点数格式化                                          |
| force_escape       | 强制立刻转义                                          |
| get_digit          | 获取数字                                              |
| iriencode          | 转换IRI                                               |
| join               | 字符列表链接                                          |
| last               | 最后一个                                              |
| length             | 长度                                                  |
| length_is          | 长度等于                                              |
| linebreaks         | 行转换                                                |
| linebreaksbr       | 行转换                                                |
| linenumbers        | 行号                                                  |
| ljust              | 左对齐                                                |
| lower              | 小写                                                  |
| make_list          | 分割成字符列表                                        |
| phone2numeric      | 电话号码                                              |
| pluralize          | 复数形式                                              |
| pprint             | 调试                                                  |
| random             | 随机获取                                              |
| rjust              | 右对齐                                                |
| safe               | 安全确认                                              |
| safeseq            | 列表安全确认                                          |
| slice              | 切片                                                  |
| slugify            | 转换成ASCII                                           |
| stringformat       | 字符串格式化                                          |
| striptags          | 去除HTML中的标签                                      |
| time               | 时间格式化                                            |
| timesince          | 从何时开始                                            |
| timeuntil          | 到何时多久                                            |
| title              | 所有单词首字母大写                                    |
| truncatechars      | 截断字符                                              |
| truncatechars_html | 截断字符                                              |
| truncatewords      | 截断单词                                              |
| truncatewords_html | 截断单词                                              |
| unordered_list     | 无序列表                                              |
| upper              | 大写                                                  |
| urlencode          | 转义url                                               |
| urlize             | url转成可点击的链接                                   |
| urlizetrunc        | urlize的截断方式                                      |
| wordcount          | 单词计数                                              |
| wordwrap           | 单词包裹                                              |
| yesno              | 将True，False和None，映射成字符串‘yes’，‘no’，‘maybe’ |

### 2.常用过滤器

#### default

```
如果变量是false或者为空，则使用给定的默认值。
```

`{{ value|default:'nothing'}}`

#### length

```
返回值的长度，对字符串和列表都起作用
```

`{{ value|length}}`  如果value是`[1,2,3,4]`,那么输出是4

#### filesizeformat

```
将值格式化成'人类可读的'文件尺寸。例如：
{{1024|filesizeformat}}  输出为1kb
```

#### date

如果`value=datetime.datetime.now()`

```
{{ value|date:"Y-m-d"}}
```

#### slice

切片

```
{{ name|slice:'0:3:3' }}
```

#### truncatechars

如果字符串多余指定字符数量，多余会被截断，替换成("...")结尾。

```
{{ value|truncatechars:9}}
```

#### safe

Django模板为了安全默认会对HTML标签和js等语法标签进行转义，有时候我们不希望这些元素被转义，可以通过设置过滤器。

```
script="'<script>alert(111)</script>"
{{ script|safe }}
```

## 五、模板之标签

```
一些在输出中创建文本，一些通过循环或逻辑来控制流程，一些加载其后的变量将使用到的额外信息到模版中。一些标签需要开始和结束标签 （例如{% tag %} ...标签 内容 ... {% endtag %}）。
{% tag %}
```

### 1.内置标签总览

| 标签        | 说明                      |
| ----------- | ------------------------- |
| autoescape  | 自动转义开关              |
| block       | 块引用                    |
| comment     | 注释                      |
| csrf_token  | CSRF令牌                  |
| cycle       | 循环对象的值              |
| debug       | 调试模式                  |
| extends     | 继承模版                  |
| filter      | 过滤功能                  |
| firstof     | 输出第一个不为False的参数 |
| for         | 循环对象                  |
| for … empty | 带empty说明的循环         |
| if          | 条件判断                  |
| ifequal     | 如果等于                  |
| ifnotequal  | 如果不等于                |
| ifchanged   | 如果有变化，则..          |
| include     | 导入子模版的内容          |
| load        | 加载标签和过滤器          |
| lorem       | 生成无用的废话            |
| now         | 当前时间                  |
| regroup     | 根据对象重组集合          |
| resetcycle  | 重置循环                  |
| spaceless   | 去除空白                  |
| templatetag | 转义模版标签符号          |
| url         | 获取url字符串             |
| verbatim    | 禁用模版引擎              |
| widthratio  | 宽度比例                  |
| with        | 上下文变量管理器          |

### 2.常用标签

#### for标签

遍历每一个元素

```django
{% for person in person_list %}
	<p>{{ person }}</p>
{% end for%}
```

遍历一个字典：

```
{% for k,v in dic.items %}
	<p>{{ k }}:{{ v }}</p>
```

下面是Django为for标签内置的一些属性，可以当作变量一样使用`{{ }}`在模版中使用。

- forloop.counter：循环的当前索引值，从1开始计数；常用于生成一个表格或者列表的序号！
- forloop.counter0：循环的当前索引值，从0开始计数；
- forloop.revcounter： 循环结束的次数（从1开始）
- forloop.revcounter0 循环结束的次数（从0开始）
- forloop.first：判断当前是否循环的第一次，是的话，该变量的值为True。我们经常要为第一行加点特殊的对待，就用得上这个判断了，结合if。
- forloop.last：如果这是最后一次循环，则为真
- forloop.parentloop：对于嵌套循环，返回父循环所在的循环次数。某些场景下，这是个大杀器，能解决你很多头疼的问题。

#### for...empty

for标签带有一个可选的`{% empty %}`从句，在给出的组是空的或者没有被找到时执行的操作

**注：循环的对象是空，才会走到empty,而不是对象里面的东西为空**

```django
{% for person in person_list %}
    <p>{{ person.name }}</p>

{% empty %}
    <p>sorry,no person here</p>
{% endfor %}
```

#### if

`{% if %}`会对一个变量求值，如果它的值是“True”（存在、不为空、且不是boolean类型的false值），对应的内容块会输出。

```django
{% if num > 100 or num < 0 %}
    <p>无效</p>
{% elif num > 80 and num < 100 %}
    <p>优秀</p>
{% else %}
    <p>凑活吧</p>
{% endif %}
```

**注：在if标签中使用括号是错误的语法，这点不同于Pythonif语句支持 ，优先级可以使用if嵌套。if支持and 、or、==、>、<、!=、<=、>=、in、not in、is、is not判断。**

#### with

使用一个简单地名字缓存一个复杂的变量，当你需要使用一个“昂贵的”方法（比如访问数据库）很多次的时候是非常有用的

```django
{% with total=business.employees.count %}
    {{ total }} employee{{ total|pluralize }}
{% endwith %}
```

## 六、自定义标签和过滤器

Django为我们提供了自定义的机制，可以通过使用Python代码，自定义标签和过滤器来扩展模板引擎，然后使用{% load %}标签。

### （一）、前置步骤

1. 在settings中的`INSTALLED_APPS`配置当前app，不然django无法找到自定义的simple_tag.

2. 在app中新建一个`templatetags`包(名字固定)，与views.py、models.py等文件在同一目录下

    **注：添加templatetags包后，需要重新启动服务器，然后才能在模板中使用标签或过滤器**

3. 创建任意 .py 文件，如：my_tags.py

4. 要在模块内自定义标签，首先，**这个模块必须包含一个名为register的变量，它是template.Library的一个实例**，所有的标签和过滤器都是在其中注册的。 所以把如下的内容放在你的模块的顶部：

```
from django.template import Library
register = Library()
```

templatetags包中放多少个模块没有限制。只需要记住`{% load xxx %}`将会载入给定模块名中的标签/过滤器，而不是app中所有的标签和过滤器。

### （二）、自定义模板过滤器

#### 1.编写过滤器

自定义过滤器就是一个带有一个或两个参数的python函数

**注：函数的第一个参数是要过滤的对象，第二个是自定义参数，函数一共只能有两个参数。**

- 变量的值：不一定是字符串形式
- 可以有一个初始值，或者不需要这个参数

例子：

```python
def filter_multi(v1,v2):
    return  v1 * v2
```

#### 2.注册过滤器

定义好过滤器函数之后，需要注册，方法是调用`register.filter`

```
register.filter('filter_multi',filter_multi)
```

`Library.filter()`方法需要两个参数：

- 过滤器的名称：一个字符串对象
- 编译的函数 :你刚才写的过滤器函数

还可以把`register.filter()`用作装饰器，以如下的方式注册过滤器：

```
@register.filter(name='yyy')
注：没有声明name参数，Django将使用函数名作为过滤器的名字。
```

#### 3.过滤器的使用方法

1. 在要使用自定义过滤器的模板中导入创建的py文件。

    语法：`{% load 文件名 %}`

2. 要使用的地方。`{{'abc'|yyy:'qwe'}}` 结果输出abcqwe

### （三）、自定义模板标签

除了装饰器，其它步骤与自定义过滤器相似。

```python
@register.simple_tag()
```

**过滤器只能有两个参数，自定义标签可以传多个值，空格传值。**

**注：过滤器可以用在if判断中，标签不能**

## 七、模板继承

模版继承可以让您创建一个基本的“骨架”模版，它包含您站点中的全部元素，并且可以定义能够被子模版覆盖的 blocks 。

### 1.模板导入

语法：{% include'模板名称' %}

如：{% include 'base.html' %}

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <link rel="stylesheet" href="style.css"/>
    <title>{% block title %}My amazing site{% endblock %}</title>
</head>

<body>
<div id="sidebar">
    {% block sidebar %}
        <ul>
            <li><a href="/">Home</a></li>
            <li><a href="/blog/">Blog</a></li>
        </ul>
    {% endblock %}
</div>

<div id="content">
    {% block content %}{% endblock %}
</div>
</body>
</html>
```

它定义了一个可以用于两列排版页面的简单HTML骨架。“子模版”的工作是用它们的内容填充空的blocks。

### 2.子模板中使用

```
语法:
{% block名字 %}
	子模板的内容
{% endblock %}
```

```html
{% block content %}
    <p>这是子的区域</p>
    <p>这是子的区域</p>
    <p>这是子的区域</p>
    <p>这是子的区域</p>
{% endblock content%}
```

**注：**

- 如果在模板中使用`{% extends %}`标签，它**必须**是模板中的第一个标签
- 在base模板中设置越多的`{% block %}`标签越好，子模板不必定义全部父模板中的blocks
- 如果发现有大量的模板存在重复内容，可以定义在父模板的{% block %}中。
- 不能再一个模板中定义多个相同名字的block标签
- 为了更好的可读性，可以给`{% endblock %}`设置一个名字

```django
{% block content %}
{% endblock content%}
```

## 八、静态文件相关

1. 写死静态文件:<link rel="stylesheet" href="/static/css/mycss.css">

2. 使用 static标签函数:

    ```
    {% load static %}
    <link rel="stylesheet" href="{% static 'css/mycss.css' %}">
    ```

3. 使用get_static_prefix 标签

    ```
    {% load static %}
    <link rel="stylesheet" href="{% get_static_prefix %}css/mycss.css">
    ```
