# 树莓派4B NAS系统搭建

### 一、硬盘挂载
由于之前硬盘(NTFS格式)里有数据不想格式化想直接挂载，就没有格式化成`ext4`文件格式的。

1. 安装ntfs-3g
 `sudo apt-get install ntfs-3g`
2. 加载内核模块
 `modprobe fuse`
3. 查看硬盘情况
`fdis -l`
4. 将硬盘挂载到/mnt下
`ntfs-3g /dev/sda1 /mnt`
5. 实现开机自动挂载
```vim
vim /etc/fsta
# 最后一行添加，重启生效
/dev/sda1 /mnt ntfs-3g defaults,noexec,umask=0000 0 0
```
6. 查看挂载情况
```vim
cd /mnt
ls
```

### 安装samba
```vim
apt install samba samba-common-bin
# 过程中需要安装额外的包 确定即可
# 在/mnt下 创建一个文件 设置权限
mkdir data
chown -R root:users /mnt/data
chmod -R ug=rwx,o=rx /mnt/data
# 修改samba配置
vim /etc/samba/smb.conf

#修改Authentication
    security = user

# 修改home下的read
    read only = no

# 最后
[public]
    # 说明信息
    comment = public storage
    # 共享文件的路径
    path = /mnt/data
    # 可以访问的用户
    valid users = @users
    force group = users
    # 新建文件权限
    create mask = 0660
    # 新建目录权限
    directory mask = 0771
    read only = no
# 保存退出
```
重启smb服务
`/etc/init.d/samba-ad-dc restart`

有问题可以试试这个：
```vim
# 重启服务: sudo /etc/init.d/smbd restart
# 重启服务: sudo /etc/init.d/nmbd restart
```
添加用户
```vim
smbpasswd -a pi
#输入密码即可
```
在添加用户的时候一开始不是`pi`是别的用户名，碰到了`Failed to add entry for user`
原因是因为没有加相应的系统账号，只要添加账号即可。
现在电脑就可以连接树莓派的ip地址了。
### 安装qBittorrent下载器
```vim
# 安装
sudo apt-get update && sudo apt-get install qbittorrent-nox
```
创建系统服务，新建/etc/systemd/system/qbittorrent.service文件，写入以下内容
```vim
[Unit]
Description=qBittorrent Daemon Service
After=network.target

[Service]
User=pi
ExecStart=/usr/bin/qbittorrent-nox
ExecStop=/usr/bin/killall -w qbittorrent-nox

[Install]
WantedBy=multi-user.target
```
更新systemctl
`sudo systemctl daemon-reload`
直接启动服务：qbittorrent-nox，默认端口是8080，如果想指定端口运行的话，则加上参数--webui-port=x，其中x就是端口号，比如指定端口为8088：qbittorrent-nox --webui-port=8088 后台运行后面加个`&`
最后
```vim
#开启qbt服务
systemctl start qbittorrent
#查看服务状态
systemctl status qbittorrent
#服务开机自启
systemctl enable qbittorrent
```
如果下载没有速度可以修改用户为root
