会话控制器
==========
Definition
^^^^^^^^^^^^
一个会话控制器就是一个实现了 __enter__(self)和 __exit__()两个魔术方法的class。

最常使用的就是如下代码。open()返回了一个IOBase obj，在class IOBase定义中就实现了上述两个魔术方法。

.. code-block:: python
	:linenos:

	with open('foo.txt') as bar:
	  # perform some action with bar
	  pass

with语句的执行过程
^^^^^^^^^^^^^^^^^^^
紧跟with的对象的__enter__ 的返回值被 with 语句的目标或者 as 后的名字绑定。

自定义会话控制器
^^^^^^^^^^^^^^^^^^
.. code-block:: python
	:linenos:

	class Closer:
	'''通过with语句和一个close方法来关闭一个对象的会话管理器'''

	def __init__(self, obj):
	    self.obj = obj

	def __enter__(self):
	    return self.obj # bound to target

	def __exit__(self, exception_type, exception_val, trace):
	    try:
	        self.obj.close()
	    except AttributeError: # obj isn't closable
	        print 'Not closable.'
	        return True # exception handled successfully

客户端代码如下：

.. code-block:: none
	:linenos:

	>>> from magicmethods import Closer
	>>> from ftplib import FTP
	>>> with Closer(FTP('ftp.somesite.com')) as conn:
	...     conn.dir()
	...
	>>> conn.dir()
	>>> with Closer(int(5)) as i:
	...     i += 1
	...
	Not closable.
	>>> i
	6