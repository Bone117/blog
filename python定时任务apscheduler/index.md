# python定时任务APScheduler

## APScheduler定时任务

> APScheduler 支持三种调度任务：固定时间间隔，固定时间点（日期），Linux 下的 Crontab 命令。同时，它还支持异步执行、后台执行调度任务。

### 一、基本架构

1. 触发器 triggers：设定触发任务的条件

   > 描述一个任务何时被触发，按日期或按时间间隔或按 cronjob 表达式三种方式触发

2. 任务存储器 job stores：存放任务，可以放内存(默认)或数据库

   > 注：调度器之间不能共享任务存储器

3. 执行器 executors：用于执行任务，可设定执行模式

   > 将指定的作业提交到线程池或者进程池中运行，任务完成通知调度器触发相应的事件。

4. 调度器 schedulers：将上方三个组件作为参数，创建调度器实例执行。

   > 协调三个组件的运行。

### 二、调度器组件(schedulers)

- `BlockingScheduler`阻塞式调度器

  > 调度程序是进程中唯一运行的进程，调用start函数会阻塞当前线程，不能立即返回。

```python
from apscheduler.schedulers.blocking import BlockingScheduler
import time

scheduler = BlockingScheduler()
 
def job1():
    print "%s: 执行任务"  % time.asctime()

scheduler.add_job(job1, 'interval', seconds=3)
scheduler.start()
```

- `BackgroundScheduler`后台调度器

  > 当前线程不会阻塞，调度器后台执行

```python
from apscheduler.schedulers.background import BackgroundScheduler
import time

scheduler = BackgroundScheduler()
 
def job1():
    print "%s: 执行任务"  % time.asctime()

scheduler.add_job(job1, 'interval', seconds=3)
scheduler.start()
time.sleep(10)
```

> 注：10秒执行完后，程序结束。

- `AsyncIOScheduler`AsyncIO调度器

  > 适用于使用了asyncio的情况

```python
from apscheduler.schedulers.asyncio import AsyncIOScheduler
import asyncio
...
...
try:
    asyncio.get_event_loop().run_forever()
except (KeyboardInterrupt, SystemExit):
    pass
```

- `GeventScheduler`Gevent调度器

  > 使用了Gevent的情况

```python
from apscheduler.schedulers.gevent import GeventScheduler
...
...
g = scheduler.start()
try:
    g.join()
except (KeyboardInterrupt, SystemExit):
    pass
```

- `TornadoScheduler`Tornado调度器

  > 适用于构建Tornado应用

- `TwistedScheduler` Twisted调度器

  > 适用于构建Twisted应用

- `QtScheduler` Qt调度器

  > 适用于构建Qt应用

### 三、触发器组件(trigger)

1. `date`：只在某个时间点执行一次，具体日期

   > `run_date(datetime|str)`
   >
   > ```python
   > scheduler.add_job(my_job, 'date', run_date=datetime(2019, 7, 12, 15, 30, 5), args=[])
   > scheduler.add_job(my_job, 'date', run_date="2019-07-12", args=[])
   > ```
   >
   > `timezone` 指定时区

2. `interval`：每隔一段时间允许一次，时间间隔

   > `weeks=0 | days=0 | hours=0 | minutes=0 | seconds=0, start_date=None, end_date=None, timezone=None`
   >
   > ```python
   > scheduler.add_job(my_job, 'interval', hours=2)
   > scheduler.add_job(my_job, 'interval', hours=2, start_date='2017-9-8 21:30:00', end_date='2018-06-15 21:30:00)
   > ```

3. `cron`：任务的运行周期

   > `(year=None, month=None, day=None, week=None, day_of_week=None, hour=None, minute=None, second=None, start_date=None, end_date=None, timezone=None)`
   >
   > 除了week和 day_of_week，它们的默认值是`*`
   >
   > 例如`day=1, minute=20`，这就等于`year='*', month='*', day=1, week='*', day_of_week='*', hour='*', minute=20, second=0`，工作将在每个月的第一天以每小时20分钟的时间执行

   表达式类型

   | 表达式 | 参数类型 | 描述                                     |
   | ------ | -------- | ---------------------------------------- |
   | *      | 所有     | 通配符。例：`minutes=*`即每分钟触发      |
   | */a    | 所有     | 可被a整除的通配符。                      |
   | a-b    | 所有     | 范围a-b触发                              |
   | a-b/c  | 所有     | 范围a-b，且可被c整除时触发               |
   | xth y  | 日       | 第几个星期几触发。x为第几个，y为星期几   |
   | last x | 日       | 一个月中，最后个星期几触发               |
   | last   | 日       | 一个月最后一天触发                       |
   | x,y,z  | 所有     | 组合表达式，可以组合确定值或上方的表达式 |

> 注：当设置的时间间隔小于，任务的执行时间，线程会阻塞住，等待执行完了才能执行下一个任务，可以设置`max_instance`指定一个任务同一时刻有多少个实例在运行，默认为1

### 四、配置调度器

