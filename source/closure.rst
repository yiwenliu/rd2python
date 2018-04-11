Closure
=========
Reference link
^^^^^^^^^^^^^^^^
https://www.programiz.com/python-programming/closure

Python为什么要引入closure概念
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
- 为了替代类。众所周知，class封装了属性和方法，方法除了使用被调用时传入的形参，就只能访问class中定义的属性了。closure就是为了实现类似“类方法”的作用。
- Closures can avoid the use of global values and provides some form of data hiding.

How to create a closure
^^^^^^^^^^^^^^^^^^^^^^^^^
`Defining a Closure Function <https://www.programiz.com/python-programming/closure#define>`_, `When do we have a closure <https://www.programiz.com/python-programming/closure#when>`_

**Q**：一个class可以定义多个方法，那么在一个enclosing function中是否可以定义多个nested function(closure)？如何决定返回哪一个呢？
**A**:But when the number of attributes and methods get larger, better implement a class.

How does a closure work
^^^^^^^^^^^^^^^^^^^^^^^^
1. call the enclosing function with some parameters, so the closure can take the passing parameters and the nonlocal variable as **Initialization Parameters** just like which are passed when creating an object but has not been called yet
2. call the closure to run the closure body

what the function really is in python
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
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