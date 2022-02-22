# mysql-约束介绍

## 一、约束介绍

约束是一种限制，它通过对表的行或列的数据做出限制，来确保数据的完整性、一致性。约束条件与数据类型宽度一样都是可选参数。
常用约束：

```
<span style="color: #0000ff;">PRIMARY</span> <span style="color: #0000ff;">KEY</span><span style="color: #000000;"> (PK)    标识该字段为该表的主键，可以唯一的标识记录
</span><span style="color: #0000ff;">FOREIGN</span> <span style="color: #0000ff;">KEY</span><span style="color: #000000;"> (FK)    标识该字段为该表的外键
</span><span style="color: #808080;">NOT</span> <span style="color: #0000ff;">NULL</span><span style="color: #000000;">    标识该字段不能为空
</span><span style="color: #0000ff;">UNIQUE</span> <span style="color: #0000ff;">KEY</span><span style="color: #000000;"> (UK)    标识该字段的值是唯一的
AUTO_INCREMENT    标识该字段的值自动增长（整数类型，而且为主键）
</span><span style="color: #0000ff;">DEFAULT</span><span style="color: #000000;">    为该字段设置默认值

UNSIGNED 无符号
ZEROFILL 使用0填充</span>
```

### （一）、not null与default

not null 用于约束列不允许为空
null 列的默认约束为null  允许为空
default 默认值，创建列时可以指定其默认值，插入数据为设置时，自动添加为默认值。
![](https://img2018.cnblogs.com/blog/1285461/201809/1285461-20180913194436327-2144216470.png)

![](http://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif)![](http://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif)

```
mysql<span style="color: #808080;">></span> <span style="color: #0000ff;">create</span> <span style="color: #0000ff;">table</span> test1(id <span style="color: #0000ff;">int</span> <span style="color: #808080;">not</span> <span style="color: #0000ff;">null</span>,name <span style="color: #0000ff;">varchar</span>(<span style="color: #800000; font-weight: bold;">11</span>) <span style="color: #0000ff;">default</span> <span style="color: #ff0000;">'</span><span style="color: #ff0000;">aaa</span><span style="color: #ff0000;">'</span><span style="color: #000000;">);
Query OK, </span><span style="color: #800000; font-weight: bold;">0</span> rows affected (<span style="color: #800000; font-weight: bold;">0.26</span><span style="color: #000000;"> sec)

mysql</span><span style="color: #808080;">></span> <span style="color: #0000ff;">insert</span> <span style="color: #0000ff;">into</span> test1 value(<span style="color: #0000ff;">null</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">abc</span><span style="color: #ff0000;">'</span><span style="color: #000000;">);
ERROR </span><span style="color: #800000; font-weight: bold;">1048</span> (<span style="color: #800000; font-weight: bold;">23000</span>): <span style="color: #0000ff;">Column</span> <span style="color: #ff0000;">'</span><span style="color: #ff0000;">id</span><span style="color: #ff0000;">'</span> cannot be <span style="color: #0000ff;">null</span><span style="color: #000000;">
mysql</span><span style="color: #808080;">></span> <span style="color: #0000ff;">insert</span> <span style="color: #0000ff;">into</span> test1(id) value(<span style="color: #800000; font-weight: bold;">1</span><span style="color: #000000;">);
Query OK, </span><span style="color: #800000; font-weight: bold;">1</span> row affected (<span style="color: #800000; font-weight: bold;">0.29</span><span style="color: #000000;"> sec)

mysql</span><span style="color: #808080;">></span> <span style="color: #0000ff;">select</span> <span style="color: #808080;">*</span> <span style="color: #0000ff;">from</span><span style="color: #000000;"> test1;
</span><span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">--+------+</span>
<span style="color: #808080;">|</span> id <span style="color: #808080;">|</span> name <span style="color: #808080;">|</span>
<span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">--+------+</span>
<span style="color: #808080;">|</span>  <span style="color: #800000; font-weight: bold;">1</span> <span style="color: #808080;">|</span> aaa  <span style="color: #808080;">|</span>
<span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">--+------+</span>
<span style="color: #800000; font-weight: bold;">1</span> row <span style="color: #808080;">in</span> <span style="color: #0000ff;">set</span> (<span style="color: #800000; font-weight: bold;">0.00</span> sec)
```

test

### （二）、unique

设置唯一约束，当你需要限定你的某个表字段每个值都唯一,没有重复值时使用。
允许为空
![](https://img2018.cnblogs.com/blog/1285461/201809/1285461-20180913195129692-2045246650.png)

![](http://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif)![](http://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif)

```
mysql<span style="color: #808080;">></span> <span style="color: #0000ff;">create</span> <span style="color: #0000ff;">table</span> test2(id <span style="color: #0000ff;">int</span> <span style="color: #808080;">not</span> <span style="color: #0000ff;">null</span> ,name <span style="color: #0000ff;">varchar</span>(<span style="color: #800000; font-weight: bold;">11</span>),phone <span style="color: #0000ff;">int</span>(<span style="color: #800000; font-weight: bold;">11</span>) <span style="color: #0000ff;">unique</span><span style="color: #000000;">);
Query OK, </span><span style="color: #800000; font-weight: bold;">0</span> rows affected (<span style="color: #800000; font-weight: bold;">0.50</span><span style="color: #000000;"> sec)

mysql</span><span style="color: #808080;">></span> <span style="color: #0000ff;">desc</span><span style="color: #000000;"> test2;
</span><span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">-----+-------------+------+-----+---------+-------+</span>
<span style="color: #808080;">|</span> Field <span style="color: #808080;">|</span> Type        <span style="color: #808080;">|</span> <span style="color: #0000ff;">Null</span> <span style="color: #808080;">|</span> <span style="color: #0000ff;">Key</span> <span style="color: #808080;">|</span> <span style="color: #0000ff;">Default</span> <span style="color: #808080;">|</span> Extra <span style="color: #808080;">|</span>
<span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">-----+-------------+------+-----+---------+-------+</span>
<span style="color: #808080;">|</span> id    <span style="color: #808080;">|</span> <span style="color: #0000ff;">int</span>(<span style="color: #800000; font-weight: bold;">11</span>)     <span style="color: #808080;">|</span> NO   <span style="color: #808080;">|</span>     <span style="color: #808080;">|</span> <span style="color: #0000ff;">NULL</span>    <span style="color: #808080;">|</span>       <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span> name  <span style="color: #808080;">|</span> <span style="color: #0000ff;">varchar</span>(<span style="color: #800000; font-weight: bold;">11</span>) <span style="color: #808080;">|</span> YES  <span style="color: #808080;">|</span>     <span style="color: #808080;">|</span> <span style="color: #0000ff;">NULL</span>    <span style="color: #808080;">|</span>       <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span> phone <span style="color: #808080;">|</span> <span style="color: #0000ff;">int</span>(<span style="color: #800000; font-weight: bold;">11</span>)     <span style="color: #808080;">|</span> YES  <span style="color: #808080;">|</span> UNI <span style="color: #808080;">|</span> <span style="color: #0000ff;">NULL</span>    <span style="color: #808080;">|</span>       <span style="color: #808080;">|</span>
<span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">-----+-------------+------+-----+---------+-------+</span>
<span style="color: #800000; font-weight: bold;">3</span> rows <span style="color: #808080;">in</span> <span style="color: #0000ff;">set</span> (<span style="color: #800000; font-weight: bold;">0.00</span><span style="color: #000000;"> sec)
mysql</span><span style="color: #808080;">></span> <span style="color: #0000ff;">insert</span> <span style="color: #0000ff;">into</span> test2 value(<span style="color: #800000; font-weight: bold;">1</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">abc</span><span style="color: #ff0000;">'</span>,<span style="color: #800000; font-weight: bold;">111111</span><span style="color: #000000;">);
Query OK, </span><span style="color: #800000; font-weight: bold;">1</span> row affected (<span style="color: #800000; font-weight: bold;">0.29</span><span style="color: #000000;"> sec)

mysql</span><span style="color: #808080;">></span> <span style="color: #0000ff;">insert</span> <span style="color: #0000ff;">into</span> test2 value(<span style="color: #800000; font-weight: bold;">1</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">abc</span><span style="color: #ff0000;">'</span>,<span style="color: #800000; font-weight: bold;">111111</span><span style="color: #000000;">);
ERROR </span><span style="color: #800000; font-weight: bold;">1062</span> (<span style="color: #800000; font-weight: bold;">23000</span>): Duplicate entry <span style="color: #ff0000;">'</span><span style="color: #ff0000;">111111</span><span style="color: #ff0000;">'</span> <span style="color: #0000ff;">for</span> <span style="color: #0000ff;">key</span> <span style="color: #ff0000;">'</span><span style="color: #ff0000;">phone</span><span style="color: #ff0000;">'</span><span style="color: #000000;">
mysql</span><span style="color: #808080;">></span>
```

test

联合唯一：（多个唯一）
![](https://img2018.cnblogs.com/blog/1285461/201809/1285461-20180913195843100-1340575921.png)

![](http://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif)![](http://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif)

```
mysql<span style="color: #808080;">></span> <span style="color: #0000ff;">create</span> <span style="color: #0000ff;">table</span> test3 (id <span style="color: #0000ff;">int</span> <span style="color: #808080;">not</span> <span style="color: #0000ff;">null</span>,name <span style="color: #0000ff;">varchar</span>(<span style="color: #800000; font-weight: bold;">11</span>),phone <span style="color: #0000ff;">int</span>(<span style="color: #800000; font-weight: bold;">11</span>),<span style="color: #0000ff;">unique</span><span style="color: #000000;">(id,phone));
Query OK, </span><span style="color: #800000; font-weight: bold;">0</span> rows affected (<span style="color: #800000; font-weight: bold;">0.47</span><span style="color: #000000;"> sec)
mysql</span><span style="color: #808080;">></span> <span style="color: #0000ff;">insert</span> <span style="color: #0000ff;">into</span> test3 value(<span style="color: #800000; font-weight: bold;">1</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">aaa</span><span style="color: #ff0000;">'</span>,<span style="color: #800000; font-weight: bold;">123</span><span style="color: #000000;">);
Query OK, </span><span style="color: #800000; font-weight: bold;">1</span> row affected (<span style="color: #800000; font-weight: bold;">0.29</span><span style="color: #000000;"> sec)

mysql</span><span style="color: #808080;">></span> <span style="color: #0000ff;">insert</span> <span style="color: #0000ff;">into</span> test3 value(<span style="color: #800000; font-weight: bold;">1</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">bbb</span><span style="color: #ff0000;">'</span>,<span style="color: #800000; font-weight: bold;">123</span><span style="color: #000000;">);
ERROR </span><span style="color: #800000; font-weight: bold;">1062</span> (<span style="color: #800000; font-weight: bold;">23000</span>): Duplicate entry <span style="color: #ff0000;">'</span><span style="color: #ff0000;">1-123</span><span style="color: #ff0000;">'</span> <span style="color: #0000ff;">for</span> <span style="color: #0000ff;">key</span> <span style="color: #ff0000;">'</span><span style="color: #ff0000;">id</span><span style="color: #ff0000;">'</span><span style="color: #000000;">
mysql</span><span style="color: #808080;">></span> <span style="color: #0000ff;">insert</span> <span style="color: #0000ff;">into</span> test3 value(<span style="color: #800000; font-weight: bold;">1</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">bbb</span><span style="color: #ff0000;">'</span>,<span style="color: #800000; font-weight: bold;">555</span><span style="color: #000000;">);
Query OK, </span><span style="color: #800000; font-weight: bold;">1</span> row affected (<span style="color: #800000; font-weight: bold;">0.29</span> sec)
```

test

### （四）、primary key

站在约束角度看primary key=not null unique。主键primary key是innodb存储引擎组织数据的依据，innodb称之为索引组织表，一张表中必须有且只有一个主键。
主键约束列不允许重复，也不允许出现空值。每个表最多只允许一个主键，建立主键约束可以在列级别创建，也可以在表级别创建。当创建主键的约束时，系统默认会在所在的列和列组合上建立对应的唯一索引。（通常id字段被设置为主键）

```
<span style="color: #008080;">--</span><span style="color: #008080;"> 基本模式</span>
<span style="color: #0000ff;">create</span> <span style="color: #0000ff;">table</span> <span style="color: #0000ff;">temp</span><span style="color: #000000;">( 
id </span><span style="color: #0000ff;">int</span> <span style="color: #0000ff;">primary</span> <span style="color: #0000ff;">key</span><span style="color: #000000;">,
name </span><span style="color: #0000ff;">varchar</span>(<span style="color: #800000; font-weight: bold;">20</span><span style="color: #000000;">)
);

</span><span style="color: #008080;">--</span><span style="color: #008080;"> 组合模式</span>
<span style="color: #0000ff;">create</span> <span style="color: #0000ff;">table</span> <span style="color: #0000ff;">temp</span><span style="color: #000000;">(
id </span><span style="color: #0000ff;">int</span><span style="color: #000000;"> ,
name </span><span style="color: #0000ff;">varchar</span>(<span style="color: #800000; font-weight: bold;">20</span><span style="color: #000000;">),
pwd </span><span style="color: #0000ff;">varchar</span>(<span style="color: #800000; font-weight: bold;">20</span><span style="color: #000000;">),
</span><span style="color: #0000ff;">primary</span> <span style="color: #0000ff;">key</span><span style="color: #000000;">(id, name)
);</span>
```

### （五）、auto_increment

 约束字段为自动增长

![](http://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif)![](http://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif)

```
<span style="color: #000000;">#不指定id，则自动增长
</span><span style="color: #0000ff;">create</span> <span style="color: #0000ff;">table</span><span style="color: #000000;"> student(
id </span><span style="color: #0000ff;">int</span> <span style="color: #0000ff;">primary</span> <span style="color: #0000ff;">key</span><span style="color: #000000;"> auto_increment,
name </span><span style="color: #0000ff;">varchar</span>(<span style="color: #800000; font-weight: bold;">20</span><span style="color: #000000;">),
sex enum(</span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">male</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">female</span><span style="color: #ff0000;">'</span>) <span style="color: #0000ff;">default</span> <span style="color: #ff0000;">'</span><span style="color: #ff0000;">male</span><span style="color: #ff0000;">'</span><span style="color: #000000;">
);

mysql</span><span style="color: #808080;">></span> <span style="color: #0000ff;">desc</span><span style="color: #000000;"> student;
</span><span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">-----+-----------------------+------+-----+---------+----------------+</span>
<span style="color: #808080;">|</span> Field <span style="color: #808080;">|</span> Type                  <span style="color: #808080;">|</span> <span style="color: #0000ff;">Null</span> <span style="color: #808080;">|</span> <span style="color: #0000ff;">Key</span> <span style="color: #808080;">|</span> <span style="color: #0000ff;">Default</span> <span style="color: #808080;">|</span> Extra          <span style="color: #808080;">|</span>
<span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">-----+-----------------------+------+-----+---------+----------------+</span>
<span style="color: #808080;">|</span> id    <span style="color: #808080;">|</span> <span style="color: #0000ff;">int</span>(<span style="color: #800000; font-weight: bold;">11</span>)               <span style="color: #808080;">|</span> NO   <span style="color: #808080;">|</span> PRI <span style="color: #808080;">|</span> <span style="color: #0000ff;">NULL</span>    <span style="color: #808080;">|</span> auto_increment <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span> name  <span style="color: #808080;">|</span> <span style="color: #0000ff;">varchar</span>(<span style="color: #800000; font-weight: bold;">20</span>)           <span style="color: #808080;">|</span> YES  <span style="color: #808080;">|</span>     <span style="color: #808080;">|</span> <span style="color: #0000ff;">NULL</span>    <span style="color: #808080;">|</span>                <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span> sex   <span style="color: #808080;">|</span> enum(<span style="color: #ff0000;">'</span><span style="color: #ff0000;">male</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">female</span><span style="color: #ff0000;">'</span>) <span style="color: #808080;">|</span> YES  <span style="color: #808080;">|</span>     <span style="color: #808080;">|</span> male    <span style="color: #808080;">|</span>                <span style="color: #808080;">|</span>
<span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">-----+-----------------------+------+-----+---------+----------------+</span>
mysql<span style="color: #808080;">></span> <span style="color: #0000ff;">insert</span> <span style="color: #0000ff;">into</span> student(name) <span style="color: #0000ff;">values</span>
    <span style="color: #808080;">-></span> (<span style="color: #ff0000;">'</span><span style="color: #ff0000;">aaa</span><span style="color: #ff0000;">'</span><span style="color: #000000;">),
    </span><span style="color: #808080;">-></span> (<span style="color: #ff0000;">'</span><span style="color: #ff0000;">bbb</span><span style="color: #ff0000;">'</span><span style="color: #000000;">)
    </span><span style="color: #808080;">-></span><span style="color: #000000;"> ;

mysql</span><span style="color: #808080;">></span> <span style="color: #0000ff;">select</span> <span style="color: #808080;">*</span> <span style="color: #0000ff;">from</span><span style="color: #000000;"> student;
</span><span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">--+------+------+</span>
<span style="color: #808080;">|</span> id <span style="color: #808080;">|</span> name <span style="color: #808080;">|</span> sex  <span style="color: #808080;">|</span>
<span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">--+------+------+</span>
<span style="color: #808080;">|</span>  <span style="color: #800000; font-weight: bold;">1</span> <span style="color: #808080;">|</span> aaa <span style="color: #808080;">|</span> male <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span>  <span style="color: #800000; font-weight: bold;">2</span> <span style="color: #808080;">|</span> bbb <span style="color: #808080;">|</span> male <span style="color: #808080;">|</span>
<span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">--+------+------+</span>
<span style="color: #000000;">

#也可以指定id
mysql</span><span style="color: #808080;">></span> <span style="color: #0000ff;">insert</span> <span style="color: #0000ff;">into</span> student <span style="color: #0000ff;">values</span>(<span style="color: #800000; font-weight: bold;">4</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">asb</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">female</span><span style="color: #ff0000;">'</span><span style="color: #000000;">);
Query OK, </span><span style="color: #800000; font-weight: bold;">1</span> row affected (<span style="color: #800000; font-weight: bold;">0.00</span><span style="color: #000000;"> sec)

mysql</span><span style="color: #808080;">></span> <span style="color: #0000ff;">insert</span> <span style="color: #0000ff;">into</span> student <span style="color: #0000ff;">values</span>(<span style="color: #800000; font-weight: bold;">7</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">wsb</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">female</span><span style="color: #ff0000;">'</span><span style="color: #000000;">);
Query OK, </span><span style="color: #800000; font-weight: bold;">1</span> row affected (<span style="color: #800000; font-weight: bold;">0.00</span><span style="color: #000000;"> sec)

mysql</span><span style="color: #808080;">></span> <span style="color: #0000ff;">select</span> <span style="color: #808080;">*</span> <span style="color: #0000ff;">from</span><span style="color: #000000;"> student;
</span><span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">--+------+--------+</span>
<span style="color: #808080;">|</span> id <span style="color: #808080;">|</span> name <span style="color: #808080;">|</span> sex    <span style="color: #808080;">|</span>
<span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">--+------+--------+</span>
<span style="color: #808080;">|</span>  <span style="color: #800000; font-weight: bold;">1</span> <span style="color: #808080;">|</span> aaa <span style="color: #808080;">|</span> male   <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span>  <span style="color: #800000; font-weight: bold;">2</span> <span style="color: #808080;">|</span> bbb <span style="color: #808080;">|</span> male   <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span>  <span style="color: #800000; font-weight: bold;">4</span> <span style="color: #808080;">|</span> asb  <span style="color: #808080;">|</span> female <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span>  <span style="color: #800000; font-weight: bold;">7</span> <span style="color: #808080;">|</span> wsb  <span style="color: #808080;">|</span> female <span style="color: #808080;">|</span>
<span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">--+------+--------+</span>
<span style="color: #000000;">

#对于自增的字段，在用delete删除后，再插入值，该字段仍按照删除前的位置继续增长
mysql</span><span style="color: #808080;">></span> <span style="color: #0000ff;">delete</span> <span style="color: #0000ff;">from</span><span style="color: #000000;"> student;
Query OK, </span><span style="color: #800000; font-weight: bold;">4</span> rows affected (<span style="color: #800000; font-weight: bold;">0.00</span><span style="color: #000000;"> sec)

mysql</span><span style="color: #808080;">></span> <span style="color: #0000ff;">select</span> <span style="color: #808080;">*</span> <span style="color: #0000ff;">from</span><span style="color: #000000;"> student;
Empty </span><span style="color: #0000ff;">set</span> (<span style="color: #800000; font-weight: bold;">0.00</span><span style="color: #000000;"> sec)

mysql</span><span style="color: #808080;">></span> <span style="color: #0000ff;">insert</span> <span style="color: #0000ff;">into</span> student(name) <span style="color: #0000ff;">values</span>(<span style="color: #ff0000;">'</span><span style="color: #ff0000;">ysb</span><span style="color: #ff0000;">'</span><span style="color: #000000;">);
mysql</span><span style="color: #808080;">></span> <span style="color: #0000ff;">select</span> <span style="color: #808080;">*</span> <span style="color: #0000ff;">from</span><span style="color: #000000;"> student;
</span><span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">--+------+------+</span>
<span style="color: #808080;">|</span> id <span style="color: #808080;">|</span> name <span style="color: #808080;">|</span> sex  <span style="color: #808080;">|</span>
<span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">--+------+------+</span>
<span style="color: #808080;">|</span>  <span style="color: #800000; font-weight: bold;">8</span> <span style="color: #808080;">|</span> ysb  <span style="color: #808080;">|</span> male <span style="color: #808080;">|</span>
<span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">--+------+------+</span>
<span style="color: #000000;">
#应该用truncate清空表，比起delete一条一条地删除记录，truncate是直接清空表，在删除大表时用它
mysql</span><span style="color: #808080;">></span> <span style="color: #0000ff;">truncate</span><span style="color: #000000;"> student;
Query OK, </span><span style="color: #800000; font-weight: bold;">0</span> rows affected (<span style="color: #800000; font-weight: bold;">0.01</span><span style="color: #000000;"> sec)

mysql</span><span style="color: #808080;">></span> <span style="color: #0000ff;">insert</span> <span style="color: #0000ff;">into</span> student(name) <span style="color: #0000ff;">values</span>(<span style="color: #ff0000;">'</span><span style="color: #ff0000;">aaa</span><span style="color: #ff0000;">'</span><span style="color: #000000;">);
Query OK, </span><span style="color: #800000; font-weight: bold;">1</span> row affected (<span style="color: #800000; font-weight: bold;">0.01</span><span style="color: #000000;"> sec)

mysql</span><span style="color: #808080;">></span> <span style="color: #0000ff;">select</span> <span style="color: #808080;">*</span> <span style="color: #0000ff;">from</span><span style="color: #000000;"> student;
</span><span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">--+------+------+</span>
<span style="color: #808080;">|</span> id <span style="color: #808080;">|</span> name <span style="color: #808080;">|</span> sex  <span style="color: #808080;">|</span>
<span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">--+------+------+</span>
<span style="color: #808080;">|</span>  <span style="color: #800000; font-weight: bold;">1</span> <span style="color: #808080;">|</span> aaa <span style="color: #808080;">|</span> male <span style="color: #808080;">|</span>
<span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">--+------+------+</span>
row <span style="color: #808080;">in</span> <span style="color: #0000ff;">set</span> (<span style="color: #800000; font-weight: bold;">0.00</span> sec)
```

test

### （六）、foreign key

外键约束是保证一个或两个表之间的参照完整性，外键是构建于一个表的两个字段或是两个表的两个字段之间的参照关系。
现在有两个表，第一个学生表 有三个字段，学号、姓名、班级 第二个学校表 有班级， 老师 等字段， 每个学生都有班级，那班级这个字段就需要重复存储，很浪费资源，我们可以建一个班级表，让学生关联这个班级表。

```
<span style="color: #000000;">#表类型必须是innodb存储引擎，且被关联的字段，即references指定的另外一个表的字段，必须保证唯一
</span><span style="color: #0000ff;">create</span> <span style="color: #0000ff;">table</span><span style="color: #000000;"> class(
id </span><span style="color: #0000ff;">int</span> <span style="color: #0000ff;">primary</span> <span style="color: #0000ff;">key</span><span style="color: #000000;">,
name </span><span style="color: #0000ff;">varchar</span>(<span style="color: #800000; font-weight: bold;">20</span>) <span style="color: #808080;">not</span> <span style="color: #0000ff;">null</span><span style="color: #000000;">
)
#cls_id外键，关联父表（department主键id），同步更新，同步删除
</span><span style="color: #0000ff;">create</span> <span style="color: #0000ff;">table</span><span style="color: #000000;"> student(
id </span><span style="color: #0000ff;">int</span> <span style="color: #0000ff;">primary</span> <span style="color: #0000ff;">key</span><span style="color: #000000;">,
name </span><span style="color: #0000ff;">varchar</span>(<span style="color: #800000; font-weight: bold;">20</span>) <span style="color: #808080;">not</span> <span style="color: #0000ff;">null</span><span style="color: #000000;">,
dpt_id </span><span style="color: #0000ff;">int</span><span style="color: #000000;">,
</span><span style="color: #0000ff;">constraint</span> fk_name <span style="color: #0000ff;">foreign</span> <span style="color: #0000ff;">key</span><span style="color: #000000;">(cls)
</span><span style="color: #0000ff;">references</span><span style="color: #000000;"> class(id)
</span><span style="color: #0000ff;">on</span> <span style="color: #0000ff;">delete</span> <span style="color: #0000ff;">cascade</span>
<span style="color: #0000ff;">on</span> <span style="color: #0000ff;">update</span> <span style="color: #0000ff;">cascade</span><span style="color: #000000;"> 
)engine</span><span style="color: #808080;">=</span>innodb;
```

 foreign key注意：  
 1、被关联的字段必须是一个key，通常是id字段  
 2、创建表时：必须先建立被关联的表，才能建立关联表
  3、插入记录时：必须先往被关联的表插入记录，才能往关联表中插入记录
 4、删除时：应该先删除关联表中的记录，再删除被关联表对应的记录

## 二、表与表之间的关系

#### 如何才能找出两张表之间的关系呢？

```
<span style="color: #000000;">分析步骤：
#</span><span style="color: #800000; font-weight: bold;">1</span><span style="color: #000000;">、先站在左表的角度去找
是否左表的多条记录可以对应右表的一条记录，如果是，则证明左表的一个字段foreign </span><span style="color: #0000ff;">key</span><span style="color: #000000;"> 右表一个字段（通常是id）

#</span><span style="color: #800000; font-weight: bold;">2</span><span style="color: #000000;">、再站在右表的角度去找
是否右表的多条记录可以对应左表的一条记录，如果是，则证明右表的一个字段foreign </span><span style="color: #0000ff;">key</span><span style="color: #000000;"> 左表一个字段（通常是id）

#</span><span style="color: #800000; font-weight: bold;">3</span><span style="color: #000000;">、总结：
#多对一：
如果只有步骤1成立，则是左表多对一右表
如果只有步骤2成立，则是右表多对一左表

#多对多
如果步骤1和2同时成立，则证明这两张表时一个双向的多对一，即多对多,需要定义一个这两张表的关系表来专门存放二者的关系

#一对一:
如果1和2都不成立，而是左表的一条记录唯一对应右表的一条记录，反之亦然。这种情况很简单，就是在左表foreign key右表的基础上，将左表的外键字段设置成unique即可</span>
```

#### 建立表与表之间的关系：

多对一：

![](http://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif)![](http://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif)

```
<span style="color: #808080;">=====================</span>多对一<span style="color: #808080;">=====================</span>
<span style="color: #0000ff;">create</span> <span style="color: #0000ff;">table</span><span style="color: #000000;"> press(
id </span><span style="color: #0000ff;">int</span> <span style="color: #0000ff;">primary</span> <span style="color: #0000ff;">key</span><span style="color: #000000;"> auto_increment,
name </span><span style="color: #0000ff;">varchar</span>(<span style="color: #800000; font-weight: bold;">20</span><span style="color: #000000;">)
);

</span><span style="color: #0000ff;">create</span> <span style="color: #0000ff;">table</span><span style="color: #000000;"> book(
id </span><span style="color: #0000ff;">int</span> <span style="color: #0000ff;">primary</span> <span style="color: #0000ff;">key</span><span style="color: #000000;"> auto_increment,
name </span><span style="color: #0000ff;">varchar</span>(<span style="color: #800000; font-weight: bold;">20</span><span style="color: #000000;">),
press_id </span><span style="color: #0000ff;">int</span> <span style="color: #808080;">not</span> <span style="color: #0000ff;">null</span><span style="color: #000000;">,
</span><span style="color: #0000ff;">foreign</span> <span style="color: #0000ff;">key</span>(press_id) <span style="color: #0000ff;">references</span><span style="color: #000000;"> press(id)
</span><span style="color: #0000ff;">on</span> <span style="color: #0000ff;">delete</span> <span style="color: #0000ff;">cascade</span>
<span style="color: #0000ff;">on</span> <span style="color: #0000ff;">update</span> <span style="color: #0000ff;">cascade</span><span style="color: #000000;">
);

</span><span style="color: #0000ff;">insert</span> <span style="color: #0000ff;">into</span> press(name) <span style="color: #0000ff;">values</span><span style="color: #000000;">
(</span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">北京工业地雷出版社</span><span style="color: #ff0000;">'</span><span style="color: #000000;">),
(</span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">人民音乐不好听出版社</span><span style="color: #ff0000;">'</span><span style="color: #000000;">),
(</span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">知识产权没有用出版社</span><span style="color: #ff0000;">'</span><span style="color: #000000;">)
;

</span><span style="color: #0000ff;">insert</span> <span style="color: #0000ff;">into</span> book(name,press_id) <span style="color: #0000ff;">values</span><span style="color: #000000;">
(</span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">九阳神功</span><span style="color: #ff0000;">'</span>,<span style="color: #800000; font-weight: bold;">1</span><span style="color: #000000;">),
(</span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">九阴真经</span><span style="color: #ff0000;">'</span>,<span style="color: #800000; font-weight: bold;">2</span><span style="color: #000000;">),
(</span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">九阴白骨爪</span><span style="color: #ff0000;">'</span>,<span style="color: #800000; font-weight: bold;">2</span><span style="color: #000000;">),
(</span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">独孤九剑</span><span style="color: #ff0000;">'</span>,<span style="color: #800000; font-weight: bold;">3</span><span style="color: #000000;">),
(</span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">降龙十巴掌</span><span style="color: #ff0000;">'</span>,<span style="color: #800000; font-weight: bold;">2</span><span style="color: #000000;">),
(</span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">葵花宝典</span><span style="color: #ff0000;">'</span>,<span style="color: #800000; font-weight: bold;">3</span><span style="color: #000000;">)
;</span>
```

View Code

多对多：

![](http://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif)![](http://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif)

```
<span style="color: #808080;">=====================</span>多对多<span style="color: #808080;">=====================</span>
<span style="color: #0000ff;">create</span> <span style="color: #0000ff;">table</span><span style="color: #000000;"> author(
id </span><span style="color: #0000ff;">int</span> <span style="color: #0000ff;">primary</span> <span style="color: #0000ff;">key</span><span style="color: #000000;"> auto_increment,
name </span><span style="color: #0000ff;">varchar</span>(<span style="color: #800000; font-weight: bold;">20</span><span style="color: #000000;">)
);

#这张表就存放作者表与书表的关系，即查询二者的关系查这表就可以了
</span><span style="color: #0000ff;">create</span> <span style="color: #0000ff;">table</span><span style="color: #000000;"> author2book(
id </span><span style="color: #0000ff;">int</span> <span style="color: #808080;">not</span> <span style="color: #0000ff;">null</span> <span style="color: #0000ff;">unique</span><span style="color: #000000;"> auto_increment,
author_id </span><span style="color: #0000ff;">int</span> <span style="color: #808080;">not</span> <span style="color: #0000ff;">null</span><span style="color: #000000;">,
book_id </span><span style="color: #0000ff;">int</span> <span style="color: #808080;">not</span> <span style="color: #0000ff;">null</span><span style="color: #000000;">,
</span><span style="color: #0000ff;">constraint</span> fk_author <span style="color: #0000ff;">foreign</span> <span style="color: #0000ff;">key</span>(author_id) <span style="color: #0000ff;">references</span><span style="color: #000000;"> author(id)
</span><span style="color: #0000ff;">on</span> <span style="color: #0000ff;">delete</span> <span style="color: #0000ff;">cascade</span>
<span style="color: #0000ff;">on</span> <span style="color: #0000ff;">update</span> <span style="color: #0000ff;">cascade</span><span style="color: #000000;">,
</span><span style="color: #0000ff;">constraint</span> fk_book <span style="color: #0000ff;">foreign</span> <span style="color: #0000ff;">key</span>(book_id) <span style="color: #0000ff;">references</span><span style="color: #000000;"> book(id)
</span><span style="color: #0000ff;">on</span> <span style="color: #0000ff;">delete</span> <span style="color: #0000ff;">cascade</span>
<span style="color: #0000ff;">on</span> <span style="color: #0000ff;">update</span> <span style="color: #0000ff;">cascade</span><span style="color: #000000;">,
</span><span style="color: #0000ff;">primary</span> <span style="color: #0000ff;">key</span><span style="color: #000000;">(author_id,book_id)
);

#插入四个作者，id依次排开
</span><span style="color: #0000ff;">insert</span> <span style="color: #0000ff;">into</span> author(name) <span style="color: #0000ff;">values</span>(<span style="color: #ff0000;">'</span><span style="color: #ff0000;">egon</span><span style="color: #ff0000;">'</span>),(<span style="color: #ff0000;">'</span><span style="color: #ff0000;">alex</span><span style="color: #ff0000;">'</span>),(<span style="color: #ff0000;">'</span><span style="color: #ff0000;">yuanhao</span><span style="color: #ff0000;">'</span>),(<span style="color: #ff0000;">'</span><span style="color: #ff0000;">wpq</span><span style="color: #ff0000;">'</span><span style="color: #000000;">);

#每个作者与自己的代表作如下
</span><span style="color: #800000; font-weight: bold;">1</span><span style="color: #000000;"> egon: 
      </span><span style="color: #800000; font-weight: bold;">1</span><span style="color: #000000;"> 九阳神功
      </span><span style="color: #800000; font-weight: bold;">2</span><span style="color: #000000;"> 九阴真经
      </span><span style="color: #800000; font-weight: bold;">3</span><span style="color: #000000;"> 九阴白骨爪
      </span><span style="color: #800000; font-weight: bold;">4</span><span style="color: #000000;"> 独孤九剑
      </span><span style="color: #800000; font-weight: bold;">5</span><span style="color: #000000;"> 降龙十巴掌
      </span><span style="color: #800000; font-weight: bold;">6</span><span style="color: #000000;"> 葵花宝典

</span><span style="color: #800000; font-weight: bold;">2</span><span style="color: #000000;"> alex: 
      </span><span style="color: #800000; font-weight: bold;">1</span><span style="color: #000000;"> 九阳神功
      </span><span style="color: #800000; font-weight: bold;">6</span><span style="color: #000000;"> 葵花宝典

</span><span style="color: #800000; font-weight: bold;">3</span><span style="color: #000000;"> yuanhao:
      </span><span style="color: #800000; font-weight: bold;">4</span><span style="color: #000000;"> 独孤九剑
      </span><span style="color: #800000; font-weight: bold;">5</span><span style="color: #000000;"> 降龙十巴掌
      </span><span style="color: #800000; font-weight: bold;">6</span><span style="color: #000000;"> 葵花宝典

</span><span style="color: #800000; font-weight: bold;">4</span><span style="color: #000000;"> wpq:
      </span><span style="color: #800000; font-weight: bold;">1</span><span style="color: #000000;"> 九阳神功

</span><span style="color: #0000ff;">insert</span> <span style="color: #0000ff;">into</span> author2book(author_id,book_id) <span style="color: #0000ff;">values</span><span style="color: #000000;">
(</span><span style="color: #800000; font-weight: bold;">1</span>,<span style="color: #800000; font-weight: bold;">1</span><span style="color: #000000;">),
(</span><span style="color: #800000; font-weight: bold;">1</span>,<span style="color: #800000; font-weight: bold;">2</span><span style="color: #000000;">),
(</span><span style="color: #800000; font-weight: bold;">1</span>,<span style="color: #800000; font-weight: bold;">3</span><span style="color: #000000;">),
(</span><span style="color: #800000; font-weight: bold;">1</span>,<span style="color: #800000; font-weight: bold;">4</span><span style="color: #000000;">),
(</span><span style="color: #800000; font-weight: bold;">1</span>,<span style="color: #800000; font-weight: bold;">5</span><span style="color: #000000;">),
(</span><span style="color: #800000; font-weight: bold;">1</span>,<span style="color: #800000; font-weight: bold;">6</span><span style="color: #000000;">),
(</span><span style="color: #800000; font-weight: bold;">2</span>,<span style="color: #800000; font-weight: bold;">1</span><span style="color: #000000;">),
(</span><span style="color: #800000; font-weight: bold;">2</span>,<span style="color: #800000; font-weight: bold;">6</span><span style="color: #000000;">),
(</span><span style="color: #800000; font-weight: bold;">3</span>,<span style="color: #800000; font-weight: bold;">4</span><span style="color: #000000;">),
(</span><span style="color: #800000; font-weight: bold;">3</span>,<span style="color: #800000; font-weight: bold;">5</span><span style="color: #000000;">),
(</span><span style="color: #800000; font-weight: bold;">3</span>,<span style="color: #800000; font-weight: bold;">6</span><span style="color: #000000;">),
(</span><span style="color: #800000; font-weight: bold;">4</span>,<span style="color: #800000; font-weight: bold;">1</span><span style="color: #000000;">)
;</span>
```

View Code

一对一：

![](http://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif)![](http://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif)

```
<span style="color: #000000;">#一定是student来foreign key表customer，这样就保证了：
#</span><span style="color: #800000; font-weight: bold;">1</span><span style="color: #000000;"> 学生一定是一个客户，
#</span><span style="color: #800000; font-weight: bold;">2</span><span style="color: #000000;"> 客户不一定是学生，但有可能成为一个学生

</span><span style="color: #0000ff;">create</span> <span style="color: #0000ff;">table</span><span style="color: #000000;"> customer(
id </span><span style="color: #0000ff;">int</span> <span style="color: #0000ff;">primary</span> <span style="color: #0000ff;">key</span><span style="color: #000000;"> auto_increment,
name </span><span style="color: #0000ff;">varchar</span>(<span style="color: #800000; font-weight: bold;">20</span>) <span style="color: #808080;">not</span> <span style="color: #0000ff;">null</span><span style="color: #000000;">,
qq </span><span style="color: #0000ff;">varchar</span>(<span style="color: #800000; font-weight: bold;">10</span>) <span style="color: #808080;">not</span> <span style="color: #0000ff;">null</span><span style="color: #000000;">,
phone </span><span style="color: #0000ff;">char</span>(<span style="color: #800000; font-weight: bold;">16</span>) <span style="color: #808080;">not</span> <span style="color: #0000ff;">null</span><span style="color: #000000;">
);

</span><span style="color: #0000ff;">create</span> <span style="color: #0000ff;">table</span><span style="color: #000000;"> student(
id </span><span style="color: #0000ff;">int</span> <span style="color: #0000ff;">primary</span> <span style="color: #0000ff;">key</span><span style="color: #000000;"> auto_increment,
class_name </span><span style="color: #0000ff;">varchar</span>(<span style="color: #800000; font-weight: bold;">20</span>) <span style="color: #808080;">not</span> <span style="color: #0000ff;">null</span><span style="color: #000000;">,
customer_id </span><span style="color: #0000ff;">int</span> <span style="color: #0000ff;">unique</span><span style="color: #000000;">, #该字段一定要是唯一的
</span><span style="color: #0000ff;">foreign</span> <span style="color: #0000ff;">key</span>(customer_id) <span style="color: #0000ff;">references</span><span style="color: #000000;"> customer(id) #外键的字段一定要保证unique
</span><span style="color: #0000ff;">on</span> <span style="color: #0000ff;">delete</span> <span style="color: #0000ff;">cascade</span>
<span style="color: #0000ff;">on</span> <span style="color: #0000ff;">update</span> <span style="color: #0000ff;">cascade</span><span style="color: #000000;">
);

#增加客户
</span><span style="color: #0000ff;">insert</span> <span style="color: #0000ff;">into</span> customer(name,qq,phone) <span style="color: #0000ff;">values</span><span style="color: #000000;">
(</span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">李飞机</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">31811231</span><span style="color: #ff0000;">'</span>,<span style="color: #800000; font-weight: bold;">13811341220</span><span style="color: #000000;">),
(</span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">王大炮</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">123123123</span><span style="color: #ff0000;">'</span>,<span style="color: #800000; font-weight: bold;">15213146809</span><span style="color: #000000;">),
(</span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">守榴弹</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">283818181</span><span style="color: #ff0000;">'</span>,<span style="color: #800000; font-weight: bold;">1867141331</span><span style="color: #000000;">),
(</span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">吴坦克</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">283818181</span><span style="color: #ff0000;">'</span>,<span style="color: #800000; font-weight: bold;">1851143312</span><span style="color: #000000;">),
(</span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">赢火箭</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">888818181</span><span style="color: #ff0000;">'</span>,<span style="color: #800000; font-weight: bold;">1861243314</span><span style="color: #000000;">),
(</span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">战地雷</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">112312312</span><span style="color: #ff0000;">'</span>,<span style="color: #800000; font-weight: bold;">18811431230</span><span style="color: #000000;">)
;

#增加学生
</span><span style="color: #0000ff;">insert</span> <span style="color: #0000ff;">into</span> student(class_name,customer_id) <span style="color: #0000ff;">values</span><span style="color: #000000;">
(</span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">美术一班</span><span style="color: #ff0000;">'</span>,<span style="color: #800000; font-weight: bold;">3</span><span style="color: #000000;">),
(</span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">声乐二班</span><span style="color: #ff0000;">'</span>,<span style="color: #800000; font-weight: bold;">4</span><span style="color: #000000;">),
(</span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">美术一班</span><span style="color: #ff0000;">'</span>,<span style="color: #800000; font-weight: bold;">5</span><span style="color: #000000;">)
;</span>
```

View Code

 
