在centos上配置环境
=====================
python
----------
安装ssl
^^^^^^^^^^
在newsbrief服务器上运行程序时，会报错No module named '_ssl'

用yum安装的步骤
+++++++++++++++++

.. code-block:: none
	:linenos:

	//查看openssl-devel是否安装
	# rpm -aq|grep openssl
	//如果没有openssl-devel，安装之
	# yum install openssl-devel -y
	//再次查看openssl-devel是否安装
	# rpm -aq|grep openssl
	openssl-1.0.2k-12.el7.x86_64
	openssl-libs-1.0.2k-12.el7.x86_64
	openssl-devel-1.0.2k-12.el7.x86_64

但是，这样的方法，还是没有/usr/local/ssl这个目录，所以只能从源码安装

从源码安装ssl
++++++++++++++++
.. code-block:: none
	:linenos:

	1.从https://www.openssl.org/source/上下载，再传到虚拟机上。
	pscp C:\Users\yiwen\Downloads\openssl-1.1.1a.tar.gz root@172.22.27.85:/root/downloads/
	2.
	tar -xzvf openssl-1.1.1a.tar.gz
	cd openssl-1.1.1a/
	./config --prefix=/usr/local/ssl --openssldir=/usr/local/openssl
	make && make install


安装python
^^^^^^^^^^^^^^^^
1. 用PC从http://172.21.95.200/xrepos/python/上下载了.tar.gz包

2. 把gz包传到虚拟机

进入putty在pc上的安装路径

.. code-block:: none
	:linenos:


	c:\Program Files\PuTTY>pscp C:\Users\yiwen\Downloads\Python-3.6.2.tgz root@172.22.27.85:/root/downloads/

	c:\Program Files\PuTTY>pscp C:\Users\yiwen\Downloads\python.3.6.2.centos7.bin.tar.gz root@172.22.27.85:/root/downloads/

3. 安装参考链接： https://segmentfault.com/a/1190000009922582

注意，其中，建立了python3的符号链接

1. 
# yum install wget gcc make

2. 
#tar -xvf Python-3.6.1.tar

3.修改Python3.6.1/setup.py