转自：https://www.jianshu.com/p/4f5305e220f0

线程池执行器默认为10，内存任务存储器为`memoryjobstore`,如果想自己配置的话可以执行以下操作

需求：

```
两个任务储存器分别搭配两个执行器；同时，还要修改任务的默认参数；最后还要改时区
```

- 名称为“mongo”的`MongoDBJobStore` 
- 名称为“default”的`SQLAlchemyJobStore` 
- 名称为“ThreadPoolExecutor ”的`ThreadPoolExecutor`，最大线程20个
- 名称“processpool”的`ProcessPoolExecutor`，最大进程5个
- UTC时间作为调度器的时区
- 默认为新任务关闭*合并模式*（）
- 设置新任务的默认最大实例数为3

##### 方法一：

```python
from pytz import utc

from apscheduler.schedulers.background import BackgroundScheduler
from apscheduler.jobstores.mongodb import MongoDBJobStore
from apscheduler.jobstores.sqlalchemy import SQLAlchemyJobStore
from apscheduler.executors.pool import ThreadPoolExecutor, ProcessPoolExecutor


jobstores = {
    'mongo': MongoDBJobStore(),
    'default': SQLAlchemyJobStore(url='sqlite:///jobs.sqlite')
}
executors = {
    'default': ThreadPoolExecutor(20),
    'processpool': ProcessPoolExecutor(5)
}
job_defaults = {
    'coalesce': False,
    'max_instances': 3
}
scheduler = BackgroundScheduler(jobstores=jobstores, executors=executors, job_defaults=job_defaults, timezone=utc)
```

##### 方法二：

```python
from apscheduler.schedulers.background import BackgroundScheduler


# The "apscheduler." prefix is hard coded
scheduler = BackgroundScheduler({
    'apscheduler.jobstores.mongo': {
         'type': 'mongodb'
    },
    'apscheduler.jobstores.default': {
        'type': 'sqlalchemy',
        'url': 'sqlite:///jobs.sqlite'
    },
    'apscheduler.executors.default': {
        'class': 'apscheduler.executors.pool:ThreadPoolExecutor',
        'max_workers': '20'
    },
    'apscheduler.executors.processpool': {
        'type': 'processpool',
        'max_workers': '5'
    },
    'apscheduler.job_defaults.coalesce': 'false',
    'apscheduler.job_defaults.max_instances': '3',
    'apscheduler.timezone': 'UTC',
})

```

##### 方法三：

```python
from pytz import utc

from apscheduler.schedulers.background import BackgroundScheduler
from apscheduler.jobstores.sqlalchemy import SQLAlchemyJobStore
from apscheduler.executors.pool import ProcessPoolExecutor


jobstores = {
    'mongo': {'type': 'mongodb'},
    'default': SQLAlchemyJobStore(url='sqlite:///jobs.sqlite')
}
executors = {
    'default': {'type': 'threadpool', 'max_workers': 20},
    'processpool': ProcessPoolExecutor(max_workers=5)
}
job_defaults = {
    'coalesce': False,
    'max_instances': 3
}
scheduler = BackgroundScheduler()

# ..这里可以添加任务

scheduler.configure(jobstores=jobstores, executors=executors, job_defaults=job_defaults, timezone=utc)

```

### 五、启动调度器

除了`BlockingScheduler`外，其他非阻塞的调度器都会立即返回，运行之后的代码。

`BlockingScheduler`需要将运行的代码放在start()之前

#### 1.添加任务

```
1.调用add_job()  #可以传参max_instance,同一任务的运行实例个数
  当有任务中途中断，后面恢复后，有N个任务没有执行  
    coalesce：true ，恢复的任务会执行一次
    coalesce：false，恢复后的任务会执行N次配合misfire_grace_time使用
    misfire_grace_time设置时间差值，由于某些原因没有运行，再次提交时，大于设置的时间，实例不会运行。
2.装饰器scheduled_job()
立即运行可以不设置trigger参数

```

#### 2.移除任务

```python
# 根据任务实例删除
job = scheduler.add_job(myfunc, 'interval', minutes=2)
job.remove()

# 根据任务id删除
scheduler.add_job(myfunc, 'interval', minutes=2, id='my_job_id')
scheduler.remove_job('my_job_id')

```

#### 3.暂停任务

```python
job = scheduler.add_job(myfunc, 'interval', minutes=2)
# 根据任务实例
job.pause()  #暂停
job.resume() #继续

# 根据任务id暂停
scheduler.add_job(myfunc, 'interval', minutes=2, id='my_job_id')
scheduler.pause_job('my_job_id')
scheduler.resume_job('my_job_id')

```

#### 4.调度器操作

```python
scheduler.start() #开启
scheduler.shotdown(wait=True|False) #关闭  False 无论任务是否执行，强制关闭

```

#### 异常捕获

```python
# 可以添加apscheduler日志至DEBUG级别，这样就能捕获异常信息
import logging

logging.basicConfig()
logging.getLogger('apscheduler').setLevel(logging.DEBUG)

```
