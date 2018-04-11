Iterator(迭代器)
=================
Intro
^^^^^^
iterator是一种“设计模式”，和“观察者模式”、“访问者模式”同属于“面向任务的模式”，用于“执行及描述任务”。

各种语言实作迭代器的方式皆不尽同，有些面向对象语言像Java, C#, Ruby, Python, Delphi都已将迭代器的特性内建语言当中，完美的跟语言整合，我们称之隐式迭代器（implicit iterator）

迭代器另一方面还可以整合生成器（generator）。有些语言将二者视为同一界面，有些语言则将之独立化。

Reference
^^^^^^^^^^^
https://www.programiz.com/python-programming/iterator

迭代协议
^^^^^^^^^
Technically speaking, Python iterator object must implement two special methods, __iter__() and __next__(), collectively called the **iterator protocol**, 例子可见下一小节。

- __iter__()

一个实现这个成员方法的iterator就把自身也变成了一个iterable，就可以使用在 **for loop** 中了。见下面的小节“How for loop actually works”

Built-in functions using iterable in Python
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
python很对的内建的函数的参数都是iterable，例如

- sum(iterable[, start])
- max(iterable, \*iterables[,key, default])
- for element in iterable

what is iterable&iterator
^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Iterator in Python is simply an object that can be iterated upon. An object which will return data, one element at a time. In the other words, iterator必须实现__iter__()和__next__()

An object is called iterable if we can get an iterator from it throught iter() function.In the other words, iterable只需要实现__iter__()

built-in iterable containers
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Most of built-in containers in Python like: list, tuple, string etc. are iterables.

.. code-block:: none

	>>> x = [42,23,24]
	#从可迭代(iterable)对象中获取iterator
	#iter() in turn calls the __iter__() method
	>>> it = iter(x)
	>>> type(it)
	<class 'list_iterator'>
	>>> type(x)
	<class 'list'>
	#We use the next(), which in turn calls the __iter__() method, to manually iterate through all the items of an iterator. 
	#When we reach the end and there is no more data to be returned, it will raise StopIteration. 
	>>> next(it)
	42
	#next(obj) is same as obj.__next__()
	>>> it.__next__()
    23
    #built-in iterable container不能被next()直接调用
    >>> next(x)
	Traceback (most recent call last):
	  File "<stdin>", line 1, in <module>
	TypeError: 'list' object is not an iterator

Building Your Own Iterator in Python
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
1. 自定义iterator，就必须要满足“迭代协议”

2. Can build your own iterables in python?

3. 既然The __iter__() method returns the iterator object itself，客户端的使用iterator的代码又是next(iter(iterator))，那么为什么必须实现__iter__()呢，可以直接使用next(iterator)吗？

上面两个问题的答案是相同的，“实现了__iter__()的iterator自身就是一个iterable”，就可以使用在for循环中了。

4. Python generators are a simple way of creating iterators. 

How to use your own iterator
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
从客户端代码看，有两种使用方法，可以参考reference-Building Your Own Iterator in Python：

1. 第一步，new an iterator；第二步，next(the-new-iterator)
2. 第一步，new an iterator；第二步，for element in the-new-iterator

How for loop actually works
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
下面的代码中最重要的几点就是 

- iterable才能用到for语句中去
- element是next(iter_obj)的返回值

.. code-block:: none
	:linenos:

	for element in iterable:
	    # do something with element
	    pass

Is actually implemented as

.. code-block:: none
	:linenos:

	# create an iterator object from that iterable
	iter_obj = iter(iterable)
	# infinite loop
	while True:
	    try:
	        # get the next item
	        element = next(iter_obj)
	        # do something with element
	        pass
	    except StopIteration:
	        # if StopIteration is raised, break from loop
	        break

Why using iterator
^^^^^^^^^^^^^^^^^^^^^
iterator其实和定义一个函数以实现一个功能是相同的，为啥不定义一个函数算了呢？

- 设计模式中，惯用的伎俩就是把操作外化为类
- 语言提供统一的调用接口，iter(), next()