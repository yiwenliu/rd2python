python版本管理和虚拟环境
===========================
pyenv和virtualenv的区别
---------------------------
+------------+------------------------+--------------------------------------+
| pyenv      | python 版本的管理      | 通过修改环境变量的方式实现           |
+------------+------------------------+--------------------------------------+
| virtualenv | python的包的多版本管理 | 通过切换目录来实现不同包环境间的切换 |
+------------+------------------------+--------------------------------------+

profile、bashrc、bash_profile区别
------------------------------------
+-----------------+----------+---------------+------------+-----------------+
| /etc/profile    | 每个用户 | 登录时        | 仅执行一次 | 重启系统生效    |
+-----------------+----------+---------------+------------+-----------------+
| ~/.bash_profile | 当前用户 | 登录时        | 仅执行一次 | 重启系统生效    |
+-----------------+----------+---------------+------------+-----------------+
| ~/.bashrc       | 当前用户 | 登录时        | 执行一次   |                 |
+                 +          +---------------+------------+-----------------+
|                 |          | 打开新的shell | 执行一次   | 打开新shell生效 |
+-----------------+----------+---------------+------------+-----------------+

pyenv
------------
使用pyenv installer
^^^^^^^^^^^^^^^^^^^^^^^^
1. 安装和卸载步骤见 https://github.com/pyenv/pyenv-installer
2. 把下面的三条指令加到~/.bashrc

.. code-block:: none
	:linenos:

	export PATH="~/.pyenv/bin:$PATH"
	eval "$(pyenv init -)"
	eval "$(pyenv virtualenv-init -)"

3. 安装完成后，要重启shell
4. 默认安装路径~/.pyenv
5. 列出可以安装的python版本（安装确认）

.. code-block:: none
	:linenos:

	# pyenv install --list

使用pyenv管理python版本
^^^^^^^^^^^^^^^^^^^^^^^^^^^^
1. Install Python versions into $~/.pyenv/versions. For example, to download and install Python 2.7.8, run:

.. code-block:: none
	:linenos:

	$ pyenv install 2.7.8

2. Uninstalling Python Versions，参考https://github.com/pyenv/pyenv#uninstalling-python-versions

pyenv-virtualenv
--------------------
使用pyenv 来管理python，使用 pyenv-virtualenv 插件来管理多版本 python包。

安装
^^^^^^^^

使用自动安装pyenv 后，它会自动安装部分插件，通过pyenv-virtualenv 插件可以很好的和 virtualenv 结合：

.. code-block:: none
	:linenos:

	[root@linux3311 ~]# cd .pyenv/plugins/
	[root@linux3311 plugins]# ll
	insgesamt 24
	drwxr-xr-x. 4 root root 4096 19. Jun 05:17 pyenv-doctor
	drwxr-xr-x. 5 root root 4096 19. Jun 05:18 pyenv-installer
	drwxr-xr-x. 4 root root 4096 19. Jun 05:18 pyenv-update
	drwxr-xr-x. 7 root root 4096 19. Jun 05:18 pyenv-virtualenv

使用
^^^^^^^
- 创建虚拟环境 

.. code-block:: none
	:linenos:

	#若不指定python 版本，会汇报认使用当前环境python版本。
	#python3.6.4的安装路径/root/.pyenv/versions/3.6.4
	#虚拟环境的路径/root/.pyenv/versions/3.6.4/envs/tdd3
	#还有符号链接/root/.pyenv/versions/tdd3 -> /root/.pyenv/versions/3.6.4/envs/tdd3
	$ pyenv virtualenv 3.6.4 tdd3 

- 列出当前虚拟环境 pyenv virtualenvs
- 激活虚拟环境 pyenv activate
- 退出虚拟环境 pyenv deactivate
- 删除虚拟环境 pyenv uninstall my-virtual-env

在windows环境创建虚拟环境
---------------------------------
anaconda
^^^^^^^^^^^^^^^^
在windows中原本安装了anaconda，于是就用如下命令创建虚拟环境

(base) C:\Users\xx>conda create -n flask-env

但是，总是报错，显示https://www.anaconda.com主站连不上了，浏览器确实打不开了。

注意，4.5.4版本的conda似乎默认在base虚拟环境下。

virtualenv
^^^^^^^^^^^^^^^^
在anaconda prompt中执行如下命令

.. code-block:: none
	:linenos:

	(base) C:\Users\xx> pip install virtualenv
	(base) C:\Users\xx> virtualenv --version
	(base) C:\Users\xx> cd my_project_folder
	(base) C:\Users\xx> virtualenv my_project_env
	#激活虚拟环境
	(base) C:\Users\xx> my_project_env\Scripts\activate
	#推出虚拟环境
	(flask-env) (base) D:\newsbrief> deactivate

pycharm
^^^^^^^^^^^^^^^^
在pycharm中创建project时，会提示"New Virtualenv environment"。

在pycharm中安装python包
++++++++++++++++++++++++++++++
在pycharm的最下面，有Terminal选项卡，从提示符可以看出，是在虚拟环境中