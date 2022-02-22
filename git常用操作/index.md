# GIT常用操作

##  GIT常用操作

### git本地仓库与远程建立连接

#### 1.在本地初始化仓库

```
git init

git add .
git commit -m 'add files'
```

#### 2.在github上新建一个仓库

#### 3.本地与远程仓库建立连接

```
git remote add origin 你远程仓库的地址
```

#### 4.合并本地代码

```
git pull origin master --allow-unrelated-histories  //合并不相关的历史内容
```

#### 5.将本地代码推导远程仓库

```
git push origin master  
```

#### 6.新建本地分支，并推送到远程

```
git checkout -b dev   //b 表示创建并切换
git push origin dev:dev
```

#### 7.查看所有分支

```
 git branch -al 
```

#### 8.合并某分支到当前分支

```
git merge dev
```

#### 9.删除分支

```
git push origin --delete dev //删除远程分支  或者
git push origin :dev  //推送本地空分支到远程dev分支
```

### git基本命令

#### 1.查看状态

```
git status #显示工作目录和暂存区的状态，不显示已经commit的信息
git log #显示提交日志

```

#### 2.add添加文件

```
git add .  #将所有修改添加到暂存区，不包括被删除的文件
git add abc.py  #添加某个文件
git add abc.py cde.py #添加多个文件时，中间用空格分开
git add ab* #将以ab开头的文件添加到暂存区
git add *.py #将以py结尾的文件添加到暂存区
git add -u <path> #只添加<path>中已跟踪的文件信息，省略<path>即当前目录
git add -A #提交所有变化，包括被删除的文件

```

#### 3.撤销add操作

```
git reset HEAD #撤销上一次的add操作 HEAD～2 代表倒数第二个
git reset HEAD abc.py #对某个文件进行撤销

```

#### 4.commit提交到本地版本库

```
git commit -m "注释信息"
git commit --amend #增补提交，使用当前节点相同节点进行一次新提交，就的提交被取消

```

#### 5.撤销commit操作

```
git reset --soft HEAD^ #不删除工作空间改动代码，撤销commit，不撤销git add
git reset --hard HEAD^ #删除工作空间改动代码，撤销commit和add

```

#### 6.撤销本地的修改

```
#本地做了一些操作，想撤销到上一次commit的版本
git checkout

```

#### 7.暂存工作区文件

> 本地进行了修改添加，这个时候需要切分支，或者更新本地代码，可以先将本地代码暂存，暂存后工作目录就是干净的，可以切分支或者更新代码，更新或者操作完别的分支后，再从暂存区取出之前的代码。

```
git stash   # 添加到暂存区
git stash save "注释"  #多了注释内容
git stash list # 查看存储的列表
git stash show #显示做了哪些改动，默认第一个,还可以git stash show stash@{2}
git stash apply stash@{1} #应用某个存储，但不会从存储列表删除
git stash drop stash@{1} #从栈中移除暂存的内容
git stash clear #删除所有缓存

```

> 注：没有在git版本控制中的文件是不能被stash存起来的

#### 8.删除文件

```
git rm a.py #将a.py从git仓库管理系统中删除
# 下次提交 远程仓库的a.py 也会被删除
git rm -r mydir #将mydir文件夹从仓库管理系统中删除

```

> 普通的本地删除文件，远程仓库还是会有的。

#### 9.branch的基本操作

```
git branch #查看本地分支
git branch -al#查看所有分支(包括远程)
git bracnh dev #创建dev分支
git checkout dev #切换到dev分支
git branch -m dev test #修改分支名字
git push origin --delete dev #删除分支
git merge dev #合并某个分支到当前分支

```
