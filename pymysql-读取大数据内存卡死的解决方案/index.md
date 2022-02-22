# pymysql 读取大数据内存卡死的解决方案

> 背景：目前表中只有5G(后期持续增长)，但是其中一个字段(以下称为detail字段)存了2M(不一定2M，部分为0，平均下来就是2M)，字段中存的是一个数组，数组中存N个json数据。这个字段如下：
```json
[{"A": "A", "B": "B", "C": "C", "D": "D"}...]
```
> 要是拆表的话，可能要拆好多个，要是存多行根据阿里巴巴《Java 开发手册》提出单表行数超过 500 万行，也不是很建议。希望有大佬能指教一下。
> 回到正题，一开始是分两个表存储，一个表存基本信息(A表)，一个表(B表)存关联字段，及detail字段。貌似没有啥用，按需求现要将两张表合在一起供BI去处理。直接复制了那张基础字段的A表，通过遍历B表根据关联字段进行更新。但是在select的时候内存读入的数据太大直接卡死(狗头)。于是在网上查找如何通过pymysql处理大数据的问题。解决方案如下：

### 1.通过limit分批次读取数据进行操作：
```python
import pymysql

up_db = pymysql.connections.Connection(host=MYSQL_HOST,
                               port=MYSQL_PORT,
                               user=MYSQL_USER,
                               password=MYSQL_PASSWORD,
                               db=MYSQL_DB,
                               charset='utf8mb4',)
                               

count = 0
while True:
    # if count == 2:
    #     break

    select_sql = "select sec_report_id,detail from sec_report_original_data_detail limit %s,2"%(count)
    up_cursor = up_db.cursor()
    up_cursor.execute(select_sql)
    result = up_cursor.fetchall()
    for data in result:
        sec_report_id = data[0]
        detail = data[1]
        update_sql = "update `sec_report_original_data_intact` set detail = '%s' where `sec_report_id` = '%s' " % (
        db.escape_string(detail), sec_report_id)
        print(update_sql)
        res = up_cursor.execute(update_sql)
        if res:
            print(res)
            up_db.commit()
            print(f'{sec_report_id}插入成功')

    count+=2
```
> 可以解决问题，不过只是拿了几条做测试(我用的是第二种)，这里没写终止条件，有朋友要用的话自己加上。

### 2.通过pymysql的`SSCursor`没有缓存的游标
> `pymysql.cursors.SSCursor`代替默认的`cursor`会从数据库中一条一条的读取记录，从而不会造成内存卡死，但是也有需要注意的地方：

* 这个游标对象只能读完所有行之后才能处理其他sql。如果你需要并行执行sql，需要重新生成一个连接
* 必须一次性读完所有行，每次读取后处理数据要快，不能超过60s，否则mysql将会断开这次连接(没有遇到这个问题，遇到的可以讨论一下)

```python
import pymysql

db = pymysql.connections.Connection(host=MYSQL_HOST,
                               port=MYSQL_PORT,
                               user=MYSQL_USER,
                               password=MYSQL_PASSWORD,
                               db=MYSQL_DB,
                               charset='utf8mb4',
                               cursorclass=pymysql.cursors.SSDictCursor)

up_db = pymysql.connections.Connection(host=MYSQL_HOST,
                               port=MYSQL_PORT,
                               user=MYSQL_USER,
                               password=MYSQL_PASSWORD,
                               db=MYSQL_DB,
                               charset='utf8mb4',)
                               
up_cursor = up_db.cursor()
cursor = pymysql.cursors.SSCursor(db)
select_sql = "select sec_report_id,detail from sec_report_original_data_detail"
cursor.execute(select_sql)
result = cursor.fetchone()

try:
    while result is not None:
        sec_report_id = result[0]
        detail = result[1]
        update_sql = "update `sec_report_original_data_intact` set detail = '%s' where `sec_report_id` = '%s'"%(db.escape_string(detail),sec_report_id)
        res = up_cursor.execute(update_sql)

        if res:
            print(res)
            up_db.commit()
            print(f'{sec_report_id}插入成功')
        result = cursor.fetchone()
except Exception as e:
    print(e)
finally:
    up_cursor.close()
    cursor.close()
    db.close()
```
> 解决了一次性读取大数据的方法，但是没找到特别好的存储那个`detail`字段中数据的办法，有朋友了解的可以沟通一下。
