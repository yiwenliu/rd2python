pip
======
pip10升级到19
---------------
从https://www.lfd.uci.edu/~gohlke/pythonlibs下载whl包，然后在pycharm的Terminal中执行
d:\pycharmprojects\django\venv\scripts\python.exe -m pip install D:\PycharmProjects\pip-19.3.1-py2.py3-none-any.whl

更新pip
----------
1. 删除D:\PycharmProjects\nltk\venv\Lib\site-packages\下的pip-10.xxx文件夹

2. pycharm-File-Settings-Project nltk-project interpreter-点击后面的install packaging tools进行安装
$python -m pip install -U --force-reinstall pip

pip升级urllib3
-------------------
onvideo使用python2，版本太旧，无法使用pip安装包了，需要升级urllib3.

从https://www.lfd.uci.edu/~gohlke/pythonlibs/#numpy上下载了whl包，在pycharm中本地安装。

whl安装
---------
whl文件下载地址
^^^^^^^^^^^^^^^^^^^
1. https://pypi.org/project/{package name}/#files 

- https://pypi.org/project/cryptography/#files

2. https://www.lfd.uci.edu/~gohlke/pythonlibs

安装命令
^^^^^^^^^^^
.. code-block:: none

    pip install D:\PycharmProjects\urllib3-1.25.7-py2.py3-none-any.whl

超时
-------
如果出现ReadTimeoutError，可以使用

pip --default-timeout=100 install xxxx

InsecurePlatformWarning&SNIMissingWarning:
-------------------------------------------------
学习django时，python版本太旧，使用pip就报这个错，尝试了本地安装相关whl的方法，但是，还是失败，于是放弃了。
这些whl位于D:\PycharmProjects\wheels_dir

配置pypi源
------------
.. code-block:: none
	:linenos:

	# 安装pip
	$yum install python-pip python-wheel
	# 配置pip.conf
	$ mkdir ~/.pip
	$ vim ~/.pip/pip.conf
	# 在pip.conf文件中写入：
	[global]
	index-url = http://10.240.128.7/pypi/simple/
	[install]
	trusted-host = 10.240.128.7