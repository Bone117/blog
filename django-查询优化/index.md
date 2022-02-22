# Django-查询优化

表数据：

```python
from django.db import models
 
class Province(models.Model):
    name = models.CharField(max_length=10)
    def __str__(self):
        return self.name
 
class City(models.Model):
    name = models.CharField(max_length=5)
    province = models.ForeignKey(Province)
    def __str__(self):
        return self.name
 
class Person(models.Model):
    firstname  = models.CharField(max_length=10)
    lastname   = models.CharField(max_length=10)
    visitation = models.ManyToManyField(City, related_name = "visitor")
    hometown   = models.ForeignKey(City, related_name = "birth")
    living     = models.ForeignKey(City, related_name = "citizen")
    def __str__(self):
        return self.firstname + self.lastname
```

## 一、select_related

> 对于一对一字段（OneToOneField）和外键字段（ForeignKey），可以使用select_related 来对QuerySet进行优化
>
> 在对QuerySet使用select_related()函数后，Django会获取**相应外键对应的对象**，从而在之后需要的时候不必再查询数据库了。

### 简单查询：

```python
citys = City.objects.all()
for c in citys:
   print (c.province)
```

> 执行上面的语句会导致SQL多次查询

#### SQL

```mysql
SELECT `QSOptimize_city`.`id`, `QSOptimize_city`.`name`, `QSOptimize_city`.`province_id`
FROM `QSOptimize_city`
 
SELECT `QSOptimize_province`.`id`, `QSOptimize_province`.`name` 
FROM `QSOptimize_province`
WHERE `QSOptimize_province`.`id` = 1 ;
 
SELECT `QSOptimize_province`.`id`, `QSOptimize_province`.`name` 
FROM `QSOptimize_province`
WHERE `QSOptimize_province`.`id` = 2 ;
 
SELECT `QSOptimize_province`.`id`, `QSOptimize_province`.`name` 
FROM `QSOptimize_province`
WHERE `QSOptimize_province`.`id` = 1 ;
```

### 使用select_related:

```python
citys = City.objects.select_related().all()
for c in citys:
  print (c.province)
```

> 只有一次SQL查询，大大减少了SQL查询的次数

#### SQL

```mysql
SELECT `QSOptimize_city`.`id`, `QSOptimize_city`.`name`, 
`QSOptimize_city`.`province_id`, `QSOptimize_province`.`id`, `QSOptimize_province`.`name` 
FROM`QSOptimize_city` 
INNER JOIN `QSOptimize_province` ON (`QSOptimize_city`.`province_id` = `QSOptimize_province`.`id`) ;
```

> Django使用了INNER JOIN来完成请求

### 多外键查询

> select_related() 接受可变长参数，每个参数是**需要获取的外键**的字段名，以及外键的外键的字段名、外键的外键的外键...。若要选择外键的外键需要使用两个下划线“__”来连接。

**注：未指定的外键则不会被添加到结果中**

1.7以前

```python
zhangs=Person.objects.select_related('hometown__province','living__province')
.get(firstname="张",lastname="三")
zhangs.hometown.province
zhangs.living.province
```

1.7以后支持链式操作

```python
zhangs=Person.objects.select_related('hometown__province')
.select_related('living__province').get(firstname="张",lastname="三")
zhangs.hometown.province
zhangs.living.province

```

### 深度查询depth

> Django会递归遍历指定深度内的所有的OneToOneField和ForeignKey。以本例说明：

```python
zhangs = Person.objects.select_related(depth = d)
# d=1  相当于 select_related('hometown','living')
# d=2  相当于 select_related('hometown__province','living__province')
```

### 无参数

> 这样表示要求Django尽可能深的select_related()
>
> **注：Django并不知道你实际要用的字段有哪些，所以会把所有的字段都抓进来，从而会造成不必要的浪费而影响性能**

### 小结：

> 1. 主要针一对一和多对一关系进行优化
> 2. 使用SQL的**JOIN语句**进行优化，通过减少SQL查询的次数来进行优化、提高性能。
> 3. 通过**可变长参数**指定需要select_related的字段名。也可以通过使用**双下划线**“__”连接字段名来实现指定的递归查询。
> 4. 没有指定的字段不会缓存，没有指定的深度不会缓存，如果要访问的话Django会再次进行SQL查询。
> 5. 也可以通过depth参数指定递归的深度，Django会自动缓存指定深度内所有的字段。如果要访问指定深度外的字段，Django会再次进行SQL查询。
> 6. 接受无参数的调用，Django会尽可能深的递归查询所有的字段。但注意有Django递归的限制和性能的浪费。
> 7. Django >= 1.7，链式调用的select_related相当于使用可变长参数。Django < 1.7，链式调用会导致前边的select_related失效，只保留最后一个。

## 二、prefetch_related

> 对于多对多字段（ManyToManyField）和一对多字段，可以使用prefetch_related()来进行优化。或许你会说，没有一个叫OneToManyField的东西啊。实际上 ，ForeignKey就是一个多对一的字段，而被ForeignKey关联的字段就是一对多字段了。
>
> prefetch_related()和select_related()都是为了减少SQL查询的数量，但是实现的方式不一样。后者是通过**JOIN**语句，在SQL查询内解决问题。但是对于多对多关系，使用SQL语句JOIN得到的表将会很长，会导致SQL语句运行时间的增加和内存占用的增加。prefetch_related()的解决方法是，**分别查询每个表**，然后用Python处理他们之间的关系

```python
zhangs = Person.objects.prefetch_related('visitation').get(firstname="张",lastname="三")
for city in zhangs.visitation.all() :
  print(city) 

```

### Prefetch 对象

> 1. 一个Prefetch对象只能指定一项prefetch操作。
> 2. Prefetch对象对字段指定的方式和prefetch_related中的参数相同，都是通过双下划线连接的字段名完成的。
> 3. 可以通过 queryset 参数手动指定prefetch使用的QuerySet。
>     可以通过 to_attr 参数指定prefetch到的属性名。
> 4. Prefetch对象和字符串形式指定的lookups参数可以混用。

### 小结：

> 1. prefetch_related主要针一对多和多对多关系进行优化。
> 2. prefetch_related通过分别获取各个表的内容，然后用Python处理他们之间的关系来进行优化。
> 3. 可以通过可变长参数指定需要select_related的字段名。指定方式和特征与select_related是相同的。
> 4. 在Django >= 1.7可以通过Prefetch对象来实现复杂查询。
> 5. 作为prefetch_related的参数，Prefetch对象和字符串可以混用。
> 6. prefetch_related的链式调用会将对应的prefetch添加进去，而非替换，似乎没有基于不同版本上区别。
> 7. 可以通过传入None来清空之前的prefetch_related。

参考：https://blog.csdn.net/cugbabybear/article/details/38342793
