Generator
============
Reference
^^^^^^^^^^^
学习generator时主要参考了两个链接：

- `python生成器的优点-zhihu <https://www.zhihu.com/question/24807364>`_
- `Python Generators <https://www.programiz.com/python-programming/generator>`_

When to use generator
^^^^^^^^^^^^^^^^^^^^^^^
我的理解是只要生成list等sequence的地方，就可以考虑使用generator

Why generator when there's iterator
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
用定义函数来取代定义类，以简化代码。

How to define a generator
^^^^^^^^^^^^^^^^^^^^^^^^^^^
generator并不像iterator那样用class来定义，Simply speaking, a generator is a function that returns an object (iterator) which we can iterate over (one value at a time).

python中generator的两种定义方式：

1. 以前经常使用的列表推导式

- generator expression creates an anonymous generator function.
- using round parentheses.

.. code-block:: none

	>>> ge = (x**2 for x in [1,2,3])
	>>> ge
	<generator object <genexpr> at 0x00000000024B8AF0>
	>>> type(ge)
	<class 'generator'>

2. 生成器函数

If a function contains at least one yield statement (it may contain other yield or return statements), it becomes a generator function. 无论是有多个yield()语句，还是在for loop中调用yield()，例如 `reverse the string <https://www.programiz.com/python-programming/generator#with-loop>`_.

The difference is that, while a return statement terminates a function entirely, yield statement pauses the function saving all its states and later continues from there on successive calls.

.. code-block:: python
	:linenos:

	# A simple generator function
	def my_gen():
	    n = 1
	    print('This is printed first')
	    # Generator function contains yield statements
	    yield n

	    n += 1
	    print('This is printed second')
	    yield n

	    n += 1
	    print('This is printed at last')
	    yield n

generator function的执行过程
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
1. 使用next() 

.. code-block:: python
	:linenos:

	# It returns an iterator object but does not start execution immediately.
	>>> a = my_gen()

	# We can iterate through the items using next().
	# 代码从第3行开始执行至第一个yield语句, the function is paused and the control is transferred to the caller.
	#yield后表达式的值被返回给caller
	>>> next(a)
	This is printed first
	1

	# Local variables and theirs states are remembered between successive calls.
	# my_gen()从上次挂起的地方继续执行至第二个yield语句
	>>> next(a)
	This is printed second
	2

	>>> next(a)
	This is printed at last
	3

	# Finally, when the function terminates, StopIteration is raised automatically on further calls.
	>>> next(a)
	Traceback (most recent call last):
	...
	StopIteration
	>>> next(a)
	Traceback (most recent call last):
	...
	StopIteration

2. 使用for loop

.. code-block:: none
	:linenos:

	# my_gen() return an iterable iterator
	# item就是my_gen()中每一条yield语句后表达式的值
	for item in my_gen():
	    print(item)
	#输出如下
	This is printed first
	1
	This is printed second
	2
	This is printed at last
	3    

Why using generator not iterator class
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Suppose we have a log file from a famous fast food chain. The log file has a column (4th column) that keeps track of the number of pizza sold every hour and we want to sum it to find the total pizzas sold in 5 years.
`Pipelining Generators <https://www.programiz.com/python-programming/generator#use>`_

Attention
^^^^^^^^^^
生成器只能遍历一次