search_for_ssl_incs_in = [
                          '/usr/local/ssl/include',
                          '/usr/local/ssl/include/openssl', # 添加此行，否则可能会报错找不到 rsa.h 文件
                          '/usr/contrib/ssl/include/'

4.修改Python3.6.1/Module/Setup.dist文件,注释掉下面5行：

.. code-block:: none
	:linenos:

	_socket socketmodle.c

	# Socket module helper for SSL support; you must comment out the other
	# socket line above, and possibly edit the SSL variable:
	SSL=/usr/local/ssl
	_ssl _ssl.c \
	    -DUSE_SSL -I$(SSL)/include -I$(SSL)/include/openssl \
	    -L$(SSL)/lib -lssl -lcrypto

5. 编译

.. code-block:: none
	:linenos:

	--prefix 是预期安装目录

	cd Python-3.6.1

	//否则，报炸不到libssl.so
	#echo 'export LD_LIBRARY_PATH=${LD_LIBRARY_PATH}:/usr/local/ssl/lib ' >> ~/.bashrc 
	#source ~/.bashrc
	./configure --prefix=/usr/local/python3.6
	make
	make install

6.
ln -s /usr/local/python3.6/bin/python3 /usr/bin/python3

7. 安装路径/usr/local/python3.6

环境变量
^^^^^^^^^^^
.. code-block:: none
	:linenos:

	#可以不用执行这些
	#vi /etc/profile.d/python.sh
	输入 PATH=$PATH:/usr/local/python3.6/bin
	#source /etc/profile.d/python.sh

使用python命令
^^^^^^^^^^^^^^^^^^
1. 使用python3

pip
-------------
安装
^^^^^^^
#yum install python-pip python-wheel

更新pypi源
^^^^^^^^^^^
1. 麻烦平台部打开虚拟机访问http://172.20.85.12/pypi/srv/pypi/web/simple/

2. 可以在编辑CentOS shell账户Home目录下pip配置文件，vi ~/.pip/pip.conf文件，内容如下：

.. code-block:: none
	:linenos:

	[global] 
	index-url = http://172.20.85.12/pypi/srv/pypi/web/simple/
	[install]
	trusted-host=172.20.85.12

这样就是将默认的pypi源改成融发内部Pypi镜像源了，而不用每次pip install的时候通过-i参数指定。

git
------
安装git
^^^^^^^^^^^
# yum info git

配置gitlab
^^^^^^^^^^^^^^^
.. code-block:: none
	:linenos:

	#ssh-keygen -t rsa -C "$your_email"
	#cat ~/.ssh/id_rsa.pub
	#在gitlab中添加这个公匙
	#git init
	#git remote add origin-gitlab http://202.123.106.102:25223/yiwen/newsbrief.git
	#git pull origin-gitlab master //相当于是从远程获取fetch最新版本并merge到本地

nginx
--------
安装
^^^^^^^^^
参考链接：https://segmentfault.com/a/1190000007116797

1. 安装

#yum -y install nginx

2. 卸载

#rpm -e nginx 

#rpm -e --nodeps nginx //这个命令相当于强制卸载，不考虑依赖问题。

3. 查看安装路径

yum 在线安装会将 nginx 的安装文件放在系统的不同位置，可以通过命令 rpm -ql nginx 来查看安装路径，

4， 启动

.. code-block:: none
	:linenos:

	service nginx start #启动 nginx 服务
	service nginx stop #停止 nginx 服务
	service nginx restart #重启 nginx 服务

5.查看nginx安装目录

在shell中输入命令

# ps -ef | grep nginx

返回结果

root      4593     1  0 Jan23 ?   00:00:00 nginx: master process /usr/sbin/nginx

6.查看nginx.conf配置文件目录

在shell中输入命令

# nginx -t

返回结果

nginx: the configuration file /etc/nginx/nginx.conf syntax is ok

nginx: configuration file /etc/nginx/nginx.conf test is successful

7. 在centos上打开80端口访问

Centos7默认安装了firewalld，如果没有安装的话，可以使用 yum install firewalld firewalld-config进行安装。

.. code-block:: none
	:linenos:

	#systemctl status firewalld或者 firewall-cmd --state //查看状态
	#firewall-cmd --version //查看版本
	#firewall-cmd --get-active-zones //查看区域
	#firewall-cmd --zone=public --list-ports //查看指定区域所有打开的端口
	#firewall-cmd --zone=public --add-port=80/tcp(永久生效再加上 --permanent) //在指定区域打开端口（记得重启防火墙）
	#firewall-cmd --reload //重启防火墙

配置nginx为静态文件服务器
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
1. 修改nginx配置文件/etc/nginx/nginx.conf

.. code-block:: none
	:linenos:

	#1. 在server{}中添加如下
	location /audio/ {
	            root /root/; #对应的本地目录是/root/audio
	            autoindex on;
	        }
	#2. 把第一行改为user root;而不是user nginx;因为要访问/root

2. 重启nginx

方向代理
^^^^^^^^^^^^
1. 定义上游服务器


使用squid配置虚拟机成http代理（失败）
---------------------------------------
想在pc上调试“语音转写API”，只能通过虚拟机来转发请求，因为pc不能访问“语音转写server”。

安装squid
^^^^^^^^^^^^^
yum -y install squid

配置squid
^^^^^^^^^^^
参考链接：https://hostpresto.com/community/tutorials/how-to-install-and-configure-squid-proxy-on-centos-7/

.. code-block:: none
	:linenos:

	#vim /etc/squid/squid.conf
	//添加 acl localnet src 172.17.0.0/16  //我的笔记本ip段
	access_log /var/log/squid/access.log //不过好像不起作用
	http_access allow all

启动squid
^^^^^^^^^^^^
.. code-block:: none
	:linenos:

	//启动
	#systemctl start squid
	//查看3128已经在运行服务了
	#netstat -ntpl | grep 3128 
	//重启
	# systemctl restart squid
	//To automatically start Squid at boot time you can run the following command.
	#systemctl enable squid
	//To view the status of Squid service, run the following command.
	#systemctl status squid

打开虚拟机centos防火墙
^^^^^^^^^^^^^^^^^^^^^^^^^
#firewall-cmd --zone=public --add-port=3128/tcp(永久生效再加上 --permanent) //在指定区域打开端口（记得重启防火墙）
#firewall-cmd --reload //重启防火墙

查看日志 squid
^^^^^^^^^^^^^^^^^^
tail -f /var/log/squid/access.log

tail -f /var/log/squid/cache.log

在request中使用代理
^^^^^^^^^^^^^^^^^^^^
.. code-block:: none
	:linenos:

	import requests

	proxies = {
	  "http": "http://172.22.27.85:3128",
	}

	requests.get("http://example.org", proxies=proxies)

使用tinyproxy搭建http代理(实际使用)
---------------------------------------
背景：想在pc上调试“语音转写API”，只能通过虚拟机来转发请求，因为pc不能访问“语音转写server”。

安装
^^^^^
yum -y install tinyproxy

修改配置
^^^^^^^^^^^
/etc/tinyproxy/tinyproxy.conf

1. 修改端口号，配置文件第23行，内容如下：

Port 8001

注意，虚拟机上允许pc访问的端口是8000-8120

2. 修改允许访问的IP，配置文件第211行，内容如下：

Allow 127.0.0.1

将127.0.0.1修改为使用这个代理的客户机的IP，如果你想任何人都可以访问，把这行前面加个#注释掉就可以了

修改防火墙
^^^^^^^^^^^
firewall-cmd --zone=public --add-port=8001/tcp --permanent

firewall-cmd --reload

使用命令
^^^^^^^^^^
启动 ：systemctl start tinyproxy.service

停止 ：systemctl stop tinyproxy.service

重新启动 ：systemctl restart tinyproxy.service

开机启动 ：systemctl enable tinyproxy.service

查看状态 ：systemctl status tinyproxy.service

取消开机启动 ：systemctl disable tinyproxy.service

创建虚拟环境
--------------

.. code-block:: none
	:linenos:

	在/root/newsbrief目录下
	# virtualenv -p python3 nb-env  //在/root/newsbrief下建立了子目录nb-env
	# source nb-env/bin/activate

安装程序需要的python包
----------------------------
在虚拟环境下安装

#pip install flask

# pip install chardet

# pip install requests

# pip install flask_cors

访问flask的端口
-----------------
8002

放在后台运行
-------------
1. 参考链接：https://www.ibm.com/developerworks/cn/linux/l-cn-nohup/index.html

#启动虚拟环境

#setsid python xxx.py

#ps -aux |grep xxx //查看所有在后台运行的进程