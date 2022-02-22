# Mac多SSH Key配置

## 多SSH key配置

> 工作的时候碰到SSH配置的问题，就是公司用的是gittea的仓库，而本人的github平常也要使用，这个时候就需要配置不同的SSH key了。将同一个公钥分配配置给github和gittea的话并不可行。个人认为是你在操作的时候他不知道你是操作哪个git。

### 1.切换到系统的SSH目录下。

```
cd ~/.ssh
```

### 2.生成自己的github的SSH key(默认这里你已经配置好了一个SSH key)

```
ssh-keygen -t rsa -C "自己Github账号" -f github_rsa   #-f表示保存的文件名
```

> 一路回车

![image-20190705090153099](http://ww3.sinaimg.cn/large/006tNc79ly1g4oop417vjj30ju03a3yv.jpg)

### 3.将对应的SSH key添加到相应的平台

![image-20190705092507081](http://ww2.sinaimg.cn/large/006tNc79ly1g4opd7dq2bj31oo0puq68.jpg)

### 4.配置config文件

```
# github
Host github.com
HostName github.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/github_rsa

# gitlab
Host gitlab.com
HostName gitlab.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/company_rsa
```

> 注：host名称没有关系，HostName是网站的地址，配置相应的地址就好了。不知道是哪个，可以看你clone的地址，`@`后面到项目名之间的就是了。

### 5.测试是否添加成功

```
ssh -T git@github.com
```

### SSH key参数选项

```
-b：指定密钥长度； 
-e：读取openssh的私钥或者公钥文件； 
-C：添加注释； 
-f：指定用来保存密钥的文件名； 
-i：读取未加密的ssh-v2兼容的私钥/公钥文件，然后在标准输出设备上显示openssh兼容的私钥/公钥； 
-l：显示公钥文件的指纹数据； 
-N：提供一个新密语； 
-P：提供（旧）密语；
-q：静默模式； 
-t：指定要创建的密钥类型。
```
