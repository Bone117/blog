# Mac设置python环境

## 1.包安装
```python
pip3 install virtualenv
pip3 install virtualenvwrapper
```
## 2.创建目录存放虚拟环境
```python
mkdir ~/.virtualenvs
```
## 3.修改zsh
> 通过`which virtualenv`和`which virtualenvwrapper`判断位置，在zsh末尾添加
```
# Setting PATH for Python 3 installed by brew
export PATH=/usr/local/share/python:$PATH

# Configuration for virtualenv
export WORKON_HOME=$HOME/.virtualenvs
export VIRTUALENVWRAPPER_PYTHON=/usr/local/bin/python3
export VIRTUALENVWRAPPER_VIRTUALENV=/usr/local/bin/virtualenv
source /usr/local/bin/virtualenvwrapper.sh
```
## 4.基本命令
```python
mkvirtualenv -p python3.9 web_py3.9 //创建环境
deactivate //退出环境
workon  //列出环境
workon web_py3.9  //激活
```
