# 多表操作

## 多表操作

**数据准备**

```python
class Publish(models.Model):
    id = models.AutoField(primary_key=True)
    name = models.CharField(max_length=32)
    addr=models.CharField(max_length=64)
    email=models.EmailField
    def __str__(self):
        return self.name

class Author(models.Model):
    id = models.AutoField(primary_key=True)
    name=models.CharField(max_length=32)
    sex=models.IntegerField()
    authordetail=models.OneToOneField(to='AuthorDetail',to_field='id')
    def __str__(self):
        return self.name

class AuthorDetail(models.Model):
    id=models.AutoField(primary_key=True)
    phone = models.CharField(max_length=32)
    addr = models.CharField(max_length=64)


class Book(models.Model):
    id = models.AutoField(primary_key=True)
    name = models.CharField(max_length=32)
    price = models.DecimalField(max_digits=5, decimal_places=2)
    create_time = models.DateField(null=True)

    publish=models.ForeignKey(to='Publish',to_field='id')
    authors=models.ManyToManyField(to='Author')
    def __str__(self):
        return self.name
-------------------------------------------------------    
# 一对一的关系:OneToOneField      模型表的字段,后面会自定加_id
# 一对多的关系:ForeignKey         模型表的字段,后面会自定加_id
# 多对多的关系:ManyToManyField    ManyToManyField会自动创建第三张表
```

### 一、一对多

#### 1.添加数据

方式一：

```python
pub=Publish.objects.get(pk=1)  #publish=出版社的对象,存到数据库,是一个id
Book.objects.create(name='金庸群侠传',price=100,create_time='2018-1-1',publish=pub)
```

方式二：

```python
book=Book.objects.create(name='红楼梦',price=34.5,create_time='2017-1-1',publish_id=1)
#用了OneToOneField和ForeignKey,模型表的字段,后面会自定加_id
```

#### 2.修改数据

方式一：

```python
#修改出版社
book=Book.object.get(pk=1)
book.publish_id=2
book.save()
```

方式二：

```python
book=Book.objects.filter(pk=1).update(publish_id=2)
或
pub=Publish.objects.get(pk=2)
book=Book.objects.filter(pk=1).update(publish=pub)
```

#### 3.删除数据

**同单表删除**

方式一：

```python
ret=models.Book.objects.filter(name='红楼梦').delete()
# ret返回的是被影响行数，queryset对象的.delete()方法
如果存在多本红楼梦将会被全部删除
```

方式二：

```python
ret=models.Book.objects.filter(name='红楼梦').first()
ret.delete()
只删除一个
```

### 二、多对多

#### 1.添加数据

```python
a=Author.objects.filter(name='虚竹').first()
b=Author.objects.filter(name='段誉').first()
book=Book.objects.filter(name='天龙八部').first()
#添加过个作者对象
book.authors.add(a,b)
#或者   添加多个作者id
book.authors.add(1,2)
```

#### 2.删除remove,可以传对象，可以传id，可以传多个，不要混着用

```python
book.authors.remove(a)
book.authors.remove(1,2)
```

#### 3.clear清空所有

```python
book.authors.clear()
```

#### 4.set,先清空所有，再新增，要传一个列表，列表内可以是id，也可以是对象

```python
book.authors.set([a,b])
```

### 三、基于对象的跨表查询

```
基于对象的查询是子查询也就是多次查询
```

#### 1.一对一

```
正向  author-----关联字段在author----->authordetail------>按字段
反向  authordetail----->关联字段在author---->author------>按表名小写
```

**例子**   

正向查询：查询abc作者的手机号

```python
author=Author.objects.filter(name='abc').first()
authordetail=author.authordetail
# author.authordetail 就是作者详情的对象----这里的authordetail是字段名
print(authordetail.phone)
```

反向查询：查询地址在上海的作者名字

```python
authordetail=AuthorDetail.objects.filter(addr='上海').first()
author=authordetail.author
# authordetail.author  这是作者对象
print(author.name)
```

#### 2.一对多

```
正向： book----关键字段在book--->publish---->按字段
反向: publish---->关键字段在book--->book---->按表名小写_set.all()
```

正向查询：查询红楼梦这本书的出版社地址

```python
book=Book.objects.filter(name='红楼梦').first()
pub=book.publish
#book.publish  出版社对象
print(pub.addr)
```

