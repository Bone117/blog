# Django-ORM-单表操作

## ORM字段参数及单表操作

### 一、字段参数

#### 1.字段

```
AutoField(Field)  #当model中如果没有自增列，则会自动创建一个列名为id的列
	-int 自增列，必须填入参数primary_key=True
SmallIntegerField(IntegerField):
    - 小整数 -32768 ～ 32767
PositiveSmallIntegerField(PositiveIntegerRelDbTypeMixin, IntegerField)
    - 正小整数 0 ～ 32767
IntegerField(Field)
    - 整数列(有符号的) -2147483648 ～ 2147483647
PositiveIntegerField(PositiveIntegerRelDbTypeMixin, IntegerField)
    - 正整数 0 ～ 2147483647
BigIntegerField(IntegerField):
    - 长整型(有符号的) -9223372036854775808 ～ 9223372036854775807
BooleanField(Field)
    - 布尔值类型
NullBooleanField(Field):
    - 可以为空的布尔值
CharField(Field)
    - 字符类型
    - 必须提供max_length参数， max_length表示字符长度
TextField(Field)
    - 文本类型
EmailField(CharField)：
    - 字符串类型，Django Admin以及ModelForm中提供验证机制
IPAddressField(Field)
    - 字符串类型，Django Admin以及ModelForm中提供验证 IPV4 机制
GenericIPAddressField(Field)
    - 字符串类型，Django Admin以及ModelForm中提供验证 Ipv4和Ipv6
    - 参数：
        protocol，用于指定Ipv4或Ipv6， 'both',"ipv4","ipv6"
        unpack_ipv4， 如果指定为True，则输入::ffff:192.0.2.1时候，可解析为192.0.2.1，开启刺功能，需要protocol="both"
URLField(CharField)
    - 字符串类型，Django Admin以及ModelForm中提供验证 URL
SlugField(CharField)
    - 字符串类型，Django Admin以及ModelForm中提供验证支持 字母、数字、下划线、连接符（减号）
CommaSeparatedIntegerField(CharField)
    - 字符串类型，格式必须为逗号分割的数字
UUIDField(Field)
    - 字符串类型，Django Admin以及ModelForm中提供对UUID格式的验证
FilePathField(Field)
    - 字符串，Django Admin以及ModelForm中提供读取文件夹下文件的功能
    - 参数：
            path,                      文件夹路径
            match=None,                正则匹配
            recursive=False,           递归下面的文件夹
            allow_files=True,          允许文件
            allow_folders=False,       允许文件夹
FileField(Field)
    - 字符串，路径保存在数据库，文件上传到指定目录
    - 参数：
       upload_to = ""      上传文件的保存路径
       storage = None      存储组件，默认django.core.files.storage.FileSystemStorage

ImageField(FileField)
    - 字符串，路径保存在数据库，文件上传到指定目录
    - 参数：
        upload_to = ""      上传文件的保存路径
        storage = None      存储组件，默认django.core.files.storage.FileSystemStorage
        width_field=None,   上传图片的高度保存的数据库字段名（字符串）
        height_field=None   上传图片的宽度保存的数据库字段名（字符串）

DateTimeField(DateField)
    - 日期+时间格式 YYYY-MM-DD HH:MM[:ss[.uuuuuu]][TZ]

DateField(DateTimeCheckMixin, Field)
    - 日期格式      YYYY-MM-DD

TimeField(DateTimeCheckMixin, Field)
    - 时间格式      HH:MM[:ss[.uuuuuu]]

DurationField(Field)
    - 长整数，时间间隔，数据库中按照bigint存储，ORM中获取的值为datetime.timedelta类型

FloatField(Field)
    - 浮点型

DecimalField(Field)
    - 10进制小数
    - 参数：
        max_digits，小数总长度
        decimal_places，小数位长度

BinaryField(Field)
    - 二进制类型
```

#### 2.参数

