# docker基础

### 简介：
> Docker镜像含有启动容器所需要的文件系统及其内容。

* 采用分层构建机制，最底层为`bootfs`,其之为`rootfs`
    * bootfs: 用于系统引导的文件系统，包括`bootloader`和`kernel`，容器启动完成后会被卸载以节约内存资源；
    * rootfs: 位于bootfs之上，表现为docker容器的根文件系统；
    * 1.传统模式中，系统启动时，内核挂载rootfs会首先将其挂载为“只读”模式，完整性自检完成后将其重新挂载为读写模式；
    * 2.docker中rootfs由内核挂载为“只读”模式，然后通过“联合挂载”技术额外挂载一个“可写”层
* 位于下层的镜像称为父镜像，最底层的为基础镜像；最上层为“可读写层”，其下的均为“只读”层

为什么docker镜像要采用分层结构?
>好处就是**共享资源**
>多个镜像都是从相同的base镜像构建而来,宿主机只需要在磁盘上保存一份base镜像,同时内存中也只需要加载一份base镜像,就可以为所有容器服务了.而且镜像的每一层都可以被共享

Registry：
>启动容器时，docker daemon会先从本地获取相关的镜像，本地镜像不存在会将其从`Registry`中下载该镜像，并保存到本地。

### 一、基于容器制作镜像(docker commit)
```shell
// 首先拉取一个镜像并启动容器
docker run --name b1 -it busybox
// 在容器上做一些修改
mkdir -p /data/html
// 随便输入一些html网页信息
vi /data/html/index.html
// 另启动一个终端，加-p是为了让启动的容器先暂停
docker commit 镜像id zfj/busybox:1.1
// docker commit 命令
// docker commit -a="作者" -m="描述信息" -p对外端口:docker对外端口 容器id 要创建的目标镜像名:[标签名]
// 这个时候就可以在本地看到多了一个docker镜像了
```
### 二、容器命令补充:
1.查看容器日志
> `docker logs 容器id`
> -t:加入时间
> -f追加打印

2.docker run命令
> `-p 对外端口:docker内部端口`
> `-P` 随机端口
> `-d` 后台运行

3.查看容器内部细节
> `docker inspect 容器id`

4.进入容器
> `docker exec -it 容器id bash/shell`
> `docker attach 容器id`直接进入容器启动终端,不会启动新进程
> 通过exec进入的容器 使用exit不会退出.因为exec会启动一个新的bash

5.复制docker内部的数据到宿主机
> `docker cp 容器id:源路径 目标路径`

### 三、docker容器数据卷
> Docker容器产生的数据，如果不通过`docker commit`生成新的镜像，使得数据做为镜像的一部分保存下来，那么**当容器删除后，数据自然也就没有了**。为了能保存数据在docker中我们使用卷。
> 卷不属于联合文件系统,因此可以持续存储或共享数据,设计的目的就是为了数据的持久化,完全独立于容器的生命周期,因此docker不会在容器删除时删除其挂载的数据卷

特点:
* 数据卷可在容器之间共享或重用数据
* 卷中的更改可以直接生效
* 卷中的更改不会包含在镜像的更新中
* 数据卷的生命周期一直持续到没有容器使用它为止.

##### 1.容器内添加数据卷:
命令添加:
> `docker run -it -v /宿主机的绝对路径目录:/容器内目录 镜像名`(-v 含有新建的功能)
> 在容器或者宿主机上新建文件,对方都能看到,说明实现了数据共享
> `docker run -it -v /宿主机的绝对路径目录:/容器内目录:ro 镜像名`(ro=read only)容器只允许查看,不允许修改

dockerfile添加(通过VOLUME来添加数据卷):
> `mkdir /mydocker`

````shell
# vim Dockerfile
FROM centos
VOLUME ["/dataVolumeContainer1","/dataVolumeContainer2"]
CMD echo "finished,------success"
CMD /bin/bash
```
> build dockerfile文件
> `docker build -f /mydocker/Dockerfile -t zfj/centos . ` (-f 路径 -t标签)

##### 2.数据卷容器
> 命名的容器挂载数据卷，其它容器通过挂载这个(父容器)实现数据共享，挂载数据卷的容器，称之为数据卷容器
> `docker run -it --name test01 zfj/centos`
> `docker run -it --name test02 --volumes-from test02 zfj/centos`
> 使用`--volumes-from`从父容器继承(继承后,父子容器资源共享)
> 再添加容器`test03 继承 test01` 删除`test01`发现 容器还是共享的
> 结论:容器之间配置信息的传递,数据卷的生命周期一直持续到没有容器使用它为止.

### 四、Dockerfile
##### 1.简介:
> Dockerfile是用来构建docker镜像的构建文件,是由一系列命令和参数构成的脚本
构建三步骤:
* 编写Dockerfile文件
* `docker build`
* `docker run`

##### 2.Dockerfile语法:
* 每条保留字指令必须**大写字母**,而且后面**至少跟随一个**参数
* 指令从上到下,顺序执行
* #表示注释
* 每条指令都会创建一个新的镜像层,并对镜像进行提交

##### 3.Dockerfile的执行流程:

1. docker从基础镜像运行一个容器
2. 执行一条指令对容器做出修改
3. 执行类似docker commit的操作提交一个新的镜像层
4. docker再基于上一次提交的镜像运行一个新容器
5. 执行Dockerfile中的下一条指令直到所有指令都执行完成.

##### 4.docker保留字指令

* `FROM`:基础镜像,当前镜像是继承哪个镜像的
* `MAINTAINER`:镜像维护者的姓名和邮箱
* `RUN`:容器构建时需要运行的命令
* `EXPOSE`:暴露对外的端口
* `WORKDIR`:指定创建容器后,终端默认登录的工作目录,落脚点
* `ENV`:构建镜像过程中设置环境变量
* `ADD`:将宿主机目录下的文件拷贝到镜像,ADD会自动处理URL和解压tar压缩包
* `COPY`:类似ADD命令 但只有拷贝功能. `COPY src dest` 或者`COPY ["src","dest"]`
* `VOLUME`:容器数据卷,用于数据保存和持久化工作
* `CMD` 指定一个容器启动时要执行的命令,但是只有**最后一个生效**,CMD会被`docker run` 之后的参数替换
* `ENTRYPOINT`:与CMD类似,但是`docker run` 之后的参数是追加
* `ONBUILD`:当构建一个被继承的Dockerfile时运行命令,父镜像在被子继承后,父的onbuild被触发

Base镜像(scratch):Docker Hub 中99% 的镜像都是通过在base镜像中安装配置需要的软件构建出来的
案例:
自定义centos镜像(修改WORKDIR，安装vim，和net-tool)

1. 新建Dockerfile文件`touch Dockerfile1`
2. 编写Dockerfile文件

````shell
FROM centos

ENV mypath /tmp
WORKDIR $mypath

RUN yum -y install vim
RUN yum -y install net-tools

EXPOSE 80
CMD /bin/bash
```

3.构建 `docker build -f /mydocker/Dockerfile1 -t mycentos:1.3 .`(-t 新镜像名 -f dockerfile路径)
4.运行新镜像产生的容器 `rocker run -it mycentos:1.3`
5.列出镜像历史:`docker history 镜像名`
