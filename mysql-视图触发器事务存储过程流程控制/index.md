# mysql-视图、触发器、事务、存储过程、流程控制

# 目录

1.   视图
2.   触发器
3.   事务
4.   存储过程
5.   流程控制

## 一、视图

视图是由查询结果构成的一张虚拟表，和真实的表一样，带有名称的列和行数据  

强调：视图是永久存储的，但是视图存储的不是数据，只是一条sql语句
视图的特点：

1.   视图的列可以来自不同的表，是表的抽象和逻辑意义上建立的新关系。 
2.   视图是由基本表（实表）产生的表（虚表）。
3.   视图的建立和删除不影响基本表。 
4.   对视图内容的更新（添加、删除和修改）直接影响基本表。 
5.   当视图来自多个基本表时，不允许添加和删除数据。

优点：

1.   可以简化查询（多表查询转换为直接通过视图查询）
2.   可以进行权限控制（把表的权限封闭，开发对应的视图权限）

### （一）、创建视图

```
<span style="color: #0000ff;">create</span> <span style="color: #0000ff;">view</span> 视图名称  <span style="color: #0000ff;">as</span> sql 查询语句  例子：<span style="color: #0000ff;">CREATE</span> <span style="color: #0000ff;">view</span> test_view <span style="color: #0000ff;">as</span> <span style="color: #0000ff;">SELECT</span> <span style="color: #808080;">*</span> <span style="color: #0000ff;">from</span> test;
```

### （二）、查询视图

```
<span style="color: #0000ff;">select</span> <span style="color: #808080;">*</span> <span style="color: #0000ff;">from</span> 视图名 <span style="color: #ff0000;">[</span><span style="color: #ff0000;">where 条件</span><span style="color: #ff0000;">]</span>
```

### （三）、修改视图

```
<span style="color: #0000ff;">alter</span> <span style="color: #0000ff;">view</span> 视图名称 <span style="color: #0000ff;">AS</span> SQL语句； 例子：<span style="color: #0000ff;">ALTER</span> <span style="color: #0000ff;">view</span> test_view <span style="color: #0000ff;">as</span> <span style="color: #0000ff;">SELECT</span> <span style="color: #808080;">*</span> <span style="color: #0000ff;">from</span> test_view <span style="color: #0000ff;">WHERE</span> salary<span style="color: #808080;">></span><span style="color: #800000; font-weight: bold;">10000</span>
```

### （四）、删除视图

```
<span style="color: #0000ff;">drop</span> <span style="color: #0000ff;">view</span><span style="color: #000000;"> 视图名称; 
例子：</span><span style="color: #0000ff;">drop</span> <span style="color: #0000ff;">view</span> test_view
```

## 二、触发器

触发器可以监视用户对表的增、删、改操作，并触发某种操作（没有查），自动执行，无法直接调用。
创建触发器语法的四要素：
　　1.监视地点（table）
　　2.监视事件（insert/update/delete）
　　3.触发时间（before/after）
　　4.触发事件（insert/update/delete）

### （一）、创建触发器

