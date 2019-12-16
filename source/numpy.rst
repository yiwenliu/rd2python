Numpy
========
安装
-------
失败的方法
^^^^^^^^^^^^^
.. note::
    这种安装方法的问题是，pycharm无法import；并且也无法安装依赖包。

在pycharm中，用pip安装时，显示找不到包。

在https://www.lfd.uci.edu/~gohlke/pythonlibs/#numpy中，选择合适的版本下载，

- cp37 - CPython version 3.7
- win_amd64 - this has been compiled for 64-bit Windows. 
- .whl - whl格式本质上是一个压缩包，

安装命令：

.. code-block:: none

    >pip install "C:\Users\xinhua\Downloads\numpy-1.17.4+mkl-cp37-cp37m-win_amd64.whl"

How to
^^^^^^^^^
在pycharm中升级了pip后，pip install pandas时，顺便以依赖包的形式安装了。

用pip安装的好处是可以安装依赖包，而采用whl的话，就不行。