Decorator
============
Relationship between decorator and closure
--------------------------------------------
Decorators in Python make an extensive use of closures as well.

python为什么要引入decorator
---------------------------------
在设计模式中有Decorator这一模式，用来生成对象，特点是有一个核心类和一堆装饰类。

Decorators in python is to add functionality to an existing code(function or class). 

How to use
------------
1. 第一步，定义一个decorator function

- 如何区分decorator和普通function呢？以function作为参数的就是decorator
- decorator为什么要返回nested function?因为要使用@decorator，就必须这么做
- 注意：其中nested function的形参必须和decorated functin相同，因为decorator要返回nested function object，那么如何定义一个可以修饰任意数量参数的“被修饰函数”的decorator呢？

.. code-block:: python
	:linenos:

	'''
	@args: will be the tuple of positional arguments and 
	@kwargs: will be the dictionary of keyword arguments.
	'''
	def works_for_all(func):
	    def inner(*args, **kwargs):
	        print("I can decorate any function")
	        return func(*args, **kwargs)
	    return inner

2. 第二步，对于decorated function使用decorator

.. code-block:: python
	:linenos:

	'''
	@make_pretty
	def ordinary():
	    print("I am ordinary")

	is equivalent to

	def ordinary():
	    print("I am ordinary")
	ordinary = make_pretty(ordinary)
	'''
	@decorator_name
	def decorated_function_name():
	  pass

3. 第三步，call the decorated function 

Examples
-----------
1. `在除法运算前检查除数非零 <https://www.programiz.com/python-programming/decorator#decorating>`_

- 存在一个实现除法运算的核心函数
- 在运行核心功能前需要实现一个检查的功能

.. _decorator-chain:

Decorator chain
------------------
`示例代码 <https://www.programiz.com/python-programming/decorator#chaining>`_

- 上面这个链接中的例子像极了decorator design pattern

decorator可以修饰的对象
-----------------------
@decorator可以修饰function, class等，因为在python中，function和class都是object

带参数的“函数decorator”
^^^^^^^^^^^^^^^^^^^^^^^^
前面所讲的decorator都以function作为参数，那么有没有可能给decorator function传入一些其他的非function参数呢？如此，可以发挥closure的作用。

场景和解决方法
+++++++++++++++
`给函数添加日志功能，同时允许用户指定日志的级别和其他的选项 <http://python3-cookbook.readthedocs.io/zh_CN/latest/c09/p04_define_decorator_that_takes_arguments.html>`_

上述链接所包含的代码中：

- decorate()首先是一个closure function，然后才是decorator function
- @logged()的实际执行过程在链接中也有讨论

带参数的“类型装饰器”
^^^^^^^^^^^^^^^^^^^^^
`重写类定义的某部分(方法)来修改它的行为，但是你又不希望使用继承或元类的方式 <http://python3-cookbook.readthedocs.io/zh_CN/latest/c09/p12_using_decorators_to_patch_class_definitions.html>`_

python自带的decorator
----------------------
@staticmethod
^^^^^^^^^^^^^^^
@classmethod
^^^^^^^^^^^^^^
@property
^^^^^^^^^^