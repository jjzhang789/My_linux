## 安装pyenv

安装git

>yum -y install git


安装pyenv
>git clone https://github.com/yyuu/pyenv.git ~/.pyenv

配置环境变量
>echo 'export PYENV_ROOT=$HOME/.pyenv' >> ~/.bash\_profile
>
>echo 'export PATH=$PYENV_ROOT/bin:$PATH' >> ~/.bash\_profile

>echo 'eval "$(pyenv init -)"' >> ~/.bash_profile

使oyenv命令生效
>source ~/.bash_profile

## 安装Python

安装依赖与编译工具
>yum -y install gcc make patch install gdbm-devel openssl-devel sqlite-devel readline-devel zlib-devel bzip2-devel


安装Python 3.5.2

>pyenv install 3.5.2


##更改pip下载源

由于网络环境，现更改下载源为阿里源

创建目录 `mkdir ~/.pip`

编辑 `~/.pip/pip.conf`, 输入以下内容
<pre>
  [global]
  index-url = http://mirrors.aliyun.com/pypi/simple/
  trusted-host = mirrors.aliyun.com
</pre>

## 使用pyenv命令

### local命令
local命令切换当前目录及其子目录的Python版本， 可以通过删除 `.python-version`恢复默认Python版本

### global命令
global名切换全局默认Python版本

**永远不要使用global命令**

### virtualenv命令
创建虚拟环境

>pyenv virtualenv $bash_version $name

### uninstall命令
卸载某个版本， 包括虚拟环境
