读取命令行参数
================
从C语言时代就使用的方式

.. code-block:: python
	:linenos:

	import sys
	arg1 = sys.argv[1]

确实够简单，但是如果想实现如下功能，该怎么办呢？

- sys.argv[1]是使用"positional arguments"的一种，如果想给"positional arguments"取个名字，并用这个名字（key）来访问呢？
- 让命令行可以使用"optional arguments"，并用key来访问呢？
- 给命令行产生进行类型检查、设置默认值、设置取值范围？

这是，就该使用argparse module了， 
- `python argparse用法总结 <https://www.jianshu.com/p/fef2d215b91d>`_
- `官方文档 <https://docs.python.org/3/library/argparse.html>`_