![](http://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif)![](http://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif)

```
<span style="color: #000000;"># 插入前
</span><span style="color: #0000ff;">CREATE</span> <span style="color: #0000ff;">TRIGGER</span> tri_before_insert_tb1 BEFORE <span style="color: #0000ff;">INSERT</span> <span style="color: #0000ff;">ON</span> tb1 <span style="color: #0000ff;">FOR</span><span style="color: #000000;"> EACH ROW
</span><span style="color: #0000ff;">BEGIN</span><span style="color: #000000;">
    ...
</span><span style="color: #0000ff;">END</span><span style="color: #000000;">

# 插入后
</span><span style="color: #0000ff;">CREATE</span> <span style="color: #0000ff;">TRIGGER</span> tri_after_insert_tb1 AFTER <span style="color: #0000ff;">INSERT</span> <span style="color: #0000ff;">ON</span> tb1 <span style="color: #0000ff;">FOR</span><span style="color: #000000;"> EACH ROW
</span><span style="color: #0000ff;">BEGIN</span><span style="color: #000000;">
    ...
</span><span style="color: #0000ff;">END</span><span style="color: #000000;">

# 删除前
</span><span style="color: #0000ff;">CREATE</span> <span style="color: #0000ff;">TRIGGER</span> tri_before_delete_tb1 BEFORE <span style="color: #0000ff;">DELETE</span> <span style="color: #0000ff;">ON</span> tb1 <span style="color: #0000ff;">FOR</span><span style="color: #000000;"> EACH ROW
</span><span style="color: #0000ff;">BEGIN</span><span style="color: #000000;">
    ...
</span><span style="color: #0000ff;">END</span><span style="color: #000000;">

# 删除后
</span><span style="color: #0000ff;">CREATE</span> <span style="color: #0000ff;">TRIGGER</span> tri_after_delete_tb1 AFTER <span style="color: #0000ff;">DELETE</span> <span style="color: #0000ff;">ON</span> tb1 <span style="color: #0000ff;">FOR</span><span style="color: #000000;"> EACH ROW
</span><span style="color: #0000ff;">BEGIN</span><span style="color: #000000;">
    ...
</span><span style="color: #0000ff;">END</span><span style="color: #000000;">

# 更新前
</span><span style="color: #0000ff;">CREATE</span> <span style="color: #0000ff;">TRIGGER</span> tri_before_update_tb1 BEFORE <span style="color: #0000ff;">UPDATE</span> <span style="color: #0000ff;">ON</span> tb1 <span style="color: #0000ff;">FOR</span><span style="color: #000000;"> EACH ROW
</span><span style="color: #0000ff;">BEGIN</span><span style="color: #000000;">
    ...
</span><span style="color: #0000ff;">END</span><span style="color: #000000;">

# 更新后
</span><span style="color: #0000ff;">CREATE</span> <span style="color: #0000ff;">TRIGGER</span> tri_after_update_tb1 AFTER <span style="color: #0000ff;">UPDATE</span> <span style="color: #0000ff;">ON</span> tb1 <span style="color: #0000ff;">FOR</span><span style="color: #000000;"> EACH ROW
</span><span style="color: #0000ff;">BEGIN</span><span style="color: #000000;">
    ...
</span><span style="color: #0000ff;">END</span>
```

语法

![](http://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif)![](http://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif)

```
<span style="color: #000000;">#准备表
</span><span style="color: #0000ff;">CREATE</span> <span style="color: #0000ff;">TABLE</span><span style="color: #000000;"> cmd (
    id </span><span style="color: #0000ff;">INT</span> <span style="color: #0000ff;">PRIMARY</span> <span style="color: #0000ff;">KEY</span><span style="color: #000000;"> auto_increment,
    </span><span style="color: #ff00ff;">USER</span> <span style="color: #0000ff;">CHAR</span> (<span style="color: #800000; font-weight: bold;">32</span><span style="color: #000000;">),
    priv </span><span style="color: #0000ff;">CHAR</span> (<span style="color: #800000; font-weight: bold;">10</span><span style="color: #000000;">),
    cmd </span><span style="color: #0000ff;">CHAR</span> (<span style="color: #800000; font-weight: bold;">64</span><span style="color: #000000;">),
    sub_time </span><span style="color: #0000ff;">datetime</span><span style="color: #000000;">, #提交时间
    success enum (</span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">yes</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">no</span><span style="color: #ff0000;">'</span><span style="color: #000000;">) #0代表执行失败
);

</span><span style="color: #0000ff;">CREATE</span> <span style="color: #0000ff;">TABLE</span><span style="color: #000000;"> errlog (
    id </span><span style="color: #0000ff;">INT</span> <span style="color: #0000ff;">PRIMARY</span> <span style="color: #0000ff;">KEY</span><span style="color: #000000;"> auto_increment,
    err_cmd </span><span style="color: #0000ff;">CHAR</span> (<span style="color: #800000; font-weight: bold;">64</span><span style="color: #000000;">),
    err_time </span><span style="color: #0000ff;">datetime</span><span style="color: #000000;">
);

#创建触发器
delimiter </span><span style="color: #808080;">//</span>
<span style="color: #0000ff;">CREATE</span> <span style="color: #0000ff;">TRIGGER</span> tri_after_insert_cmd AFTER <span style="color: #0000ff;">INSERT</span> <span style="color: #0000ff;">ON</span> cmd <span style="color: #0000ff;">FOR</span><span style="color: #000000;"> EACH ROW
</span><span style="color: #0000ff;">BEGIN</span>
    <span style="color: #0000ff;">IF</span> NEW.success <span style="color: #808080;">=</span> <span style="color: #ff0000;">'</span><span style="color: #ff0000;">no</span><span style="color: #ff0000;">'</span> <span style="color: #0000ff;">THEN</span><span style="color: #000000;"> #等值判断只有一个等号
            </span><span style="color: #0000ff;">INSERT</span> <span style="color: #0000ff;">INTO</span> errlog(err_cmd, err_time) <span style="color: #0000ff;">VALUES</span><span style="color: #000000;">(NEW.cmd, NEW.sub_time) ; #必须加分号
      </span><span style="color: #0000ff;">END</span> <span style="color: #0000ff;">IF</span><span style="color: #000000;"> ; #必须加分号
</span><span style="color: #0000ff;">END</span><span style="color: #808080;">//</span><span style="color: #000000;">
delimiter ;

#往表cmd中插入记录，触发触发器，根据IF的条件决定是否插入错误日志
</span><span style="color: #0000ff;">INSERT</span> <span style="color: #0000ff;">INTO</span><span style="color: #000000;"> cmd (
    </span><span style="color: #ff00ff;">USER</span><span style="color: #000000;">,
    priv,
    cmd,
    sub_time,
    success
)
</span><span style="color: #0000ff;">VALUES</span><span style="color: #000000;">
    (</span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">A</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">0755</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">ls -l /etc</span><span style="color: #ff0000;">'</span>,NOW(),<span style="color: #ff0000;">'</span><span style="color: #ff0000;">yes</span><span style="color: #ff0000;">'</span><span style="color: #000000;">),
    (</span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">A</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">0755</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">cat /etc/passwd</span><span style="color: #ff0000;">'</span>,NOW(),<span style="color: #ff0000;">'</span><span style="color: #ff0000;">no</span><span style="color: #ff0000;">'</span><span style="color: #000000;">),
    (</span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">A</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">0755</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">useradd xxx</span><span style="color: #ff0000;">'</span>,NOW(),<span style="color: #ff0000;">'</span><span style="color: #ff0000;">no</span><span style="color: #ff0000;">'</span><span style="color: #000000;">),
    (</span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">A</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">0755</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">ps aux</span><span style="color: #ff0000;">'</span>,NOW(),<span style="color: #ff0000;">'</span><span style="color: #ff0000;">yes</span><span style="color: #ff0000;">'</span><span style="color: #000000;">);

#查询错误日志，发现有两条
mysql</span><span style="color: #808080;">></span> <span style="color: #0000ff;">select</span> <span style="color: #808080;">*</span> <span style="color: #0000ff;">from</span><span style="color: #000000;"> errlog;
</span><span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">--+-----------------+---------------------+</span>
<span style="color: #808080;">|</span> id <span style="color: #808080;">|</span> err_cmd         <span style="color: #808080;">|</span> err_time            <span style="color: #808080;">|</span>
<span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">--+-----------------+---------------------+</span>
<span style="color: #808080;">|</span>  <span style="color: #800000; font-weight: bold;">1</span> <span style="color: #808080;">|</span> cat <span style="color: #808080;">/</span>etc<span style="color: #808080;">/</span>passwd <span style="color: #808080;">|</span> <span style="color: #800000; font-weight: bold;">2018</span><span style="color: #808080;">-</span><span style="color: #800000; font-weight: bold;">09</span><span style="color: #808080;">-</span><span style="color: #800000; font-weight: bold;">18</span> <span style="color: #800000; font-weight: bold;">20</span>:<span style="color: #800000; font-weight: bold;">18</span>:<span style="color: #800000; font-weight: bold;">48</span> <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span>  <span style="color: #800000; font-weight: bold;">2</span> <span style="color: #808080;">|</span> useradd xxx     <span style="color: #808080;">|</span> <span style="color: #800000; font-weight: bold;">2018</span><span style="color: #808080;">-</span><span style="color: #800000; font-weight: bold;">09</span><span style="color: #808080;">-</span><span style="color: #800000; font-weight: bold;">18</span> <span style="color: #800000; font-weight: bold;">20</span>:<span style="color: #800000; font-weight: bold;">18</span>:<span style="color: #800000; font-weight: bold;">48</span> <span style="color: #808080;">|</span>
<span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">--+-----------------+---------------------+</span>
<span style="color: #800000; font-weight: bold;">2</span> rows <span style="color: #808080;">in</span> <span style="color: #0000ff;">set</span> (<span style="color: #800000; font-weight: bold;">0.00</span> sec)
```

案例

强调：NEW表示即将插入的数据行，OLD表示即将删除的数据行

### （二）、查看触发器

```
show triggers
```

### （三）、删除触发器

```
<span style="color: #0000ff;">drop</span> <span style="color: #0000ff;">trigger</span> 触发器的名称
```

## 三、事务

事务用于将某些操作的多个SQL作为原子性操作，意思就是，事务是一组sql语句集合。
一旦有某一个出现错误，即可回滚到原来的状态，从而保证数据库数据完整性。在事务内的语句, 要么全部执行成功, 要么全部执行失败。

### （一）、事务的特性

事务具有以下四个特性（ACID）
　　1.原子性：事务是一个整体，不可分割，包含在其中的sql操作要么全部成功，要么全部失败回滚，不可能只执行其中一部分操作。
　　2.一致性：当事务执行后 所有的数据都是完整的(外键约束 非空约束)。
　　3.持久性：一旦事务提交，数据永久保存在数据库中
　　4.隔离性：事务之间相互隔离，一个事务的执行不影响其他事务的执行
SQL标准定义了4类隔离级别，包括了一些具体规则，用来限定事务内外的哪些改变是可见的，哪些是不可见的。低级别的隔离级一般支持更高的并发处理，并拥有更低的系统开销。

### （二）、事务的隔离级别

　　1.READ UNCOMMITED（未提交读）：所有事务都可以看到其他未提交事务的执行结果。很少用于实际应用，因为它的性能不比其他级别好多少
　　2.READ COMMITED（提交读）：大部分数据库默认级别，不包括mysql。一个事务从开始到提交之前, 所做的任何修改对其他事务都是不可见的。
　　3.REPEATABLE READ（可重复读）：mysql默认级别，解决了脏读的问题. 该级别保证了在同一个事务中多次读取同样记录的结果时一致的. 无法解决幻读问题
　　4.SERIALIZABLE（可串行化）：是最高的隔离级别，强制事务排序，使之不可能相互冲突，从而解决幻读问题
![](https://img2018.cnblogs.com/blog/1285461/201809/1285461-20180918195828851-51817063.png)
脏读： 一个事物 读到了 另一个事务未提交的数据 查询 之前要保证 所有的更新都已经完成。
不可重复读：在一个事务的两次查询之中数据不一致，这可能是两次查询过程中间插入了一个事务更新的原有的数据。
幻读：指的是当某个事务在读取某个范围内的记录时, 另外一个事务又在该范围内插入了新的记录, 当之前的事务再次读取该范围的记录时, 会产生幻行(Phantom Row).

###  （三）、事务操作

```
start <span style="color: #0000ff;">transaction</span><span style="color: #000000;">; 开启一个事物
</span><span style="color: #0000ff;">commit</span><span style="color: #000000;"> 提交事物
</span><span style="color: #0000ff;">rollback</span> 回滚事务
```

注：mysql默认开启自动提交事务，pymysql默认是不自动提交，需手动commit

## 四、存储过程

存储过程包含了一系列可执行的sql语句的集合，类似于函数(方法)。
使用存储过程的优点：

```
#<span style="color: #800000; font-weight: bold;">1</span><span style="color: #000000;">. 用于替代程序写的SQL语句，实现程序与sql解耦

#</span><span style="color: #800000; font-weight: bold;">2</span>. 基于网络传输，传别名的数据量小，而直接传sql数据量大
```

缺点：不方便扩展

### （一）、使用存储过程

```
<span style="color: #000000;">创建语法:
    </span><span style="color: #0000ff;">create</span> <span style="color: #0000ff;">procedure</span> 过程的名称 ({<span style="color: #808080;">in</span><span style="color: #000000;">,out,inout}  数据类型 参数名称)
    </span><span style="color: #0000ff;">begin</span><span style="color: #000000;">
        具体的sql代码
    </span><span style="color: #0000ff;">end</span><span style="color: #000000;">
    参数前面需要指定参数的作用<br/>
    </span><span style="color: #808080;">in</span><span style="color: #000000;"> 表示该参数用于传入数据
    out 用于返回数据
    inout 即可传入 也可返回

    参数类型是 mysql中的数据类型<br/><br/>调用语法：<br/>　　call 存储过程()<br/></span>
```

![](http://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif)![](http://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif)

```
<span style="color: #000000;">         案例:创建一个存储过程 作用是将两个整数相加
            </span><span style="color: #0000ff;">create</span> <span style="color: #0000ff;">procedure</span> add_p (<span style="color: #808080;">in</span> a <span style="color: #0000ff;">int</span>,<span style="color: #808080;">in</span> b <span style="color: #0000ff;">int</span><span style="color: #000000;">)
            </span><span style="color: #0000ff;">begin</span>
                <span style="color: #0000ff;">select</span> a <span style="color: #808080;">+</span><span style="color: #000000;"> b;
            </span><span style="color: #0000ff;">end</span>

            <span style="color: #808080;">//</span><span style="color: #000000;">
      调用 call add_p(</span><span style="color: #800000; font-weight: bold;">1</span>,<span style="color: #800000; font-weight: bold;">2</span><span style="color: #000000;">)

         案例:创建一个存储过程 作用是将两个整数相加 将结果保存在变量中
        定义一个变量
        </span><span style="color: #0000ff;">set</span> <span style="color: #008000;">@su</span> <span style="color: #808080;">=</span> <span style="color: #800000; font-weight: bold;">100</span><span style="color: #000000;">;
            </span><span style="color: #0000ff;">create</span> <span style="color: #0000ff;">procedure</span> add_p2 (<span style="color: #808080;">in</span> a <span style="color: #0000ff;">int</span>,<span style="color: #808080;">in</span> b <span style="color: #0000ff;">int</span>,out su <span style="color: #0000ff;">int</span><span style="color: #000000;">)
            </span><span style="color: #0000ff;">begin</span>
                <span style="color: #0000ff;">set</span> su <span style="color: #808080;">=</span> a <span style="color: #808080;">+</span><span style="color: #000000;"> b;
            </span><span style="color: #0000ff;">end</span>

            <span style="color: #808080;">//</span><span style="color: #000000;">
    
    定义变量 </span><span style="color: #0000ff;">set</span> <span style="color: #008000;">@su</span> <span style="color: #808080;">=</span> <span style="color: #800000; font-weight: bold;">100</span><span style="color: #000000;">;
    调用过程 call  add_p2(</span><span style="color: #800000; font-weight: bold;">10</span>,<span style="color: #800000; font-weight: bold;">20</span>,<span style="color: #008000;">@su</span><span style="color: #000000;">);

          注意  在存储过程中 需要使用分号来结束一行 但是分号有特殊含义
          得将原始的结束符 修改为其他符号
            delimiter </span><span style="color: #808080;">//</span> 结束符更换为<span style="color: #808080;">//</span><span style="color: #000000;">
            delimiter;</span>
```

案列

```
<span style="color: #000000;">在存储过程中 需要使用分号来结束一行 但是分号有特殊含义
得将原始的结束符 修改为其他符号
      delimiter </span><span style="color: #808080;">//</span> 结束符更换为<span style="color: #808080;">//</span><span style="color: #000000;">
      delimiter;</span>
```

![](http://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif)![](http://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif)

```
    <span style="color: #0000ff;">create</span> <span style="color: #0000ff;">procedure</span> show_p (<span style="color: #808080;">in</span> a <span style="color: #0000ff;">int</span><span style="color: #000000;">)
    </span><span style="color: #0000ff;">begin</span>
    <span style="color: #0000ff;">if</span> a <span style="color: #808080;">=</span> <span style="color: #800000; font-weight: bold;">1</span> <span style="color: #0000ff;">then</span>
        <span style="color: #0000ff;">select</span><span style="color: #000000;"> "壹";
    elseif a </span><span style="color: #808080;">=</span> <span style="color: #800000; font-weight: bold;">2</span> <span style="color: #0000ff;">then</span>
        <span style="color: #0000ff;">select</span><span style="color: #000000;"> "贰";
    </span><span style="color: #0000ff;">else</span>
        <span style="color: #0000ff;">select</span><span style="color: #000000;"> "other";
    </span><span style="color: #0000ff;">end</span> <span style="color: #0000ff;">if</span><span style="color: #000000;">;
    </span><span style="color: #0000ff;">end</span> <span style="color: #808080;">//</span>
```

使用存储过程 完成 输入 一个 数字 1或2 显示 壹 或 贰

### （二）、删除存储过程

```
<span style="color: #0000ff;">drop</span> <span style="color: #0000ff;">procedure</span> proc_name;
```

## 五、流程控制

### （一）、条件语句

![](http://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif)![](http://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif)

```
delimiter <span style="color: #808080;">//</span>
<span style="color: #0000ff;">CREATE</span> <span style="color: #0000ff;">PROCEDURE</span><span style="color: #000000;"> proc_if ()
</span><span style="color: #0000ff;">BEGIN</span>
    
    <span style="color: #0000ff;">declare</span> i <span style="color: #0000ff;">int</span> <span style="color: #0000ff;">default</span> <span style="color: #800000; font-weight: bold;">0</span><span style="color: #000000;">;
    </span><span style="color: #0000ff;">if</span> i <span style="color: #808080;">=</span> <span style="color: #800000; font-weight: bold;">1</span> <span style="color: #0000ff;">THEN</span>
        <span style="color: #0000ff;">SELECT</span> <span style="color: #800000; font-weight: bold;">1</span><span style="color: #000000;">;
    ELSEIF i </span><span style="color: #808080;">=</span> <span style="color: #800000; font-weight: bold;">2</span> <span style="color: #0000ff;">THEN</span>
        <span style="color: #0000ff;">SELECT</span> <span style="color: #800000; font-weight: bold;">2</span><span style="color: #000000;">;
    </span><span style="color: #0000ff;">ELSE</span>
        <span style="color: #0000ff;">SELECT</span> <span style="color: #800000; font-weight: bold;">7</span><span style="color: #000000;">;
    </span><span style="color: #0000ff;">END</span> <span style="color: #0000ff;">IF</span><span style="color: #000000;">;

</span><span style="color: #0000ff;">END</span> <span style="color: #808080;">//</span><span style="color: #000000;">
delimiter ;</span>
```

if

### （二）、循环语句

![](http://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif)![](http://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif)

```
delimiter <span style="color: #808080;">//</span>
<span style="color: #0000ff;">CREATE</span> <span style="color: #0000ff;">PROCEDURE</span><span style="color: #000000;"> proc_while ()
</span><span style="color: #0000ff;">BEGIN</span>

    <span style="color: #0000ff;">DECLARE</span> num <span style="color: #0000ff;">INT</span><span style="color: #000000;"> ;
    </span><span style="color: #0000ff;">SET</span> num <span style="color: #808080;">=</span> <span style="color: #800000; font-weight: bold;">0</span><span style="color: #000000;"> ;
    </span><span style="color: #0000ff;">WHILE</span> num <span style="color: #808080;"><</span> <span style="color: #800000; font-weight: bold;">10</span><span style="color: #000000;"> DO
        </span><span style="color: #0000ff;">SELECT</span><span style="color: #000000;">
            num ;
        </span><span style="color: #0000ff;">SET</span> num <span style="color: #808080;">=</span> num <span style="color: #808080;">+</span> <span style="color: #800000; font-weight: bold;">1</span><span style="color: #000000;"> ;
    </span><span style="color: #0000ff;">END</span> <span style="color: #0000ff;">WHILE</span><span style="color: #000000;"> ;

</span><span style="color: #0000ff;">END</span> <span style="color: #808080;">//</span><span style="color: #000000;">
delimiter ;</span>
```

while

![](http://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif)![](http://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif)

```
delimiter <span style="color: #808080;">//</span>
<span style="color: #0000ff;">CREATE</span> <span style="color: #0000ff;">PROCEDURE</span><span style="color: #000000;"> proc_repeat ()
</span><span style="color: #0000ff;">BEGIN</span>

    <span style="color: #0000ff;">DECLARE</span> i <span style="color: #0000ff;">INT</span><span style="color: #000000;"> ;
    </span><span style="color: #0000ff;">SET</span> i <span style="color: #808080;">=</span> <span style="color: #800000; font-weight: bold;">0</span><span style="color: #000000;"> ;
    repeat
        </span><span style="color: #0000ff;">select</span><span style="color: #000000;"> i;
        </span><span style="color: #0000ff;">set</span> i <span style="color: #808080;">=</span> i <span style="color: #808080;">+</span> <span style="color: #800000; font-weight: bold;">1</span><span style="color: #000000;">;
        until i </span><span style="color: #808080;">>=</span> <span style="color: #800000; font-weight: bold;">5</span>
    <span style="color: #0000ff;">end</span><span style="color: #000000;"> repeat;

</span><span style="color: #0000ff;">END</span> <span style="color: #808080;">//</span><span style="color: #000000;">
delimiter ;</span>
```

repeat

![](http://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif)![](http://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif)

```
<span style="color: #0000ff;">BEGIN</span>
    
    <span style="color: #0000ff;">declare</span> i <span style="color: #0000ff;">int</span> <span style="color: #0000ff;">default</span> <span style="color: #800000; font-weight: bold;">0</span><span style="color: #000000;">;
    loop_label: loop
        
        </span><span style="color: #0000ff;">set</span> i<span style="color: #808080;">=</span>i<span style="color: #808080;">+</span><span style="color: #800000; font-weight: bold;">1</span><span style="color: #000000;">;
        </span><span style="color: #0000ff;">if</span> i<span style="color: #808080;"><</span><span style="color: #800000; font-weight: bold;">8</span> <span style="color: #0000ff;">then</span><span style="color: #000000;">
            iterate loop_label;
        </span><span style="color: #0000ff;">end</span> <span style="color: #0000ff;">if</span><span style="color: #000000;">;
        </span><span style="color: #0000ff;">if</span> i<span style="color: #808080;">>=</span><span style="color: #800000; font-weight: bold;">10</span> <span style="color: #0000ff;">then</span><span style="color: #000000;">
            leave loop_label;
        </span><span style="color: #0000ff;">end</span> <span style="color: #0000ff;">if</span><span style="color: #000000;">;
        </span><span style="color: #0000ff;">select</span><span style="color: #000000;"> i;
    </span><span style="color: #0000ff;">end</span><span style="color: #000000;"> loop loop_label;

</span><span style="color: #0000ff;">END</span>
```

loop

 
