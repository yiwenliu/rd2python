Function
==========
what the function really is in python
---------------------------------------
- Besides that function is an object, even everything in Python (Yes! Even classes) are objects. 

- Various different names can be bound to the same function object.

.. code-block:: python
	:linenos:

	def first(msg):
	    print(msg)    

	first("Hello")

	second = first
	second("Hello")

- Functions can be passed as arguments to another function.

- a function can return another function, like the closure

- In fact, any object which implements the special method __call__() is termed callable. So, in the most basic sense, a decorator is a callable that returns a callable.

Closure
--------------
Reference link
^^^^^^^^^^^^^^^^
https://www.programiz.com/python-programming/closure

nested function&enclosing function
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
`参考代码 <https://www.programiz.com/python-programming/closure#nonlocal>`_

- We can see that **the nested function** printer() was able to access the non-local variable msg of **the enclosing function**.

What the closure really is
^^^^^^^^^^^^^^^^^^^^^^^^^^^
closure is a tech by which some data in the enclosing scope gets attached to the nested function scope, that is we have a closure in Python when a nested function references a value in its enclosing scope.

当我们谈论“闭包函数”时，到底指的而是enclosing function还是nested function？其实，都不是，在下面的代码中 **times3** 才是"闭包函数"

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

How to create a closure
^^^^^^^^^^^^^^^^^^^^^^^^^
1. 在python中创建closure，必须要满足 `三个条件 <https://www.programiz.com/python-programming/closure#when>`_

- We must have a nested function (function inside a function).
- The nested function must refer to a value defined in the enclosing function.
- The enclosing function must return the nested function.

2. `Examples of defining a Closure Function <https://www.programiz.com/python-programming/closure#define>`_

使用闭包的场景
^^^^^^^^^^^^^^^^^^^
1. 在什么时候，会让人想起要使用闭包呢？——2个条件

- 为了可复用性，要用一个函数来封装某一个功能，例如列表排序
- 在封装过程中，不得不定义另外一个“辅助函数”(nested function)
e.g. :ref:`优先级排序 <priority-sorting>`

2. 如果nested function的状态要保持，即，enclosing function要用这个状态，还是把nested function定义为类，并令其实现__call__方法，如此，类实例就能被调用了。"effective python"p33-1，def sort_priority3()和class Sorter()

closure v.s. class
^^^^^^^^^^^^^^^^^^^^^^^^^
- 为了替代类。众所周知，class封装了属性和方法，方法除了使用被调用时传入的形参，就只能访问class中定义的属性了。closure就是为了实现类似“类方法”的作用。
- Closures can avoid the use of global values and provides some form of data hiding.

**Q**：一个class可以定义多个方法，那么在一个enclosing function中是否可以定义多个nested function(closure)？如何决定返回哪一个呢？

**A**:But when the number of attributes and methods get larger, better implement a class.

How does a closure work
^^^^^^^^^^^^^^^^^^^^^^^^
使用闭包的客户端代码要经过两次函数调用：

1. call the enclosing function with some parameters, so the closure can take the passing parameters and the nonlocal variable as **Initialization Parameters** just like which are passed when creating an object but has not been called yet
2. call the closure to run the closure body

Generator function
----------------------
参见 :ref:`Generator <generator-function>`