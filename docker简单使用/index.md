# docker简单使用

## 一、安装docker

```shell
# SET UP THE REPOSITORY
# 配置 Docker 的官方软件源（并默认使用稳定版，其它版本请参考官方文档）
sudo yum -y install device-mapper-persistent-data lvm2 yum-utils
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

# INSTALL DOCKER ENGINE - COMMUNITY
# 安装 Docker CE - 社区版
sudo yum -y install docker-ce docker-ce-cli containerd.io

# 将当前用户加入 Docker 用户组
# 加入 Docker 用户组的用户，在执行 Docker 相关命令时，不再需要键入 sudo 以提权
sudo usermod -aG docker ${USER}

# Start Docker - 启动 Docker
sudo systemctl start docker

# 设置 Docker CE 开机自启（可选）
sudo systemctl enable docker

# 安装 Dokcer Compose 编排工具(如果没有安装的话)
sudo yum -y install epel-release
sudo yum -y install python-pip
sudo pip install docker-compose
```

MacOS可以通过brew安装`brew cask install docker`

## 二、配置镜像加速器

```shell
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": [
    "https://dockerhub.azk8s.cn",
    "https://reg-mirror.qiniu.com"
  ]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```

## 三、下载使用镜像

#### 1.搭建一个web服务器，拉取一个centos镜像

`docker run -p 80 --name web -i -t centos /bin/bash`

> `Docker` 通过 `run` 命令来启动一个新容器。`Docker` 首先在本机中寻找该镜像，如果没有安装，Docker 在 `Docker Hub` 上查找该镜像并下载安装到本机，最后 Docker 创建一个新的容器并启动该程序。
> 当第二次执行`docker run`时，因为Docker在本机中已经安装该镜像，所以 Docker 会直接创建一个新的容器并启动该程序。
> 注:**`docker run` 每次使用都会创建一个新的容器**，因此，我们以后再次启动这个容器时，只需要使用命令 `docker start`  即可。这里， `docker start` 的作用在用重新启动已存在的镜像，而`docker run` 包含将镜像放入容器中 `docker create` ，然后将容器启动 `docker start`
> `Docker` 容器重启后会沿用 `docker run` 命令指定的参数来运行，所以还是在后台运行的。可以通过`docker attach`命令切换到运行交互式容器

#### 2.安装nginx服务器

执行`rpm -ivh http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm`
再执行`yum install -y nginx`
启动nginx`nginx`
如果出现无法安装的情况试试`systemctl restart docker`
可以执行`ctrl+P+Q`切换到后台,通过`docker ps -a`查看随机分配的端口，通过浏览器访问即可。

## 四、构建镜像

#### 1.创建`Dokcerfile`文件

> Dockerfile是Docker用来构建镜像的文本文件，包含自定义的指令和格式, 可以通过docker build命令从Dockerfile中构建镜像。

```shell
mkdir dockerfile_test
cd dockerfile_test/
touch Dockerfile
nano Dockerfile
```

编写`Dockerfile`文件

```shell
FROM centos:7
MAINTAINER test "test@gmail.com"
RUN rpm -ivh http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm
RUN yum install -y nginx
EXPOSE 80
```

#### 2.构建镜像

```shell
# docker build -t registry.cn-hangzhou.aliyuncs.com/<命名空间>/<应用镜像名>:<镜像版本> .
docker build -t="test/docker_demo:v1" .
# 这个时候查看本地已经有镜像了 docker images
```

#### 3.将镜像推送到远程仓库

```shell
# 登录到阿里云控制台
https://cr.console.aliyun.com/cn-hangzhou/instances/repositories
# 创建命名空间
https://cr.console.aliyun.com/cn-hangzhou/instances/namespaces
# 获取访问凭证
https://cr.console.aliyun.com/cn-hangzhou/instances/credentials
# 登录阿里云的镜像仓库
docker login --username=<阿里云登录账号> registry.cn-hangzhou.aliyuncs.com
# 推送镜像
docker push registry.cn-hangzhou.aliyuncs.com/<命名空间>/<应用镜像名>:<镜像版本>
```

## 五、停止、删除docker容器和镜像

> 学习的时候经常下载很多镜像搞的环境有点多，可以试试下面的命令整理环境

```shell
# 列出所有的容器ID
docker ps -aq
# 停止所有的容器
docker stop $(docker ps -aq)
# 删除所有的容器
docker rm $(docker ps -aq)
# 删除所有的镜像
docker rmi $(docker images -q)
```

参考：
https://juejin.im/post/5cacbfd7e51d456e8833390c#heading-12
https://yeasy.gitbooks.io/docker_practice/content/
