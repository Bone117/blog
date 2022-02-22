# 树莓派4B基本配置

### 一、系统安装
> 官网下载好系统解压，使用`SD Card Formatter`格式化内存卡

```vim
# 查看内存卡状态,通过内存卡大小判断是哪个
df -lh
# 卸载内存卡
diskutil unmount /dev/disk2s1
# 确认设备号
diskutil list
# 烧写系统 ，进入解压镜像所在目录
sudo dd bs=4m if=rpi_35_v6_1_2_3_jessie_kernel_4_4_50.img of=/dev/disk2
# if = 镜像名称，of = sd卡
# 等待一段时间烧写完毕会有提示
# 卸载sd卡
diskutil unmountDisk /dev/disk2
```
插入到树莓派，试验是否成功
### 二、开启ssh连接
>`sudo raspi-config`
>选择第五个选项(interfacing options) 选择开启`ssh`这个选项
>修改默认的远程连接密码 第一个选项就是

### 三、开启root远程登录权限

1. 设置root用户密码
`sudo passwd root`
2. 启用root用户
`sudo passwd --unlock root`
3. 设置ssh允许登录
`sudo sed -i "s/^#PermitRootLogin.*/PermitRootLogin yes/g" /etc/ssh/sshd_config`
4. 重启ssh服务
`sudo systemctl restart ssh`
5. 为root用户应用于当前用户相同的bash配置
`sudo cp ~/.bashrc /root/.bashrc`

### 四、更新软件源

1. 更新sources.list
```vim
sudo cp /etc/apt/sources.list /etc/apt/sources.list.bk
sudo nano /etc/apt/sources.list
deb http://mirrors.aliyun.com/raspbian/raspbian/ buster main contrib non-free rpi
```

2. 更新raspi.list
```vim
sudo cp /etc/apt/sources.list.d/raspi.list /etc/apt/sources.list.d/raspi.list.bk
sudo nano /etc/apt/sources.list.d/raspi.list
deb http://mirrors.aliyun.com/raspbian/raspbian/ buster main
```
