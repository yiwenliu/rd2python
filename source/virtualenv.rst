在windows环境创建虚拟环境
===============================
anaconda
-----------
在windows中原本安装了anaconda，于是就用如下命令创建虚拟环境

(base) C:\Users\xx>conda create -n flask-env

但是，总是报错，显示https://www.anaconda.com主站连不上了，浏览器确实打不开了。

注意，4.5.4版本的conda似乎默认在base虚拟环境下。

virtualenv
--------------
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
----------
在pycharm中创建project时，会提示"New Virtualenv environment"。

在pycharm中安装python包
^^^^^^^^^^^^^^^^^^^^^^^^^^^^
在pycharm的最下面，有Terminal选项卡，从提示符可以看出，是在虚拟环境中