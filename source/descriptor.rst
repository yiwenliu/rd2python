.. _descriptor:

Descriptor
============
Design Pattern
----------------
strategy pattern

描述符协议
----------
1. 描述符协议由三个魔术方法组成：

- __get__(self, instance, owner=None)

定义当描述器的值被取得的时候的行为， instance 是“拥有者对象”的一个实例。 owner 是拥有者类本身

- __set__(self, instance, value)

定义当描述器值被改变时候的行为。 instance 是“拥有者类”的一个实例, value 是要设置的值

- __delete__(self, instance)

2. 和其他魔术方法的参数列表比较，上述三个魔术方法的传入参数中instance是很特别的，需要格外注意。

3. 描述符被分配给一个类，而不是实例。

创建描述符
-----------
有三种创建描述符的方法：

- 三种方法逐渐简化
- 不同的创建方法对客户端代码并无影响
- 关注客户端代码使用什么语句才能触发调用魔术方法。

创建描述符类
^^^^^^^^^^^^^

.. code-block:: python
	:linenos:

	class Descriptor(object): 
	    def __init__(self):
	        self._name = ''
	 
	    def __get__(self, instance, owner):
	        print "Getting: %s" % self._name
	        return self._name
	 
	    def __set__(self, instance, name):
	        print "Setting: %s" % name
	        self._name = name.title()
	 
	    def __delete__(self, instance):
	        print "Deleting: %s" %self._name
	        del self._name
	 
	class Person(object):
		"""
		Attributes:
			name: 一个类属性同时也是一个“描述符对象”
		"""
	    name = Descriptor()

在Anaconda prompt中使用这段代码：

.. code-block:: none
	:linenos:

	>>> from descriptor import *
	>>> user1 = Person()
	>>> user2 = Person()
	#__set__(self, user1, 'yiwen')被调用
	>>> user1.name = 'yiwen'
	Setting: yiwen
	#__get__(self, user2)被调用
	>>> user2.name
	Getting: Yiwen
	'Yiwen'

上面的代码中，

- 触发描述符魔术方法调用的是拥有者对象对描述符对象的使用，描述符对象使用自己的属性时反而不会触发描述符魔术方法，这一点和其他魔术方法的调用有很大的区别。
- class Person和class Descriptor是“组合关系”，
- class Person的对象作为参数传入class Descriptor的魔术方法__get__, __set__，于是，这些class Descriptor方法就能操作class Person的属性，像极了“策略模式”
- 从上面的客户端代码可以看出,user1和user2两个对象并没有真正拥有自己的_name属性，对class Descriptor稍作修改，就可以实现，见下述代码

.. code-block:: python
	:linenos:

	class Descriptor(object):

	    def __init__(self):
	        # self._name = ''
	        pass

	    def __get__(self, instance, owner):
	        print("Getting: %s" % instance._name)
	        # return self._name
	        return instance._name

	    def __set__(self, instance, name):
	        print("Setting: %s" % name)
	        # self._name = name.title()
	        instance._name = name.title()

	    def __delete__(self, instance):
	        print("Deleting: %s" % self._name)
	        # del self._name
	        del instance._name

使用property()
^^^^^^^^^^^^^^^^^

.. code-block:: python
	:linenos:

	class Person(object):
	    def __init__(self):
	        self._name = ''
	 
	    def fget(self):
	        print "Getting: %s" % self._name
	        return self._name
	     
	    def fset(self, value):
	        print "Setting: %s" % value
	        self._name = value.title()
	 
	    def fdel(self):
	        print "Deleting: %s" %self._name
	        del self._name
	    name = property(fget, fset, fdel, "I'm the property.")

这种方法无需手动创建“描述符类”，只需在拥有者类中定义下列三个方法中的一个或几个，然后调用property()。

- fget：属性获取方法
- fset：属性设置方法
- fdel：属性删除方法

注意：

1. 这三个方法不是魔术方法，方法名可以随便取，只需要传入property()的顺序不错就行。
2. fget、fset 和 fdel 方法是可选的，但是如果没有指定这些方法，那么将在尝试各个操作时出现一个 AttributeError 异常。例如，声明 name 属性时，fset 被设置为 None，然后开发人员尝试向 name 属性分配值。这时将出现一个 AttributeError 异常。

.. code-block:: none
	:linenos:

	#这种方法可以用于定义系统中的只读属性。
	name = property(fget, None, fdel, "I'm the property")

使用@property(属性修饰符)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
上面小节的代码中，类属性name = property()的定义形式就是Decorator。

在Decorator中引入了“修饰符”的概念@decorator, 用以更改函数的功能。

当用@decorator修饰一个函数时，到底发生了什么，可以参见Decorator的相关章节。

用@property来替代property()，可以对上小节的代码进一步简化

.. code-block:: python
	:linenos:

	class Person(object):
	 
	    def __init__(self):
	        self._name = ''
	 
	    #name = property(name)
	    @property
	    def name(self):
	        print "Getting: %s" % self._name
	        return self._name
	 
	    #name = name.setter(name)
	    @name.setter
	    def name(self, value):
	        print "Setting: %s" % value
	        self._name = value.title()
	 
	    #name = name.deleter(name)
	    @name.deleter
	    def name(self):
	        print ">Deleting: %s" % self._name
	        del self._name

其中：

- property(), name.setter(), name.deleter()都是decorator function
- name原本是方法，被@property改成了类属性，一个property object
- setter(), deleter()是property object所有的方法
- 客户端代码仍然使用user1.name对_name进行读写
- 如果要对_name属性的读、写和删除都要进行控制，那么必须有三个同名的decorated function
- 三个被修饰的name()实际上形成了一个 :ref:`decorator chain <decorator-chain>`

在运行时创建描述符
-------------------
`参考代码 <https://www.ibm.com/developerworks/cn/opensource/os-pythondescriptors/index.html>`_

- 如何在运行时给class添加type是property object的“类属性”？