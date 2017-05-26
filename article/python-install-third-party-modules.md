# 如何安装Python第三方模块

Python官方为我们提供了第三方库，那么如何安装这些库呢？

安装第三方库有两种方式：

1. 第一种就是使用python自带的仓库pip进安装
2. 第二种就是使用源码进行安装

## PIP方式安装

首先用yum安装`python-pip`软件包

```bash
[root@ansheng ~]# yum  install python-pip
```

安装完成之后可以使用`pip -V`查看安装版本

```bash
[root@ansheng ~]# pip -V
pip 7.1.0 from /usr/lib/python2.6/site-packages (python 2.6)
```

这次就以`requests`模块为例把，先查看当前系统有没有安装`requests`模块

```bash
[root@ansheng ~]# python
Python 2.6.6 (r266:84292, Jul 23 2015, 15:22:56) 
[GCC 4.4.7 20120313 (Red Hat 4.4.7-11)] on linux2
Type "help", "copyright", "credits" or "license" for more information.
# 如果没有安装在导入的时候就会报错
>>> import requests
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ImportError: No module named requests
>>> exit()
```

用`pip`的方式安装`requests`模块

```bash
[root@ansheng ~]# pip install requests
```

安装完成之后进入`python`解释器导入`requests`模块，看看能不能导入成功

```bash
[root@ansheng ~]# python
Python 2.6.6 (r266:84292, Jul 23 2015, 15:22:56) 
[GCC 4.4.7 20120313 (Red Hat 4.4.7-11)] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> import requests
```

安装成功。

卸载执行`pip uninstall`加模块名

```bash
[root@ansheng ~]# pip uninstall requests
```

## 源码包方式安装

下载模块`requests`的源码包

```bash
[root@ansheng ~]# git clone git://github.com/kennethreitz/requests.git
Initialized empty Git repository in /root/requests/.git/
remote: Counting objects: 17546, done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 17546 (delta 0), reused 0 (delta 0), pack-reused 17544
Receiving objects: 100% (17546/17546), 5.04 MiB | 46 KiB/s, done.
Resolving deltas: 100% (11232/11232), done.
```

查看下载下来的文件

```bash
[root@ansheng ~]# cd requests/
[root@ansheng requests]# ls
AUTHORS.rst      docs  HISTORY.rst  Makefile     NOTICE      requests                    requirements.txt  tests
CONTRIBUTING.md  ext   LICENSE      MANIFEST.in  README.rst  requirements-to-freeze.txt  setup.py
```

执行`python setup.py install`进行编译安装

```bash
[root@ansheng requests]# python setup.py install
```

验证是否安装成功

```bash
[root@ansheng requests]# python
Python 2.6.6 (r266:84292, Jul 23 2015, 15:22:56) 
[GCC 4.4.7 20120313 (Red Hat 4.4.7-11)] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> import requests
```

安装成功，以上就是`Python`第三方模块的两种安装方式，愿能够为你提供帮助。