安装python3
===============
从源码安装python3，因为默认的官方 yum 源中不提供 Python 3 的安装包。

.. code-block:: none
	:linenos:

	#先安装几个必须的包，以方便后续的操作
	$yum install wget gcc make
	#用PC从http://172.21.95.200/xrepos/python/上下载了python源码，
	#然后，把python源码传到虚拟机上。
	#因为pscp是putty自带的工具，所以进入putty在pc上的安装路径
	#c:\Program Files\PuTTY，执行下面这条命令
	$pscp  C:\Users\xinhua\Downloads\Python-3.6.9.tgz  apiServ@1
	72.22.25.56:/home/apiServ/downloads
	$tar -xvf Python-3.6.9.tgz
	$cd Python-3.6.9
	#--prefix 是预期安装目录
	$ ./configure --prefix=/usr/local/python3.6
	$make
	#安装依赖，否则make install报错
	$sudo yum install zlib-devel
	$sudo make install
	$ln -s  /usr/local/python3.6/bin/python3  /usr/bin/python3