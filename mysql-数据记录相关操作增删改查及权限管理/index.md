# mysql-数据（记录）相关操作（增删改查）及权限管理

## 一、介绍

在MySQL管理软件中，可以通过SQL语句中的DML语言来实现数据的操作，包括

1.   使用INSERT实现数据的插入
2.   UPDATE实现数据的更新
3.   使用DELETE实现数据的删除
4.   使用SELECT查询数据以及。

## 二、插入数据

```
<span style="color: #800000; font-weight: bold;">1</span><span style="color: #000000;">. 插入完整数据（顺序插入）
    语法一：
    </span><span style="color: #0000ff;">INSERT</span> <span style="color: #0000ff;">INTO</span> 表名(字段1,字段2,字段3…字段n) <span style="color: #0000ff;">VALUES</span><span style="color: #000000;">(值1,值2,值3…值n);

    语法二：
    </span><span style="color: #0000ff;">INSERT</span> <span style="color: #0000ff;">INTO</span> 表名 <span style="color: #0000ff;">VALUES</span><span style="color: #000000;"> (值1,值2,值3…值n);

</span><span style="color: #800000; font-weight: bold;">2</span><span style="color: #000000;">. 指定字段插入数据
    语法：
    </span><span style="color: #0000ff;">INSERT</span> <span style="color: #0000ff;">INTO</span> 表名(字段1,字段2,字段3…) <span style="color: #0000ff;">VALUES</span><span style="color: #000000;"> (值1,值2,值3…);

</span><span style="color: #800000; font-weight: bold;">3</span><span style="color: #000000;">. 插入多条记录
    语法：
    </span><span style="color: #0000ff;">INSERT</span> <span style="color: #0000ff;">INTO</span> 表名 <span style="color: #0000ff;">VALUES</span><span style="color: #000000;">
        (值1,值2,值3…值n),
        (值1,值2,值3…值n),
        (值1,值2,值3…值n);
        
</span><span style="color: #800000; font-weight: bold;">4</span><span style="color: #000000;">. 插入查询结果
    语法：
    </span><span style="color: #0000ff;">INSERT</span> <span style="color: #0000ff;">INTO</span><span style="color: #000000;"> 表名(字段1,字段2,字段3…字段n) 
                    </span><span style="color: #0000ff;">SELECT</span> (字段1,字段2,字段3…字段n) <span style="color: #0000ff;">FROM</span><span style="color: #000000;"> 表2
                    </span><span style="color: #0000ff;">WHERE</span> …;
```

## 三、更新数据

```
<span style="color: #000000;">语法：
    </span><span style="color: #0000ff;">UPDATE</span> 表名 <span style="color: #0000ff;">SET</span><span style="color: #000000;">
        字段1</span><span style="color: #808080;">=</span><span style="color: #000000;">值1,
        字段2</span><span style="color: #808080;">=</span><span style="color: #000000;">值2,
        </span><span style="color: #0000ff;">WHERE</span> CONDITION;
```

## 四、删除数据

```
<span style="color: #000000;">语法：
    </span><span style="color: #0000ff;">DELETE</span> <span style="color: #0000ff;">FROM</span><span style="color: #000000;"> 表名 
        </span><span style="color: #0000ff;">WHERE</span> CONITION;
```

## 五、查询数据

```
<span style="color: #000000;">单表查询语法：
    </span><span style="color: #0000ff;">SELECT</span> 字段1,字段2... <span style="color: #0000ff;">FROM</span><span style="color: #000000;"> 表名
                  </span><span style="color: #0000ff;">WHERE</span><span style="color: #000000;"> 条件
                  </span><span style="color: #0000ff;">GROUP</span> <span style="color: #0000ff;">BY</span><span style="color: #000000;"> field
                  </span><span style="color: #0000ff;">HAVING</span><span style="color: #000000;"> 筛选
                  </span><span style="color: #0000ff;">ORDER</span> <span style="color: #0000ff;">BY</span><span style="color: #000000;"> field
                  LIMIT 限制条数

多表查询语法：
    </span><span style="color: #0000ff;">SELECT</span><span style="color: #000000;"> 字段列表
    </span><span style="color: #0000ff;">FROM</span> 表1 <span style="color: #0000ff;">INNER</span><span style="color: #808080;">|LEFT|RIGHT</span> <span style="color: #808080;">JOIN</span><span style="color: #000000;"> 表2
    </span><span style="color: #0000ff;">ON</span> 表1.字段 <span style="color: #808080;">=</span> 表2.字段;
```

### select关键字的定义顺序：

```
<span style="color: #0000ff;">SELECT</span> <span style="color: #0000ff;">DISTINCT</span> <span style="color: #808080;"><</span>select_list<span style="color: #808080;">></span>
<span style="color: #0000ff;">FROM</span> <span style="color: #808080;"><</span>left_table<span style="color: #808080;">></span>
<span style="color: #808080;"><</span>join_type<span style="color: #808080;">></span> <span style="color: #808080;">JOIN</span> <span style="color: #808080;"><</span>right_table<span style="color: #808080;">></span>
<span style="color: #0000ff;">ON</span> <span style="color: #808080;"><</span>join_condition<span style="color: #808080;">></span>
<span style="color: #0000ff;">WHERE</span> <span style="color: #808080;"><</span>where_condition<span style="color: #808080;">></span>
<span style="color: #0000ff;">GROUP</span> <span style="color: #0000ff;">BY</span> <span style="color: #808080;"><</span>group_by_list<span style="color: #808080;">></span>
<span style="color: #0000ff;">HAVING</span> <span style="color: #808080;"><</span>having_condition<span style="color: #808080;">></span>
<span style="color: #0000ff;">ORDER</span> <span style="color: #0000ff;">BY</span> <span style="color: #808080;"><</span>order_by_condition<span style="color: #808080;">></span><span style="color: #000000;">
LIMIT </span><span style="color: #808080;"><</span>limit_number<span style="color: #808080;">></span>
```

### select关键字的执行顺序：

```
(<span style="color: #800000; font-weight: bold;">7</span>)     <span style="color: #0000ff;">SELECT</span><span style="color: #000000;"> 
(</span><span style="color: #800000; font-weight: bold;">8</span>)     <span style="color: #0000ff;">DISTINCT</span> <span style="color: #808080;"><</span>select_list<span style="color: #808080;">></span><span style="color: #000000;">
(</span><span style="color: #800000; font-weight: bold;">1</span>)     <span style="color: #0000ff;">FROM</span> <span style="color: #808080;"><</span>left_table<span style="color: #808080;">></span><span style="color: #000000;">
(</span><span style="color: #800000; font-weight: bold;">3</span>)     <span style="color: #808080;"><</span>join_type<span style="color: #808080;">></span> <span style="color: #808080;">JOIN</span> <span style="color: #808080;"><</span>right_table<span style="color: #808080;">></span><span style="color: #000000;">
(</span><span style="color: #800000; font-weight: bold;">2</span>)     <span style="color: #0000ff;">ON</span> <span style="color: #808080;"><</span>join_condition<span style="color: #808080;">></span><span style="color: #000000;">
(</span><span style="color: #800000; font-weight: bold;">4</span>)     <span style="color: #0000ff;">WHERE</span> <span style="color: #808080;"><</span>where_condition<span style="color: #808080;">></span><span style="color: #000000;">
(</span><span style="color: #800000; font-weight: bold;">5</span>)     <span style="color: #0000ff;">GROUP</span> <span style="color: #0000ff;">BY</span> <span style="color: #808080;"><</span>group_by_list<span style="color: #808080;">></span><span style="color: #000000;">
(</span><span style="color: #800000; font-weight: bold;">6</span>)     <span style="color: #0000ff;">HAVING</span> <span style="color: #808080;"><</span>having_condition<span style="color: #808080;">></span><span style="color: #000000;">
(</span><span style="color: #800000; font-weight: bold;">9</span>)     <span style="color: #0000ff;">ORDER</span> <span style="color: #0000ff;">BY</span> <span style="color: #808080;"><</span>order_by_condition<span style="color: #808080;">></span><span style="color: #000000;">
(</span><span style="color: #800000; font-weight: bold;">10</span>)    LIMIT <span style="color: #808080;"><</span>limit_number<span style="color: #808080;">></span>
```

### （一）、单表查询

