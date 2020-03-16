Pycharm
============
更新pip
----------
.. code-block:: none

	python -m pip install --upgrade pip

如果这条命令执行后，依然提示要更新pip，就参考https://www.jianshu.com/p/6453e3353694即可。

pip安装超时
--------------
参考https://blog.csdn.net/qq_39161804/article/details/81191977

pip安装时显示找不到包
-----------------------
方法1. 升级pip再安装

方法2：下载后，离线安装https://www.lfd.uci.edu/~gohlke/pythonlibs

创建requirements
--------------------
.. code-block:: none

	pip freeze >  requirements.txt

import自定义module
---------------------
https://stackoverflow.com/questions/21236824/unresolved-reference-issue-in-pycharm

File-Settings-Build,Execution,Deployment-Console-Python Console-勾选链接中两个勾勾