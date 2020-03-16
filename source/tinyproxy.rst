使用tinyproxy搭建http代理
=====================================
背景：想在pc上调试“语音转写API”，只能通过虚拟机来转发请求，因为pc不能访问“语音转写server”。

安装
----------
yum -y install tinyproxy

修改配置
--------------
/etc/tinyproxy/tinyproxy.conf

1. 修改端口号，配置文件第23行，内容如下：

Port 8001

注意，虚拟机上允许pc访问的端口是8000-8120

2. 修改允许访问的IP，配置文件第211行，内容如下：

Allow 127.0.0.1

将127.0.0.1修改为使用这个代理的客户机的IP，如果你想任何人都可以访问，把这行前面加个#注释掉就可以了

修改防火墙
---------------------
firewall-cmd --zone=public --add-port=8001/tcp --permanent

firewall-cmd --reload

使用命令
---------------
启动 ：systemctl start tinyproxy.service

停止 ：systemctl stop tinyproxy.service

重新启动 ：systemctl restart tinyproxy.service

开机启动 ：systemctl enable tinyproxy.service

查看状态 ：systemctl status tinyproxy.service

取消开机启动 ：systemctl disable tinyproxy.service