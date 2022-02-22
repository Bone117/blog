# 黑苹果_win 双系统 单硬盘安装


### 1.准备工作
1. 需要两个U盘，一个windows的装机盘(我使用了微pe)，一个用来安装macos系统
2. 去[黑果小兵](https://blog.daliansky.net/archives/)下载所需的镜像文件
3. 下载苹果镜像工具[etcher](https://www.balena.io/etcher/)
4. 下载对应的efi文件，可以在github上搜你的主板和cpu型号
5. 将下载好的镜像烧录到用于安装macos系统的u盘中

### 2.黑苹果BIOS设置(微星B460m)
1. settings\高级\唤醒时间管理\BIOS
2. settings\高级\内建显示器\集成显卡多显示器\允许
3. settings\高级\PCIe/PCi子系统设置/Above 4G memory.../允许
4. settings\高级\USB设置\XHCI Hand-off\ 允许
5. settings\高级\USB设置\传统USB支持\ 允许
6. settings\高级\BIOS CSM/UEFI Mode\ UEFI
7. OC\cpu特征\intel虚拟化技术\允许
8. OC\cpu特征\intel VT-D技术\禁止
9. OC\cpu特征\CFG锁定\禁止
![网络设置](https://raw.githubusercontent.com/cheneyxx/Hackintosh-10400-B460M-MORTAR/main/images/pic.png)

### 3.分区
1. F11(微星B460m)进入bios，选择u盘启动
2. 通过微pe里的DiskGenius进行分区，如果事先已经安装win系统，也可以自行在win下通过该工具进行分区
3. 选中要安装系统的硬盘，将硬盘分区表类型转为GUID模式(如果原来有系统了，只需新建一个分区，格式为Mac OS X（HFS+)patition，并且保证有esp分区，并且分区表类型为GUID模式）)
4. 选中硬盘，建立新分区，这时会跳出来建立ESP，MS分区，我们只要勾选ESP就好了，大小默认300M
5. 在空闲的位置点击建立新分区，文件系统类型要选择Mac OS X（HFS+)patition，空间大小随你决定
6. 在空余的位置点击建立新分区，文件系统选择NTFS
7. 如图所示(网图)![网图](https://img-blog.csdnimg.cn/20200112141642185.png)

### 4.安装系统

#### windows系统安装
1. F11(微星B460m)进入bios，选择u盘启动
2. 进入pe安装windows系统在之前新建的NTFS分区中
3. 等

#### mac系统安装
1. 删除macos系统盘中的EFI文件夹，替换为下载好的efi文件
2. F11选择macos系统盘启动
3. 选择`Boot macOS install from macOS Big Sur`
4. 选择**磁盘工具**，将之前的macos分区抹掉，名称为`xx(随便你)`小写然后选择`APFS`格式
5. 开始安装系统(ps:重启期间一定要按F11，继续选择`Boot macOS install from xxx`,不然可能会安装失败)
6. 重启几次，安装完成之后会看到`Boot macOS from xx`,选择这个进入配置
7. 安装成功

### 5.添加引导
1. 使用DiskGenius对win的数据盘右键，比如`本地磁盘D`，调整分区大小，分区前部空间200M-300M。开始
2. 选择`本地磁盘D`前的空余分区，右键建立新分区，选择FAT32格式，保存
3. 删除ESP分区下EFI的所有文件
4. 将opencore/EFI下的文件拖动到刚刚的ESP分区下的EFI文件夹
5. 使用windows引导修复工具，选择`引导磁盘`为我们刚刚添加的分区`E盘`，系统盘为`C盘`，选中`修复UEFI引导`，开始修复。
6. 重启电脑



### 6.参考文献
* https://github.com/maemual/MSI-B460M-10700-5500XT
* 分区引导：https://www.bilibili.com/video/BV1nz4y1Q7p6/
* Windows环境下UEFI引导修复工具：https://pan.baidu.com/s/10tG7Y-naPAUOkOZ9cs_blQ  提取件码：pcyu
* 磁盘分区工具DiskGenius: https://pan.baidu.com/s/1GTwzo4v3O6T9P9Ku36IgRg      提取码: av96
