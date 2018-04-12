Decorator
============
Relationship between decorator and closure
--------------------------------------------
Decorators in Python make an extensive use of closures as well.

python为什么要引入decorator的概念
---------------------------------
在设计模式中有Decorator这一模式，用来生成对象，特点是有一个核心类和一堆装饰类。

Decorators in python is to add functionality to an existing code(function). 

- **Q**:对于一个不能修改函数体的现存的函数，如何才能给它增加功能呢？

**A**:把decorated function作为参数传入decorator

- **Q**:既然以decorated function作为形参，decorator为什么不在其函数体中，在调用decorated function前后，实现它想增加的功能就可以了呢，为什么还要以closure的方式返回一个函数对象呢？

**A**:因为decorated 客户function实现的是核心功能，所以在“客户端代码”中，还是要出现调用decorated function的形式，decoreated_function_name()

How to use
------------
1. 第一步，定义一个decorator function

- 如何区分decorator和普通function呢？以function作为参数的就是decorator
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