```
1.null
如果未True,Django将用NULL来在数据库中存储空值，默认为false

2.blank
如果未True，该字段允许为空，默认为False。
注：与null不同。null是数据库范畴，blank是数据验证

3.default
字段默认值

4.primary_key
如果未True，那么这个字段就是模型的主键。如果没有指定，Django会自动添加一个IntegerField字段作为主键

5.unique
该字段的值在表中唯一

6.choices
由二元组组成的一个可迭代对象（例如，列表或元组），用来给字段提供选择项。 如果设置了choices ，默认的表单将是一个选择框而不是标准的文本框，<br>而且这个选择框的选项就是choices 中的选项。
```

**补充：**

1. **数据库迁移记录都在 app下的migrations里**
2. **使用showmigrations命令可以查看没有执行migrate的文件**
3. **makemigrations是生成一个文件，migrate是将更改提交到数据量**

### 二、单表操作

#### 1.添加表记录

方式一：返回结果是一个对象

```
# create方法的返回值book_obj就是插入book表中的红楼梦这本书籍纪录对象
book_obj=models.Book.objects.create(name='红楼梦',price=23.8,publish='人民出版社',author='曹雪芹',create_data='2018-09-17')
```

方式二：先实例化产生对象，然后调用save方法保存

```
book_obj=models.Book(name='水浒传',price=99,publish='清华出版社',author='施耐庵',create_data='2017-09-17')
book_obj.save()
```

#### 2.删除表记录

删除前要先查询

方式一：

```
ret=models.Book.objects.filter(name='红楼梦').delete()
# ret返回的是被影响行数，queryset对象的.delete()方法
如果存在多本红楼梦将会被全部删除
```

方式二：

```
ret=models.Book.objects.filter(name='红楼梦').first()
ret.delete()
只删除一个
```

#### 3.修改表记录

方式一：

```
ret=models.Book.objects.filter(name='红楼梦').update(price=20.9)
```

方式二：

```
对象没有update方法，但是可以用save来修改
book=models.Book.objects.filter(name='红楼梦').first()
book.price=33
book.save()
```

#### 4.查询表记录

```
1.all() 查询所有结果 
models.Book.objects.all()

2.filter(**kwargs) 指定条件查询   filter内可传多个参数，逗号分隔，他们之间是and关系
models.Book.objects.filter(name='红楼梦').first()
或者models.Book.objects.filter(name='红楼梦')[0]
注：不支持负数

3.get(**kwargs) 有且只有一个结果,返回的是对象，不是queryset对象，通常使用id查询 
ret=Book.objects.get(name='红楼梦') 不存在或超过一个会报错

4.exclude(**kwargs) 返回与筛选对象不匹配的对象
models.Book.objects.exclude(price>40)

5.order_by() 对查询结果排序
models.Book.objects.all().order_by('price') 默认升序
倒序：models.Book.objects.all().order_by('-price')
可以传多个参数，逗号隔开。

6.reverse() 对查询结果反向排序

7.count()查询结果的个数

8.first()返回第一条记录   last()返回最后一条记录

9.exists()如果queryset包含数据就返回True，否则返回False
models.Book.objects.filter(name='红楼梦').exists()

10.values()  queryset对象里套字典

11.values_list() 返回元组序列，values返回的是一个字典序列

12.distinct()从结果中剔除重复记录

-----------------------------------------------------------
基于双下划线的模糊查询
1.大于__gt
models.Book.objects.filter(price__gt='89')

2.小于__lt=
models.Book.objects.filter(price__lt='99')

3.小于等于__lte=
models.Book.objects.filter(price__lte='99')

4.大于等于__gte
models.Book.objects.filter(price__gte='89')

5.in在XX中
ret=models.Book.objects.filter(price__in=['23.8','89','100'])

6.range 在xx范围内 between and
ret=models.Book.objects.filter(price__range=[50,100])

7.contains 查询名字带有 红 字的书
models.Book.objects.filter(name__contains='红')

8.icontains 查询名字带p的书，忽略大小写
models.Book.objects.filter(name__icontains='P')

9.startswith 以XX开头
models.Book.objects.filter(name__startswith='红')

10.endwith 以xx结尾
models.Book.objects.filter(name__endwith='梦')

11.__year按年查询
models.Book.objects.filter(create_data_year='2018')
```