反向查询：查询地址是上海的出版社的出版图书

```python
pub=Publish.objects.filter(addr='上海').first()
books=pub.book_set.all()
```

#### 3.多对多

```python
正向： book---关联字段在book--->author--->按字段.all()
反向： author---关联字段在book--->book---->按表名小写_set.all()
```

正向查询：查询红楼梦这本书的所有作者

```python
book=Book.objects.filter(name='红楼梦').first()
authors=book.authors.all()
```

反向查询：查询B写的所有书

```python
b=Author.objects.filter(name='B').first()
books=b.book_set.all()
```

#### 4.连续跨表

查询红楼梦这本书所有的作者的手机号

```python
book=Book.objects.filter(name='红楼梦')
authors=book.author.all()
for author in authors:
    author_detail=author.authordetail
    print(authordetail.phone)
```

### 四、基于双下划线查询

#### 1.一对一

```python
-正向:按字段,跨表可以在filter,也可以在values中    按字段
-反向:按表名小写,跨表可以在filter,也可以在values中    按表名小写
```

正向查询：查询作者B的手机号 

（以author表作为基表）

```python
ret=Author.object.filter(name='B').values('authordetail__phone')
print(ret)
```

反向查询：

```python
ret=AuthorDetail.object.filter(author__name='B').values('phone')
```

#### 2.一对多

正向查询：

```
#查询出版社为北京出版社出版的所有图书的名字,价格
Book.objects.filter(publish_name='北京出版社').values('name','price')
```

反向查询：

```django
Publish.objects.filter(name='北京出版社').values('book_name','book_price')
```

#### 3.多对多

正向查询：

```django
查询红楼梦的所有作者名字
Book.objects.filter(name='红楼梦').values('authors_name')
```

反向查询：

```
Author.objects.filter(book__name='红楼梦').values('name')
```

#### 4.连续跨表

查询北京出版社出版过的所有书籍的名字以及作者的姓名

```django
Publish.objects.filter('北京出版社').values('book__name','book_author__name')
```

```
Book.object.filter(publish__name='北京出版社').values('name','author__name')
```

### 五、聚合查询

`aggregate`(*args, **kwargs)

```
aggregate()是QuerySet 的一个终止子句,返回一个包含一些键值对的字典
可以向aggregate()子句中添加另一个参数,生成不止一个聚合
```

```python
from django.db.models import Avg,Count,Max,Min,Sum
#1.计算所有图书的平均价格
Book.objects.all().aggregate(Avg('price'),Max('price'))
```

### 六、分组查询

```
annotate()为调用的QuerySet中每个对象生成独立的统计值。跨表分组查询本质就是将关联表join成一张表，再按单表的思路进行分组查询
```

```django
#统计每一本书作者个数
Book.objects.all().annotate(s=Count('author')).values('name','c')
```

```django
#查询所有作者写的书的总价格大于30的作者
Author.objects.value('pk')annotate(s=Sum('book_price')).filter(s_gt=30).values('name','s')
或者
Author.objects.annotate(s=Sum('book_price)).filter(s_gt=30).values('name''s') #默认按id主键分组
```

**注：**

```
values在annotate前表示分组，在后表示取值
filter在annotate前表示where条件，在后表示having
```

### 七、F查询

```
对两个字段的值做比较
F() 的实例可以在查询中引用字段，来比较同一个 model 实例中两个不同字段的值。
```

例如book的model中再添加两个字段，评论数和阅读数.

查询评论数大于阅读数的书

```django
ret=Book.objects.all().filter(commit_num__gt=read_num) #错误
正确如下：
ret=Book.objects.all().filter(commit_num_gt=F('read_num'))
```

把所有书的评论数加1

```
ret=Book.objects.all().update(commit_num=F('commit_num')+1)
```

### 八、Q查询

```
filter()等方法进行的是'AND'，更复杂的查询需要Q查询
表示与& ,或 | ,非 ~
```

```django
#查询是作者A也或者是作者B的书
Book.object.all().filter(Q(author_name='A')|Q(author_name='B'))
```

**补充：**

```
Blog.objects.filter(entry__headline__contains='Lennon').filter(entry__pub_date__year=2008)
```

把两个参数拆开，放在两个filter调用里面，Django在这种情况下，将两个filter之间的关系设计为-或“or”