![](http://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif)![](http://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif)

```
<span style="color: #000000;">company.employee
    员工id      id                  </span><span style="color: #0000ff;">int</span><span style="color: #000000;">             
    姓名        emp_name            </span><span style="color: #0000ff;">varchar</span><span style="color: #000000;">
    性别        sex                 enum
    年龄        age                 </span><span style="color: #0000ff;">int</span><span style="color: #000000;">
    入职日期     hire_date           date
    岗位        post                </span><span style="color: #0000ff;">varchar</span><span style="color: #000000;">
    职位描述     post_comment        </span><span style="color: #0000ff;">varchar</span><span style="color: #000000;">
    薪水        salary              </span><span style="color: #0000ff;">double</span><span style="color: #000000;">
    办公室       office              </span><span style="color: #0000ff;">int</span><span style="color: #000000;">
    部门编号     depart_id           </span><span style="color: #0000ff;">int</span><span style="color: #000000;">

#创建表
</span><span style="color: #0000ff;">create</span> <span style="color: #0000ff;">table</span><span style="color: #000000;"> employee(
id </span><span style="color: #0000ff;">int</span> <span style="color: #808080;">not</span> <span style="color: #0000ff;">null</span> <span style="color: #0000ff;">unique</span><span style="color: #000000;"> auto_increment,
name </span><span style="color: #0000ff;">varchar</span>(<span style="color: #800000; font-weight: bold;">20</span>) <span style="color: #808080;">not</span> <span style="color: #0000ff;">null</span><span style="color: #000000;">,
sex enum(</span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">male</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">female</span><span style="color: #ff0000;">'</span>) <span style="color: #808080;">not</span> <span style="color: #0000ff;">null</span> <span style="color: #0000ff;">default</span> <span style="color: #ff0000;">'</span><span style="color: #ff0000;">male</span><span style="color: #ff0000;">'</span><span style="color: #000000;">, #大部分是男的
age </span><span style="color: #0000ff;">int</span>(<span style="color: #800000; font-weight: bold;">3</span>) unsigned <span style="color: #808080;">not</span> <span style="color: #0000ff;">null</span> <span style="color: #0000ff;">default</span> <span style="color: #800000; font-weight: bold;">28</span><span style="color: #000000;">,
hire_date date </span><span style="color: #808080;">not</span> <span style="color: #0000ff;">null</span><span style="color: #000000;">,
post </span><span style="color: #0000ff;">varchar</span>(<span style="color: #800000; font-weight: bold;">50</span><span style="color: #000000;">),
post_comment </span><span style="color: #0000ff;">varchar</span>(<span style="color: #800000; font-weight: bold;">100</span><span style="color: #000000;">),
salary </span><span style="color: #0000ff;">double</span>(<span style="color: #800000; font-weight: bold;">15</span>,<span style="color: #800000; font-weight: bold;">2</span><span style="color: #000000;">),
office </span><span style="color: #0000ff;">int</span><span style="color: #000000;">, #一个部门一个屋子
depart_id </span><span style="color: #0000ff;">int</span><span style="color: #000000;">
);
#插入记录
#三个部门：教学，销售，运营
</span><span style="color: #0000ff;">insert</span> <span style="color: #0000ff;">into</span> employee(name,sex,age,hire_date,post,salary,office,depart_id) <span style="color: #0000ff;">values</span><span style="color: #000000;">
(</span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">egon</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">male</span><span style="color: #ff0000;">'</span>,<span style="color: #800000; font-weight: bold;">18</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">20170301</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">老男孩驻沙河办事处外交大使</span><span style="color: #ff0000;">'</span>,<span style="color: #800000; font-weight: bold;">7300.33</span>,<span style="color: #800000; font-weight: bold;">401</span>,<span style="color: #800000; font-weight: bold;">1</span><span style="color: #000000;">), #以下是教学部
(</span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">alex</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">male</span><span style="color: #ff0000;">'</span>,<span style="color: #800000; font-weight: bold;">78</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">20150302</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">teacher</span><span style="color: #ff0000;">'</span>,<span style="color: #800000; font-weight: bold;">1000000.31</span>,<span style="color: #800000; font-weight: bold;">401</span>,<span style="color: #800000; font-weight: bold;">1</span><span style="color: #000000;">),
(</span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">wupeiqi</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">male</span><span style="color: #ff0000;">'</span>,<span style="color: #800000; font-weight: bold;">81</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">20130305</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">teacher</span><span style="color: #ff0000;">'</span>,<span style="color: #800000; font-weight: bold;">8300</span>,<span style="color: #800000; font-weight: bold;">401</span>,<span style="color: #800000; font-weight: bold;">1</span><span style="color: #000000;">),
(</span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">yuanhao</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">male</span><span style="color: #ff0000;">'</span>,<span style="color: #800000; font-weight: bold;">73</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">20140701</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">teacher</span><span style="color: #ff0000;">'</span>,<span style="color: #800000; font-weight: bold;">3500</span>,<span style="color: #800000; font-weight: bold;">401</span>,<span style="color: #800000; font-weight: bold;">1</span><span style="color: #000000;">),
(</span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">liwenzhou</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">male</span><span style="color: #ff0000;">'</span>,<span style="color: #800000; font-weight: bold;">28</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">20121101</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">teacher</span><span style="color: #ff0000;">'</span>,<span style="color: #800000; font-weight: bold;">2100</span>,<span style="color: #800000; font-weight: bold;">401</span>,<span style="color: #800000; font-weight: bold;">1</span><span style="color: #000000;">),
(</span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">jingliyang</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">female</span><span style="color: #ff0000;">'</span>,<span style="color: #800000; font-weight: bold;">18</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">20110211</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">teacher</span><span style="color: #ff0000;">'</span>,<span style="color: #800000; font-weight: bold;">9000</span>,<span style="color: #800000; font-weight: bold;">401</span>,<span style="color: #800000; font-weight: bold;">1</span><span style="color: #000000;">),
(</span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">jinxin</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">male</span><span style="color: #ff0000;">'</span>,<span style="color: #800000; font-weight: bold;">18</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">19000301</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">teacher</span><span style="color: #ff0000;">'</span>,<span style="color: #800000; font-weight: bold;">30000</span>,<span style="color: #800000; font-weight: bold;">401</span>,<span style="color: #800000; font-weight: bold;">1</span><span style="color: #000000;">),
(</span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">成龙</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">male</span><span style="color: #ff0000;">'</span>,<span style="color: #800000; font-weight: bold;">48</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">20101111</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">teacher</span><span style="color: #ff0000;">'</span>,<span style="color: #800000; font-weight: bold;">10000</span>,<span style="color: #800000; font-weight: bold;">401</span>,<span style="color: #800000; font-weight: bold;">1</span><span style="color: #000000;">),

(</span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">歪歪</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">female</span><span style="color: #ff0000;">'</span>,<span style="color: #800000; font-weight: bold;">48</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">20150311</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">sale</span><span style="color: #ff0000;">'</span>,<span style="color: #800000; font-weight: bold;">3000.13</span>,<span style="color: #800000; font-weight: bold;">402</span>,<span style="color: #800000; font-weight: bold;">2</span><span style="color: #000000;">),#以下是销售部门
(</span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">丫丫</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">female</span><span style="color: #ff0000;">'</span>,<span style="color: #800000; font-weight: bold;">38</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">20101101</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">sale</span><span style="color: #ff0000;">'</span>,<span style="color: #800000; font-weight: bold;">2000.35</span>,<span style="color: #800000; font-weight: bold;">402</span>,<span style="color: #800000; font-weight: bold;">2</span><span style="color: #000000;">),
(</span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">丁丁</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">female</span><span style="color: #ff0000;">'</span>,<span style="color: #800000; font-weight: bold;">18</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">20110312</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">sale</span><span style="color: #ff0000;">'</span>,<span style="color: #800000; font-weight: bold;">1000.37</span>,<span style="color: #800000; font-weight: bold;">402</span>,<span style="color: #800000; font-weight: bold;">2</span><span style="color: #000000;">),
(</span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">星星</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">female</span><span style="color: #ff0000;">'</span>,<span style="color: #800000; font-weight: bold;">18</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">20160513</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">sale</span><span style="color: #ff0000;">'</span>,<span style="color: #800000; font-weight: bold;">3000.29</span>,<span style="color: #800000; font-weight: bold;">402</span>,<span style="color: #800000; font-weight: bold;">2</span><span style="color: #000000;">),
(</span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">格格</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">female</span><span style="color: #ff0000;">'</span>,<span style="color: #800000; font-weight: bold;">28</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">20170127</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">sale</span><span style="color: #ff0000;">'</span>,<span style="color: #800000; font-weight: bold;">4000.33</span>,<span style="color: #800000; font-weight: bold;">402</span>,<span style="color: #800000; font-weight: bold;">2</span><span style="color: #000000;">),

(</span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">张野</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">male</span><span style="color: #ff0000;">'</span>,<span style="color: #800000; font-weight: bold;">28</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">20160311</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">operation</span><span style="color: #ff0000;">'</span>,<span style="color: #800000; font-weight: bold;">10000.13</span>,<span style="color: #800000; font-weight: bold;">403</span>,<span style="color: #800000; font-weight: bold;">3</span><span style="color: #000000;">), #以下是运营部门
(</span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">程咬金</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">male</span><span style="color: #ff0000;">'</span>,<span style="color: #800000; font-weight: bold;">18</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">19970312</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">operation</span><span style="color: #ff0000;">'</span>,<span style="color: #800000; font-weight: bold;">20000</span>,<span style="color: #800000; font-weight: bold;">403</span>,<span style="color: #800000; font-weight: bold;">3</span><span style="color: #000000;">),
(</span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">程咬银</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">female</span><span style="color: #ff0000;">'</span>,<span style="color: #800000; font-weight: bold;">18</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">20130311</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">operation</span><span style="color: #ff0000;">'</span>,<span style="color: #800000; font-weight: bold;">19000</span>,<span style="color: #800000; font-weight: bold;">403</span>,<span style="color: #800000; font-weight: bold;">3</span><span style="color: #000000;">),
(</span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">程咬铜</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">male</span><span style="color: #ff0000;">'</span>,<span style="color: #800000; font-weight: bold;">18</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">20150411</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">operation</span><span style="color: #ff0000;">'</span>,<span style="color: #800000; font-weight: bold;">18000</span>,<span style="color: #800000; font-weight: bold;">403</span>,<span style="color: #800000; font-weight: bold;">3</span><span style="color: #000000;">),
(</span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">程咬铁</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">female</span><span style="color: #ff0000;">'</span>,<span style="color: #800000; font-weight: bold;">18</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">20140512</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">operation</span><span style="color: #ff0000;">'</span>,<span style="color: #800000; font-weight: bold;">17000</span>,<span style="color: #800000; font-weight: bold;">403</span>,<span style="color: #800000; font-weight: bold;">3</span><span style="color: #000000;">)
;

#ps：如果在windows系统中，插入中文字符，select的结果为空白，可以将所有字符编码统一设置成gbk</span>
```

表数据

#### 简单查询：

```
<span style="color: #000000;">#简单查询
    </span><span style="color: #0000ff;">SELECT</span><span style="color: #000000;"> id,name,sex,age,hire_date,post,post_comment,salary,office,depart_id 
    </span><span style="color: #0000ff;">FROM</span><span style="color: #000000;"> employee;

    </span><span style="color: #0000ff;">SELECT</span> <span style="color: #808080;">*</span> <span style="color: #0000ff;">FROM</span><span style="color: #000000;"> employee;

    </span><span style="color: #0000ff;">SELECT</span> name,salary <span style="color: #0000ff;">FROM</span><span style="color: #000000;"> employee;

#避免重复DISTINCT
    </span><span style="color: #0000ff;">SELECT</span> <span style="color: #0000ff;">DISTINCT</span> post <span style="color: #0000ff;">FROM</span><span style="color: #000000;"> employee;    

#通过四则运算查询
    </span><span style="color: #0000ff;">SELECT</span> name, salary<span style="color: #808080;">*</span><span style="color: #800000; font-weight: bold;">12</span> <span style="color: #0000ff;">FROM</span><span style="color: #000000;"> employee;
    </span><span style="color: #0000ff;">SELECT</span> name, salary<span style="color: #808080;">*</span><span style="color: #800000; font-weight: bold;">12</span> <span style="color: #0000ff;">AS</span> Annual_salary <span style="color: #0000ff;">FROM</span><span style="color: #000000;"> employee;
    </span><span style="color: #0000ff;">SELECT</span> name, salary<span style="color: #808080;">*</span><span style="color: #800000; font-weight: bold;">12</span> Annual_salary <span style="color: #0000ff;">FROM</span><span style="color: #000000;"> employee;

#定义显示格式
   CONCAT() 函数用于连接字符串
   </span><span style="color: #0000ff;">SELECT</span> CONCAT(<span style="color: #ff0000;">'</span><span style="color: #ff0000;">姓名: </span><span style="color: #ff0000;">'</span>,name,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">  年薪: </span><span style="color: #ff0000;">'</span>, salary<span style="color: #808080;">*</span><span style="color: #800000; font-weight: bold;">12</span>)  <span style="color: #0000ff;">AS</span><span style="color: #000000;"> Annual_salary 
   </span><span style="color: #0000ff;">FROM</span><span style="color: #000000;"> employee;
   
   CONCAT_WS() 第一个参数为分隔符
   </span><span style="color: #0000ff;">SELECT</span> CONCAT_WS(<span style="color: #ff0000;">'</span><span style="color: #ff0000;">:</span><span style="color: #ff0000;">'</span>,name,salary<span style="color: #808080;">*</span><span style="color: #800000; font-weight: bold;">12</span>)  <span style="color: #0000ff;">AS</span><span style="color: #000000;"> Annual_salary 
   </span><span style="color: #0000ff;">FROM</span><span style="color: #000000;"> employee;

   结合CASE语句：
   </span><span style="color: #0000ff;">SELECT</span><span style="color: #000000;">
       (
           </span><span style="color: #ff00ff;">CASE</span>
           <span style="color: #0000ff;">WHEN</span> NAME <span style="color: #808080;">=</span> <span style="color: #ff0000;">'</span><span style="color: #ff0000;">egon</span><span style="color: #ff0000;">'</span> <span style="color: #0000ff;">THEN</span><span style="color: #000000;">
               NAME
           </span><span style="color: #0000ff;">WHEN</span> NAME <span style="color: #808080;">=</span> <span style="color: #ff0000;">'</span><span style="color: #ff0000;">alex</span><span style="color: #ff0000;">'</span> <span style="color: #0000ff;">THEN</span><span style="color: #000000;">
               CONCAT(name,</span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">_BIGSB</span><span style="color: #ff0000;">'</span><span style="color: #000000;">)
           </span><span style="color: #0000ff;">ELSE</span><span style="color: #000000;">
               concat(NAME, </span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">SB</span><span style="color: #ff0000;">'</span><span style="color: #000000;">)
           </span><span style="color: #0000ff;">END</span><span style="color: #000000;">
       ) </span><span style="color: #0000ff;">as</span><span style="color: #000000;"> new_name
   </span><span style="color: #0000ff;">FROM</span><span style="color: #000000;">
       emp;</span>
```

#### 1.where约束：

where字句中可以使用：
　　1. 比较运算符：> < >= <= <> !=  
　　2. between 80 and 100 值在10到20之间  
　　3. in(10,20,30) 值是10或20或30  
　　4. like 'abc%'  
   　　 %表示任意多字符  
   　　 _表示一个字符   
　　5. 逻辑运算符：在多个条件直接可以使用逻辑运算符 not and or 

```
#<span style="color: #800000; font-weight: bold;">1</span><span style="color: #000000;">:单条件查询
    </span><span style="color: #0000ff;">SELECT</span> name <span style="color: #0000ff;">FROM</span><span style="color: #000000;"> employee
        </span><span style="color: #0000ff;">WHERE</span> post<span style="color: #808080;">=</span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">sale</span><span style="color: #ff0000;">'</span><span style="color: #000000;">;
        
#</span><span style="color: #800000; font-weight: bold;">2</span><span style="color: #000000;">:多条件查询
    </span><span style="color: #0000ff;">SELECT</span> name,salary <span style="color: #0000ff;">FROM</span><span style="color: #000000;"> employee
        </span><span style="color: #0000ff;">WHERE</span> post<span style="color: #808080;">=</span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">teacher</span><span style="color: #ff0000;">'</span> <span style="color: #808080;">AND</span> salary<span style="color: #808080;">></span><span style="color: #800000; font-weight: bold;">10000</span><span style="color: #000000;">;

#</span><span style="color: #800000; font-weight: bold;">3</span>:关键字BETWEEN <span style="color: #808080;">AND</span>
    <span style="color: #0000ff;">SELECT</span> name,salary <span style="color: #0000ff;">FROM</span><span style="color: #000000;"> employee 
        </span><span style="color: #0000ff;">WHERE</span> salary <span style="color: #808080;">BETWEEN</span> <span style="color: #800000; font-weight: bold;">10000</span> <span style="color: #808080;">AND</span> <span style="color: #800000; font-weight: bold;">20000</span><span style="color: #000000;">;

    </span><span style="color: #0000ff;">SELECT</span> name,salary <span style="color: #0000ff;">FROM</span><span style="color: #000000;"> employee 
        </span><span style="color: #0000ff;">WHERE</span> salary <span style="color: #808080;">NOT</span> <span style="color: #808080;">BETWEEN</span> <span style="color: #800000; font-weight: bold;">10000</span> <span style="color: #808080;">AND</span> <span style="color: #800000; font-weight: bold;">20000</span><span style="color: #000000;">;
    
#</span><span style="color: #800000; font-weight: bold;">4</span>:关键字IS <span style="color: #0000ff;">NULL</span><span style="color: #000000;">(判断某个字段是否为NULL不能用等号，需要用IS)
    </span><span style="color: #0000ff;">SELECT</span> name,post_comment <span style="color: #0000ff;">FROM</span><span style="color: #000000;"> employee 
        </span><span style="color: #0000ff;">WHERE</span> post_comment <span style="color: #0000ff;">IS</span> <span style="color: #0000ff;">NULL</span><span style="color: #000000;">;

    </span><span style="color: #0000ff;">SELECT</span> name,post_comment <span style="color: #0000ff;">FROM</span><span style="color: #000000;"> employee 
        </span><span style="color: #0000ff;">WHERE</span> post_comment <span style="color: #0000ff;">IS</span> <span style="color: #808080;">NOT</span> <span style="color: #0000ff;">NULL</span><span style="color: #000000;">;
        
    </span><span style="color: #0000ff;">SELECT</span> name,post_comment <span style="color: #0000ff;">FROM</span><span style="color: #000000;"> employee 
        </span><span style="color: #0000ff;">WHERE</span> post_comment<span style="color: #808080;">=</span><span style="color: #ff0000;">''</span>; 注意<span style="color: #ff0000;">''</span><span style="color: #000000;">是空字符串，不是null
    ps：
        执行
        </span><span style="color: #0000ff;">update</span> employee <span style="color: #0000ff;">set</span> post_comment<span style="color: #808080;">=</span><span style="color: #ff0000;">''</span> <span style="color: #0000ff;">where</span> id<span style="color: #808080;">=</span><span style="color: #800000; font-weight: bold;">2</span><span style="color: #000000;">;
        再用上条查看，就会有结果了

#</span><span style="color: #800000; font-weight: bold;">5</span><span style="color: #000000;">:关键字IN集合查询
    </span><span style="color: #0000ff;">SELECT</span> name,salary <span style="color: #0000ff;">FROM</span><span style="color: #000000;"> employee 
        </span><span style="color: #0000ff;">WHERE</span> salary<span style="color: #808080;">=</span><span style="color: #800000; font-weight: bold;">3000</span> <span style="color: #808080;">OR</span> salary<span style="color: #808080;">=</span><span style="color: #800000; font-weight: bold;">3500</span> <span style="color: #808080;">OR</span> salary<span style="color: #808080;">=</span><span style="color: #800000; font-weight: bold;">4000</span> <span style="color: #808080;">OR</span> salary<span style="color: #808080;">=</span><span style="color: #800000; font-weight: bold;">9000</span><span style="color: #000000;"> ;
    
    </span><span style="color: #0000ff;">SELECT</span> name,salary <span style="color: #0000ff;">FROM</span><span style="color: #000000;"> employee 
        </span><span style="color: #0000ff;">WHERE</span> salary <span style="color: #808080;">IN</span> (<span style="color: #800000; font-weight: bold;">3000</span>,<span style="color: #800000; font-weight: bold;">3500</span>,<span style="color: #800000; font-weight: bold;">4000</span>,<span style="color: #800000; font-weight: bold;">9000</span><span style="color: #000000;">) ;

    </span><span style="color: #0000ff;">SELECT</span> name,salary <span style="color: #0000ff;">FROM</span><span style="color: #000000;"> employee 
        </span><span style="color: #0000ff;">WHERE</span> salary <span style="color: #808080;">NOT</span> <span style="color: #808080;">IN</span> (<span style="color: #800000; font-weight: bold;">3000</span>,<span style="color: #800000; font-weight: bold;">3500</span>,<span style="color: #800000; font-weight: bold;">4000</span>,<span style="color: #800000; font-weight: bold;">9000</span><span style="color: #000000;">) ;

#</span><span style="color: #800000; font-weight: bold;">6</span><span style="color: #000000;">:关键字LIKE模糊查询
    通配符’</span><span style="color: #808080;">%</span><span style="color: #000000;">’
    </span><span style="color: #0000ff;">SELECT</span> <span style="color: #808080;">*</span> <span style="color: #0000ff;">FROM</span><span style="color: #000000;"> employee 
            </span><span style="color: #0000ff;">WHERE</span> name <span style="color: #808080;">LIKE</span> <span style="color: #ff0000;">'</span><span style="color: #ff0000;">eg%</span><span style="color: #ff0000;">'</span><span style="color: #000000;">;

    通配符’_’
    </span><span style="color: #0000ff;">SELECT</span> <span style="color: #808080;">*</span> <span style="color: #0000ff;">FROM</span><span style="color: #000000;"> employee 
            </span><span style="color: #0000ff;">WHERE</span> name <span style="color: #808080;">LIKE</span> <span style="color: #ff0000;">'</span><span style="color: #ff0000;">al__</span><span style="color: #ff0000;">'</span>;
```

#### 2.正则表达式查询：

```
<span style="color: #0000ff;">SELECT</span> <span style="color: #808080;">*</span> <span style="color: #0000ff;">FROM</span> employee <span style="color: #0000ff;">WHERE</span> name REGEXP <span style="color: #ff0000;">'</span><span style="color: #ff0000;">on$</span><span style="color: #ff0000;">'</span><span style="color: #000000;">;
除了模糊查询 还可以使用正则表达式查询
小结：对字符串匹配的方式
</span><span style="color: #0000ff;">WHERE</span> name <span style="color: #808080;">=</span> <span style="color: #ff0000;">'</span><span style="color: #ff0000;">onclose</span><span style="color: #ff0000;">'</span><span style="color: #000000;">;
</span><span style="color: #0000ff;">WHERE</span> name <span style="color: #808080;">LIKE</span> <span style="color: #ff0000;">'</span><span style="color: #ff0000;">on%</span><span style="color: #ff0000;">'</span><span style="color: #000000;">;
</span><span style="color: #0000ff;">WHERE</span> name REGEXP <span style="color: #ff0000;">'</span><span style="color: #ff0000;">on$</span><span style="color: #ff0000;">'</span>;
```

#### 3.group by 分组：

```
#查看MySQL <span style="color: #800000; font-weight: bold;">5</span><span style="color: #000000;">.7默认的sql_mode如下：
mysql</span><span style="color: #808080;">></span> <span style="color: #0000ff;">select</span> <span style="color: #008000; font-weight: bold;">@@global</span><span style="color: #000000;">.sql_mode;
ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION

#！！！注意
ONLY_FULL_GROUP_BY的语义就是确定select target list中的所有列的值都是明确语义，简单的说来，在ONLY_FULL_GROUP_BY模式下，target list中的值要么是来自于聚集函数的结果，要么是来自于group </span><span style="color: #0000ff;">by</span><span style="color: #000000;"> list中的表达式的值。

#设置sql_mole如下操作(我们可以去掉ONLY_FULL_GROUP_BY模式)：
mysql</span><span style="color: #808080;">></span> <span style="color: #0000ff;">set</span> global sql_mode<span style="color: #808080;">=</span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION</span><span style="color: #ff0000;">'</span>;
```

语法：

```
<span style="color: #000000;">单独使用GROUP BY关键字分组
    </span><span style="color: #0000ff;">SELECT</span> post <span style="color: #0000ff;">FROM</span> employee <span style="color: #0000ff;">GROUP</span> <span style="color: #0000ff;">BY</span><span style="color: #000000;"> post;
    注意：我们按照post字段分组，那么select查询的字段只能是post，想要获取组内的其他相关信息，需要借助函数

</span><span style="color: #0000ff;">GROUP</span><span style="color: #000000;"> BY关键字和GROUP_CONCAT()函数一起使用
    </span><span style="color: #0000ff;">SELECT</span> post,GROUP_CONCAT(name) <span style="color: #0000ff;">FROM</span> employee <span style="color: #0000ff;">GROUP</span> <span style="color: #0000ff;">BY</span><span style="color: #000000;"> post;#按照岗位分组，并查看组内成员名
    </span><span style="color: #0000ff;">SELECT</span> post,GROUP_CONCAT(name) <span style="color: #0000ff;">as</span> emp_members <span style="color: #0000ff;">FROM</span> employee <span style="color: #0000ff;">GROUP</span> <span style="color: #0000ff;">BY</span><span style="color: #000000;"> post;

</span><span style="color: #0000ff;">GROUP</span><span style="color: #000000;"> BY与聚合函数一起使用
    </span><span style="color: #0000ff;">select</span> post,<span style="color: #ff00ff;">count</span>(id) <span style="color: #0000ff;">as</span> <span style="color: #ff00ff;">count</span> <span style="color: #0000ff;">from</span> employee <span style="color: #0000ff;">group</span> <span style="color: #0000ff;">by</span> post;#按照岗位分组，并查看每个组有多少人
```

```
<span style="color: #000000;">#如果没有设置ONLY_FULL_GROUP_BY,也可以有结果，默认都是组内的第一条记录，但其实这是没有意义的

mysql</span><span style="color: #808080;">></span> <span style="color: #0000ff;">set</span> global sql_mode<span style="color: #808080;">=</span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">ONLY_FULL_GROUP_BY</span><span style="color: #ff0000;">'</span><span style="color: #000000;">;
Query OK, </span><span style="color: #800000; font-weight: bold;">0</span> rows affected (<span style="color: #800000; font-weight: bold;">0.00</span><span style="color: #000000;"> sec)

mysql</span><span style="color: #808080;">></span><span style="color: #000000;"> quit #设置成功后，一定要退出，然后重新登录方可生效
Bye

mysql</span><span style="color: #808080;">></span> <span style="color: #0000ff;">use</span><span style="color: #000000;"> db1;
</span><span style="color: #0000ff;">Database</span><span style="color: #000000;"> changed
mysql</span><span style="color: #808080;">></span> <span style="color: #0000ff;">select</span> <span style="color: #808080;">*</span> <span style="color: #0000ff;">from</span> emp <span style="color: #0000ff;">group</span> <span style="color: #0000ff;">by</span><span style="color: #000000;"> post; #报错
ERROR </span><span style="color: #800000; font-weight: bold;">1055</span> (<span style="color: #800000; font-weight: bold;">42000</span>): <span style="color: #ff0000;">'</span><span style="color: #ff0000;">db1.emp.id</span><span style="color: #ff0000;">'</span> isn<span style="color: #ff0000;">'</span><span style="color: #ff0000;">t in GROUP BY
mysql> select post,count(id) from emp group by post; #只能查看分组依据和使用聚合函数</span>
```

#### 4.聚合函数：

```
<span style="color: #000000;">#强调：聚合函数聚合的是组的内容，若是没有分组，则默认一组

示例：
    </span><span style="color: #0000ff;">SELECT</span> <span style="color: #ff00ff;">COUNT</span>(<span style="color: #808080;">*</span>) <span style="color: #0000ff;">FROM</span><span style="color: #000000;"> employee;
    </span><span style="color: #0000ff;">SELECT</span> <span style="color: #ff00ff;">COUNT</span>(<span style="color: #808080;">*</span>) <span style="color: #0000ff;">FROM</span> employee <span style="color: #0000ff;">WHERE</span> depart_id<span style="color: #808080;">=</span><span style="color: #800000; font-weight: bold;">1</span><span style="color: #000000;">;
    </span><span style="color: #0000ff;">SELECT</span> <span style="color: #ff00ff;">MAX</span>(salary) <span style="color: #0000ff;">FROM</span><span style="color: #000000;"> employee;
    </span><span style="color: #0000ff;">SELECT</span> <span style="color: #ff00ff;">MIN</span>(salary) <span style="color: #0000ff;">FROM</span><span style="color: #000000;"> employee;
    </span><span style="color: #0000ff;">SELECT</span> <span style="color: #ff00ff;">AVG</span>(salary) <span style="color: #0000ff;">FROM</span><span style="color: #000000;"> employee;
    </span><span style="color: #0000ff;">SELECT</span> <span style="color: #ff00ff;">SUM</span>(salary) <span style="color: #0000ff;">FROM</span><span style="color: #000000;"> employee;
    </span><span style="color: #0000ff;">SELECT</span> <span style="color: #ff00ff;">SUM</span>(salary) <span style="color: #0000ff;">FROM</span> employee <span style="color: #0000ff;">WHERE</span> depart_id<span style="color: #808080;">=</span><span style="color: #800000; font-weight: bold;">3</span>;
```

#### 5.having过滤：

“Where” 是一个约束声明，使用Where来约束来自数据库的数据，Where是在结果返回之前起作用的，且Where中不能使用聚合函数。
“Having”是一个过滤声明，是在查询返回结果集以后对查询结果进行的过滤操作，在Having中可以使用聚合函数。
执行优先级从高到低：where > group by > having
 
注：用having就一定要用group by， 用group by不一定要有having。只要条件里面的字段, 不是表里面原先有的字段就需要用having。

#### 6.order by 排序：

```
<span style="color: #000000;">按单列排序
    </span><span style="color: #0000ff;">SELECT</span> <span style="color: #808080;">*</span> <span style="color: #0000ff;">FROM</span> employee <span style="color: #0000ff;">ORDER</span> <span style="color: #0000ff;">BY</span><span style="color: #000000;"> salary;<span style="color: #ff6600;">#默认ASC升序
    </span></span><span style="color: #0000ff;">SELECT</span> <span style="color: #808080;">*</span> <span style="color: #0000ff;">FROM</span> employee <span style="color: #0000ff;">ORDER</span> <span style="color: #0000ff;">BY</span> salary <span style="color: #0000ff;">ASC</span><span style="color: #000000;">;
    </span><span style="color: #0000ff;">SELECT</span> <span style="color: #808080;">*</span> <span style="color: #0000ff;">FROM</span> employee <span style="color: #0000ff;">ORDER</span> <span style="color: #0000ff;">BY</span> salary <span style="color: #0000ff;">DESC</span><span style="color: #000000;">;#降序

按多列排序:先按照age排序，如果年纪相同，则按照薪资排序
    </span><span style="color: #0000ff;">SELECT</span> <span style="color: #808080;">*</span> <span style="color: #0000ff;">from</span><span style="color: #000000;"> employee
        </span><span style="color: #0000ff;">ORDER</span> <span style="color: #0000ff;">BY</span><span style="color: #000000;"> age,
        salary </span><span style="color: #0000ff;">DESC</span>;
```

#### 7.limit 限制查询记录数：

```
<span style="color: #000000;">示例：
    </span><span style="color: #0000ff;">SELECT</span> <span style="color: #808080;">*</span> <span style="color: #0000ff;">FROM</span> employee <span style="color: #0000ff;">ORDER</span> <span style="color: #0000ff;">BY</span> salary <span style="color: #0000ff;">DESC</span><span style="color: #000000;"> 
        LIMIT </span><span style="color: #800000; font-weight: bold;">3</span><span style="color: #000000;">;                    #默认初始位置为0 ，查询3条数据
    
    </span><span style="color: #0000ff;">SELECT</span> <span style="color: #808080;">*</span> <span style="color: #0000ff;">FROM</span> employee <span style="color: #0000ff;">ORDER</span> <span style="color: #0000ff;">BY</span> salary <span style="color: #0000ff;">DESC</span><span style="color: #000000;">
        LIMIT </span><span style="color: #800000; font-weight: bold;">0</span>,<span style="color: #800000; font-weight: bold;">5</span><span style="color: #000000;">; #从第0开始，即先查询出第一条，然后包含这一条在内往后查5条

    </span><span style="color: #0000ff;">SELECT</span> <span style="color: #808080;">*</span> <span style="color: #0000ff;">FROM</span> employee <span style="color: #0000ff;">ORDER</span> <span style="color: #0000ff;">BY</span> salary <span style="color: #0000ff;">DESC</span><span style="color: #000000;">
        LIMIT </span><span style="color: #800000; font-weight: bold;">5</span>,<span style="color: #800000; font-weight: bold;">5</span>; #从第5开始，即先查询出第6条，然后包含这一条在内往后查5条
```

### （二）、多表查询

```
<span style="color: #0000ff;">SELECT</span><span style="color: #000000;"> 字段列表
    </span><span style="color: #0000ff;">FROM</span> 表1 <span style="color: #0000ff;">INNER</span><span style="color: #808080;">|LEFT|RIGHT</span> <span style="color: #808080;">JOIN</span><span style="color: #000000;"> 表2
    </span><span style="color: #0000ff;">ON</span> 表1.字段 <span style="color: #808080;">=</span> 表2.字段;
```

#### 1.交叉连接（笛卡尔积）：

交叉连接返回左表中的所有行，左表中的每一行与右表中的所有行组合，结果集的行数是两个表的行数的乘积。（效率最低）

```
mysql<span style="color: #808080;">></span> <span style="color: #0000ff;">select</span> <span style="color: #808080;">*</span> <span style="color: #0000ff;">from</span><span style="color: #000000;"> employee,department;
</span><span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">--+------------+--------+------+--------+------+--------------+</span>
<span style="color: #808080;">|</span> id <span style="color: #808080;">|</span> name       <span style="color: #808080;">|</span> sex    <span style="color: #808080;">|</span> age  <span style="color: #808080;">|</span> dep_id <span style="color: #808080;">|</span> id   <span style="color: #808080;">|</span> name         <span style="color: #808080;">|</span>
<span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">--+------------+--------+------+--------+------+--------------+</span>
<span style="color: #808080;">|</span>  <span style="color: #800000; font-weight: bold;">1</span> <span style="color: #808080;">|</span> A       <span style="color: #808080;">|</span> male   <span style="color: #808080;">|</span>   <span style="color: #800000; font-weight: bold;">18</span> <span style="color: #808080;">|</span>    <span style="color: #800000; font-weight: bold;">200</span> <span style="color: #808080;">|</span>  <span style="color: #800000; font-weight: bold;">200</span> <span style="color: #808080;">|</span> 技术         <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span>  <span style="color: #800000; font-weight: bold;">1</span> <span style="color: #808080;">|</span> A       <span style="color: #808080;">|</span> male   <span style="color: #808080;">|</span>   <span style="color: #800000; font-weight: bold;">18</span> <span style="color: #808080;">|</span>    <span style="color: #800000; font-weight: bold;">200</span> <span style="color: #808080;">|</span>  <span style="color: #800000; font-weight: bold;">201</span> <span style="color: #808080;">|</span> 人力资源     <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span>  <span style="color: #800000; font-weight: bold;">1</span> <span style="color: #808080;">|</span> A       <span style="color: #808080;">|</span> male   <span style="color: #808080;">|</span>   <span style="color: #800000; font-weight: bold;">18</span> <span style="color: #808080;">|</span>    <span style="color: #800000; font-weight: bold;">200</span> <span style="color: #808080;">|</span>  <span style="color: #800000; font-weight: bold;">202</span> <span style="color: #808080;">|</span> 销售         <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span>  <span style="color: #800000; font-weight: bold;">1</span> <span style="color: #808080;">|</span> A       <span style="color: #808080;">|</span> male   <span style="color: #808080;">|</span>   <span style="color: #800000; font-weight: bold;">18</span> <span style="color: #808080;">|</span>    <span style="color: #800000; font-weight: bold;">200</span> <span style="color: #808080;">|</span>  <span style="color: #800000; font-weight: bold;">203</span> <span style="color: #808080;">|</span> 运营         <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span>  <span style="color: #800000; font-weight: bold;">2</span> <span style="color: #808080;">|</span> B       <span style="color: #808080;">|</span> female <span style="color: #808080;">|</span>   <span style="color: #800000; font-weight: bold;">48</span> <span style="color: #808080;">|</span>    <span style="color: #800000; font-weight: bold;">201</span> <span style="color: #808080;">|</span>  <span style="color: #800000; font-weight: bold;">200</span> <span style="color: #808080;">|</span> 技术         <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span>  <span style="color: #800000; font-weight: bold;">2</span> <span style="color: #808080;">|</span> B       <span style="color: #808080;">|</span> female <span style="color: #808080;">|</span>   <span style="color: #800000; font-weight: bold;">48</span> <span style="color: #808080;">|</span>    <span style="color: #800000; font-weight: bold;">201</span> <span style="color: #808080;">|</span>  <span style="color: #800000; font-weight: bold;">201</span> <span style="color: #808080;">|</span> 人力资源     <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span>  <span style="color: #800000; font-weight: bold;">2</span> <span style="color: #808080;">|</span> B       <span style="color: #808080;">|</span> female <span style="color: #808080;">|</span>   <span style="color: #800000; font-weight: bold;">48</span> <span style="color: #808080;">|</span>    <span style="color: #800000; font-weight: bold;">201</span> <span style="color: #808080;">|</span>  <span style="color: #800000; font-weight: bold;">202</span> <span style="color: #808080;">|</span> 销售         <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span>  <span style="color: #800000; font-weight: bold;">2</span> <span style="color: #808080;">|</span> B       <span style="color: #808080;">|</span> female <span style="color: #808080;">|</span>   <span style="color: #800000; font-weight: bold;">48</span> <span style="color: #808080;">|</span>    <span style="color: #800000; font-weight: bold;">201</span> <span style="color: #808080;">|</span>  <span style="color: #800000; font-weight: bold;">203</span> <span style="color: #808080;">|</span> 运营         <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span>  <span style="color: #800000; font-weight: bold;">3</span> <span style="color: #808080;">|</span> C       <span style="color: #808080;">|</span> male   <span style="color: #808080;">|</span>   <span style="color: #800000; font-weight: bold;">38</span> <span style="color: #808080;">|</span>    <span style="color: #800000; font-weight: bold;">201</span> <span style="color: #808080;">|</span>  <span style="color: #800000; font-weight: bold;">200</span> <span style="color: #808080;">|</span> 技术         <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span>  <span style="color: #800000; font-weight: bold;">3</span> <span style="color: #808080;">|</span> C       <span style="color: #808080;">|</span> male   <span style="color: #808080;">|</span>   <span style="color: #800000; font-weight: bold;">38</span> <span style="color: #808080;">|</span>    <span style="color: #800000; font-weight: bold;">201</span> <span style="color: #808080;">|</span>  <span style="color: #800000; font-weight: bold;">201</span> <span style="color: #808080;">|</span> 人力资源     <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span>  <span style="color: #800000; font-weight: bold;">3</span> <span style="color: #808080;">|</span> C       <span style="color: #808080;">|</span> male   <span style="color: #808080;">|</span>   <span style="color: #800000; font-weight: bold;">38</span> <span style="color: #808080;">|</span>    <span style="color: #800000; font-weight: bold;">201</span> <span style="color: #808080;">|</span>  <span style="color: #800000; font-weight: bold;">202</span> <span style="color: #808080;">|</span> 销售         <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span>  <span style="color: #800000; font-weight: bold;">3</span> <span style="color: #808080;">|</span> C       <span style="color: #808080;">|</span> male   <span style="color: #808080;">|</span>   <span style="color: #800000; font-weight: bold;">38</span> <span style="color: #808080;">|</span>    <span style="color: #800000; font-weight: bold;">201</span> <span style="color: #808080;">|</span>  <span style="color: #800000; font-weight: bold;">203</span> <span style="color: #808080;">|</span> 运营         <span style="color: #808080;">|</span>
<span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">--+------------+--------+------+--------+------+--------------+</span>
```

#### 2.内连接：只连接匹配的行

INNER JOIN（内连接），也成为自然连接，找两张表共有的部分，相当于利用条件从笛卡尔积结果中筛选出了正确的结果，内连接是从结果中删除其他被连接表中没有匹配行的所有行，所以内连接可能会丢失信息。

```
<span style="color: #000000;">mysql</span><span style="color: #808080;">></span> <span style="color: #0000ff;">select</span> employee.id,employee.name,employee.age,employee.sex,department.name <span style="color: #0000ff;">from</span> employee <span style="color: #0000ff;">inner</span> <span style="color: #808080;">join</span> department <span style="color: #0000ff;">on</span> employee.dep_id<span style="color: #808080;">=</span><span style="color: #000000;">department.id; 
</span><span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">--+-----------+------+--------+--------------+</span>
<span style="color: #808080;">|</span> id <span style="color: #808080;">|</span> name      <span style="color: #808080;">|</span> age  <span style="color: #808080;">|</span> sex    <span style="color: #808080;">|</span> name         <span style="color: #808080;">|</span>
<span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">--+-----------+------+--------+--------------+</span>
<span style="color: #808080;">|</span>  <span style="color: #800000; font-weight: bold;">1</span> <span style="color: #808080;">|</span> A     <span style="color: #808080;">|</span>   <span style="color: #800000; font-weight: bold;">18</span> <span style="color: #808080;">|</span> male   <span style="color: #808080;">|</span> 技术         <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span>  <span style="color: #800000; font-weight: bold;">2</span> <span style="color: #808080;">|</span> B     <span style="color: #808080;">|</span>   <span style="color: #800000; font-weight: bold;">48</span> <span style="color: #808080;">|</span> female <span style="color: #808080;">|</span> 人力资源     <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span>  <span style="color: #800000; font-weight: bold;">3</span> <span style="color: #808080;">|</span> C  <span style="color: #808080;">|</span>   <span style="color: #800000; font-weight: bold;">38</span> <span style="color: #808080;">|</span> male   <span style="color: #808080;">|</span> 人力资源     <span style="color: #808080;">|</span>
<span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">--+-----------+------+--------+--------------+</span>
<span style="color: #000000;">
#上述sql等同于
mysql</span><span style="color: #808080;">></span> <span style="color: #0000ff;">select</span> employee.id,employee.name,employee.age,employee.sex,department.name <span style="color: #0000ff;">from</span> employee,department <span style="color: #0000ff;">where</span> employee.dep_id<span style="color: #808080;">=</span>department.id;
```

#### 3.外连接之左连接：

返回左表中的所有行，如果左表中行在右表中没有匹配行，则结果中右表中的列返回空值。

```
<span style="color: #0000ff;">select</span> employee.id,employee.name,department.name <span style="color: #0000ff;">as</span> depart_name <span style="color: #0000ff;">from</span> employee <span style="color: #808080;">left</span> <span style="color: #808080;">join</span> department <span style="color: #0000ff;">on</span> employee.dep_id<span style="color: #808080;">=</span>department.id;
```

#### 4.外连接之右连接：

与左连接正好相反，返回右表中的所有行，如果右表中行在左表中没有匹配行，则结果中左表中的列返回空值。

```
<span style="color: #0000ff;">select</span> employee.id,employee.name,department.name <span style="color: #0000ff;">as</span> depart_name <span style="color: #0000ff;">from</span> employee <span style="color: #808080;">right</span> <span style="color: #808080;">join</span> department <span style="color: #0000ff;">on</span> employee.dep_id<span style="color: #808080;">=</span>department.id;
```

#### 5.全外连接：显示左右两个表全部记录

在内连接的基础上增加左边有右边没有的，右边有左边没有的。是左外连接和右外连接的并集。
注：mysql不支持全外连接full join 但是可以用下面的方式实现（union与union all的区别：union会去掉相同的纪录）

```
<span style="color: #0000ff;">select</span> <span style="color: #808080;">*</span> <span style="color: #0000ff;">from</span> employee <span style="color: #808080;">left</span> <span style="color: #808080;">join</span> department <span style="color: #0000ff;">on</span> employee.dep_id <span style="color: #808080;">=</span><span style="color: #000000;"> department.id
</span><span style="color: #0000ff;">union</span>
<span style="color: #0000ff;">select</span> <span style="color: #808080;">*</span> <span style="color: #0000ff;">from</span> employee <span style="color: #808080;">right</span> <span style="color: #808080;">join</span> department <span style="color: #0000ff;">on</span> employee.dep_id <span style="color: #808080;">=</span><span style="color: #000000;"> department.id
;</span>
```

### （三）、子查询

```
#<span style="color: #800000; font-weight: bold;">1</span><span style="color: #000000;">：子查询是将一个查询语句嵌套在另一个查询语句中。
#</span><span style="color: #800000; font-weight: bold;">2</span><span style="color: #000000;">：内层查询语句的查询结果，可以为外层查询语句提供查询条件。
#</span><span style="color: #800000; font-weight: bold;">3</span>：子查询中可以包含：<span style="color: #808080;">IN</span>、<span style="color: #808080;">NOT</span> <span style="color: #808080;">IN</span>、<span style="color: #808080;">ANY</span>、<span style="color: #808080;">ALL</span>、<span style="color: #808080;">EXISTS</span> 和 <span style="color: #808080;">NOT</span><span style="color: #000000;"> EXISTS等关键字
#</span><span style="color: #800000; font-weight: bold;">4</span>：还可以包含比较运算符：<span style="color: #808080;">=</span> 、 <span style="color: #808080;">!=</span>、<span style="color: #808080;">></span> 、<span style="color: #808080;"><</span>等
```

#### 1.带in关键字的子查询

```
<span style="color: #000000;">#查询平均年龄在25岁以上的部门名
</span><span style="color: #0000ff;">select</span> id,name <span style="color: #0000ff;">from</span><span style="color: #000000;"> department
    </span><span style="color: #0000ff;">where</span> id <span style="color: #808080;">in</span><span style="color: #000000;"> 
        (</span><span style="color: #0000ff;">select</span> dep_id <span style="color: #0000ff;">from</span> employee <span style="color: #0000ff;">group</span> <span style="color: #0000ff;">by</span> dep_id <span style="color: #0000ff;">having</span> <span style="color: #ff00ff;">avg</span>(age) <span style="color: #808080;">></span> <span style="color: #800000; font-weight: bold;">25</span><span style="color: #000000;">);

#查看技术部员工姓名
</span><span style="color: #0000ff;">select</span> name <span style="color: #0000ff;">from</span><span style="color: #000000;"> employee
    </span><span style="color: #0000ff;">where</span> dep_id <span style="color: #808080;">in</span><span style="color: #000000;"> 
        (</span><span style="color: #0000ff;">select</span> id <span style="color: #0000ff;">from</span> department <span style="color: #0000ff;">where</span> name<span style="color: #808080;">=</span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">技术</span><span style="color: #ff0000;">'</span><span style="color: #000000;">);

#查看不足1人的部门名(子查询得到的是有人的部门id)
</span><span style="color: #0000ff;">select</span> name <span style="color: #0000ff;">from</span> department <span style="color: #0000ff;">where</span> id <span style="color: #808080;">not</span> <span style="color: #808080;">in</span> (<span style="color: #0000ff;">select</span> <span style="color: #0000ff;">distinct</span> dep_id <span style="color: #0000ff;">from</span> employee);
```

#### 2.带比较运算符的子查询

```
<span style="color: #000000;">#查询大于部门内平均年龄的员工名、年龄
</span><span style="color: #0000ff;">select</span> t1.name,t1.age <span style="color: #0000ff;">from</span><span style="color: #000000;"> emp t1
</span><span style="color: #0000ff;">inner</span> <span style="color: #808080;">join</span><span style="color: #000000;"> 
(</span><span style="color: #0000ff;">select</span> dep_id,<span style="color: #ff00ff;">avg</span>(age) avg_age <span style="color: #0000ff;">from</span> emp <span style="color: #0000ff;">group</span> <span style="color: #0000ff;">by</span><span style="color: #000000;"> dep_id) t2
</span><span style="color: #0000ff;">on</span> t1.dep_id <span style="color: #808080;">=</span><span style="color: #000000;"> t2.dep_id
</span><span style="color: #0000ff;">where</span> t1.age <span style="color: #808080;">></span> t2.avg_age; 
```

#### 3.带exists关键字的子查询

```
#department表中存在dept_id<span style="color: #808080;">=</span><span style="color: #800000; font-weight: bold;">203</span><span style="color: #000000;">，Ture
mysql</span><span style="color: #808080;">></span> <span style="color: #0000ff;">select</span> <span style="color: #808080;">*</span> <span style="color: #0000ff;">from</span><span style="color: #000000;"> employee
    </span><span style="color: #808080;">-></span>     <span style="color: #0000ff;">where</span> <span style="color: #808080;">exists</span>
    <span style="color: #808080;">-></span>         (<span style="color: #0000ff;">select</span> id <span style="color: #0000ff;">from</span> department <span style="color: #0000ff;">where</span> id<span style="color: #808080;">=</span><span style="color: #800000; font-weight: bold;">200</span><span style="color: #000000;">);

#department表中存在dept_id</span><span style="color: #808080;">=</span><span style="color: #800000; font-weight: bold;">205</span><span style="color: #000000;">，False
mysql</span><span style="color: #808080;">></span> <span style="color: #0000ff;">select</span> <span style="color: #808080;">*</span> <span style="color: #0000ff;">from</span><span style="color: #000000;"> employee
    </span><span style="color: #808080;">-></span>     <span style="color: #0000ff;">where</span> <span style="color: #808080;">exists</span>
    <span style="color: #808080;">-></span>         (<span style="color: #0000ff;">select</span> id <span style="color: #0000ff;">from</span> department <span style="color: #0000ff;">where</span> id<span style="color: #808080;">=</span><span style="color: #800000; font-weight: bold;">204</span><span style="color: #000000;">);
Empty </span><span style="color: #0000ff;">set</span> (<span style="color: #800000; font-weight: bold;">0.00</span> sec)
```

## 六、权限管理

```
<span style="color: #000000;">#授权表
</span><span style="color: #ff00ff;">user</span><span style="color: #000000;"> #该表放行的权限，针对：所有数据，所有库下所有表，以及表下的所有字段
db #该表放行的权限，针对：某一数据库，该数据库下的所有表，以及表下的所有字段
tables_priv #该表放行的权限。针对：某一张表，以及该表下的所有字段
columns_priv #该表放行的权限，针对：某一个字段<br/><br/>#查看自己的权限<br/>show grants;

#创建用户
</span><span style="color: #0000ff;">create</span> <span style="color: #ff00ff;">user</span>  用户名@"主机地址" identified <span style="color: #0000ff;">by</span><span style="color: #000000;"> "密码";

</span><span style="color: #0000ff;">create</span> <span style="color: #ff00ff;">user</span> <span style="color: #ff0000;">'</span><span style="color: #ff0000;">A</span><span style="color: #ff0000;">'</span>@<span style="color: #ff0000;">'</span><span style="color: #ff0000;">1127.0.0.1</span><span style="color: #ff0000;">'</span> identified <span style="color: #0000ff;">by</span> <span style="color: #ff0000;">'</span><span style="color: #ff0000;">123</span><span style="color: #ff0000;">'</span><span style="color: #000000;">;
</span><span style="color: #0000ff;">create</span> <span style="color: #ff00ff;">user</span> <span style="color: #ff0000;">'</span><span style="color: #ff0000;">B</span><span style="color: #ff0000;">'</span>@<span style="color: #ff0000;">'</span><span style="color: #ff0000;">192.168.1.%</span><span style="color: #ff0000;">'</span> identified <span style="color: #0000ff;">by</span> <span style="color: #ff0000;">'</span><span style="color: #ff0000;">123</span><span style="color: #ff0000;">'</span><span style="color: #000000;">;#指定ip
</span><span style="color: #0000ff;">create</span> <span style="color: #ff00ff;">user</span> <span style="color: #ff0000;">'</span><span style="color: #ff0000;">C</span><span style="color: #ff0000;">'</span>@<span style="color: #ff0000;">'</span><span style="color: #ff0000;">%</span><span style="color: #ff0000;">'</span> identified <span style="color: #0000ff;">by</span> <span style="color: #ff0000;">'</span><span style="color: #ff0000;">123</span><span style="color: #ff0000;">'</span><span style="color: #000000;">;#任意用户

#授权：对文件夹，对文件，对文件某一字段的权限
查看帮助：help </span><span style="color: #0000ff;">grant</span><span style="color: #000000;">
常用权限有：</span><span style="color: #0000ff;">select</span>,<span style="color: #0000ff;">update</span>,<span style="color: #0000ff;">alter</span>,<span style="color: #0000ff;">delete</span><span style="color: #000000;">
all可以代表除了grant之外的所有权限
语法: </span><span style="color: #0000ff;">grant</span> <span style="color: #ff0000;">[</span><span style="color: #ff0000;">权限的名称 select insert.... | all </span><span style="color: #ff0000;">]</span> <span style="color: #0000ff;">on</span> 数据库.表名  <span style="color: #0000ff;">to</span> 用户名<span style="color: #008000;">@主机地址</span><span style="color: #000000;">;
<strong>特点: 如果授权时，用户不存在则直接自动创建用户</strong>

#针对所有库的授权:</span><span style="color: #808080;">*</span>.<span style="color: #808080;">*</span>
<span style="color: #0000ff;">grant</span> <span style="color: #0000ff;">select</span> <span style="color: #0000ff;">on</span> <span style="color: #808080;">*</span>.<span style="color: #808080;">*</span> <span style="color: #0000ff;">to</span> <span style="color: #ff0000;">'</span><span style="color: #ff0000;">A</span><span style="color: #ff0000;">'</span>@<span style="color: #ff0000;">'</span><span style="color: #ff0000;">localhost</span><span style="color: #ff0000;">'</span> identified <span style="color: #0000ff;">by</span> <span style="color: #ff0000;">'</span><span style="color: #ff0000;">123</span><span style="color: #ff0000;">'</span><span style="color: #000000;">; #只在user表中可以查到A用户的select权限被设置为Y<br/>没有 Grant_priv（授权）的权限

#针对某一数据库：db1.</span><span style="color: #808080;">*</span>
<span style="color: #0000ff;">grant</span> <span style="color: #0000ff;">select</span> <span style="color: #0000ff;">on</span> db1.<span style="color: #808080;">*</span> <span style="color: #0000ff;">to</span> <span style="color: #ff0000;">'</span><span style="color: #ff0000;">B</span><span style="color: #ff0000;">'</span>@<span style="color: #ff0000;">'</span><span style="color: #ff0000;">%</span><span style="color: #ff0000;">'</span> identified <span style="color: #0000ff;">by</span> <span style="color: #ff0000;">'</span><span style="color: #ff0000;">123</span><span style="color: #ff0000;">'</span><span style="color: #000000;">; #只在db表中可以查到B用户的select权限被设置为Y

#针对某一个表：db1.t1
</span><span style="color: #0000ff;">grant</span> <span style="color: #0000ff;">select</span> <span style="color: #0000ff;">on</span> db1.t1 <span style="color: #0000ff;">to</span> <span style="color: #ff0000;">'</span><span style="color: #ff0000;">C</span><span style="color: #ff0000;">'</span>@<span style="color: #ff0000;">'</span><span style="color: #ff0000;">%</span><span style="color: #ff0000;">'</span> identified <span style="color: #0000ff;">by</span> <span style="color: #ff0000;">'</span><span style="color: #ff0000;">123</span><span style="color: #ff0000;">'</span><span style="color: #000000;">;  #只在tables_priv表中可以查到C用户的select权限

#针对某一个字段：
</span><span style="color: #0000ff;">grant</span> <span style="color: #0000ff;">select</span> (id,name),<span style="color: #0000ff;">update</span> (age) <span style="color: #0000ff;">on</span> db1.t3 <span style="color: #0000ff;">to</span> <span style="color: #ff0000;">'</span><span style="color: #ff0000;">D</span><span style="color: #ff0000;">'</span>@<span style="color: #ff0000;">'</span><span style="color: #ff0000;">localhost</span><span style="color: #ff0000;">'</span> identified <span style="color: #0000ff;">by</span> <span style="color: #ff0000;">'</span><span style="color: #ff0000;">123</span><span style="color: #ff0000;">'</span><span style="color: #000000;">; 
#可以在tables_priv和columns_priv中看到相应的权限

#删除权限
</span><span style="color: #0000ff;">revoke</span> 权限的名称 <span style="color: #0000ff;">on</span> 数据库.表名  <span style="color: #0000ff;">from</span><span style="color: #000000;"> 用户名@"主机名" ;
</span><span style="color: #0000ff;">revoke</span> <span style="color: #0000ff;">select</span> <span style="color: #0000ff;">on</span> db1.<span style="color: #808080;">*</span> <span style="color: #0000ff;">from</span> <span style="color: #ff0000;">'</span><span style="color: #ff0000;">A</span><span style="color: #ff0000;">'</span>@<span style="color: #ff0000;">'</span><span style="color: #ff0000;">%</span><span style="color: #ff0000;">'</span><span style="color: #000000;">;

#删除用户
</span><span style="color: #0000ff;">drop</span> <span style="color: #ff00ff;">user</span> 用户名@"主机地址";
```

注：添加权限之后，要记得刷新权限

```
flush <span style="color: #0000ff;">privileges</span>;
```

如果要让用户拥有所有权限，可以执行下面的命令：

```
<span style="color: #0000ff;">grant</span> <span style="color: #ff0000;">[</span><span style="color: #ff0000;">权限的名称 select insert.... | all </span><span style="color: #ff0000;">]</span> <span style="color: #0000ff;">on</span> 数据库.表名  <span style="color: #0000ff;">to</span> 用户名<span style="color: #008000;">@主机地址</span> <span style="color: #0000ff;">with</span> <span style="color: #0000ff;">grant</span> <span style="color: #0000ff;">option</span><span style="color: #000000;">;
        </span><span style="color: #0000ff;">with</span> <span style="color: #0000ff;">grant</span> <span style="color: #0000ff;">option</span> 这个用户可以将他有的权限授予别的账户
```

 
