Closure
=========
Reference link
^^^^^^^^^^^^^^^^
https://www.programiz.com/python-programming/closure

closure的先导概念——nested function访问nonlocal variable
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
`参考代码 <https://www.programiz.com/python-programming/closure#nonlocal>`_

- We can see that the nested function printer() was able to access the non-local variable msg of the enclosing function.

Python为什么要引入closure概念
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
- 为了替代类。众所周知，class封装了属性和方法，方法除了使用被调用时传入的形参，就只能访问class中定义的属性了。closure就是为了实现类似“类方法”的作用。
- Closures can avoid the use of global values and provides some form of data hiding.

**Q**：一个class可以定义多个方法，那么在一个enclosing function中是否可以定义多个nested function(closure)？如何决定返回哪一个呢？

**A**:But when the number of attributes and methods get larger, better implement a class.

What the closure really is
^^^^^^^^^^^^^^^^^^^^^^^^^^^
closure is a tech by which some data in the enclosing scope gets attached to the nested function scope, that is we have a closure in Python when a nested function references a value in its enclosing scope.

How to create a closure
^^^^^^^^^^^^^^^^^^^^^^^^^
1. 在python中创建closure，必须要满足 `三个条件 <https://www.programiz.com/python-programming/closure#when>`_

2. `Examples of defining a Closure Function <https://www.programiz.com/python-programming/closure#define>`_

what the closure function is
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
当我们谈论“闭包函数”时，到底指的而是enclosing function还是nested function？

- 其实，都不是，在下面的代码中 **times3** 才是"闭包函数"

.. code-block:: python
	:linenos:

	def make_multiplier_of(n):
	    def multiplier(x):
	        return x * n
	    return multiplier

	# Multiplier of 3
	times3 = make_multiplier_of(3)

	#closure function have a __closure__ attribute that returns a tuple of cell objects 
	>>> times3.__closure__
	(<cell at 0x0000000002D155B8: int object at 0x000000001E39B6E0>,)

How does a closure work
^^^^^^^^^^^^^^^^^^^^^^^^
1. call the enclosing function with some parameters, so the closure can take the passing parameters and the nonlocal variable as **Initialization Parameters** just like which are passed when creating an object but has not been called yet
2. call the closure to run the closure body

