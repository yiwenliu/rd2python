Property
==========
Reference Link
------------------
https://www.programiz.com/python-programming/property

what is property
--------------------
`property is a built-in function to return a property object <https://www.programiz.com/python-programming/property#dig>`_

python为什么要引入property
------------------------------
python的OO实现方式（无论类还是其实例都是object）对“实例属性”的操作太有随意性，没有对属性public, protected和private的访问权限控制，任何使用了某个class的代码，实例化一个class object后，可以给这个object进行任意属性的操作（添加、修改等），哪怕这个属性根本和class无任何关系，例如 `An Example <https://www.programiz.com/python-programming/property#eg>`_。

引入property的目的：

- 对操作实例属性必须加以控制，包括，参数检查、类型检查、读写限制等

注意：虽然可以定义set和get方法，例如在这段代码中 `Using Getters and Setters <https://www.programiz.com/python-programming/property#using>`_, 但是，仍然无法强制客户端代码使用这两个方法来读写 **_temperature** 属性

- 提供一种访问实例属性的统一的简单的方法

想要达到的效果是，客户端代码可以直接使用“对象.属性名”来读写属性，但是程序会自动执行对应的get, set函数。

python中和属性相关的魔术方法
----------------------------
既然property是为了控制属性访问的，那么python中和属性相关的魔术方法可否实现property的功能呢？

1. __getattr__(self, name)

客户端代码出现self.name， 而name是一个不存在的属性时，这个魔术方法就会被调用。

2. __getattribute__()

这个方法要谨慎使用，因为__getattribute__() is invoked before looking at the actual attributes on the object, and so can be tricky to implement correctly. You can end up in **infinite recursions** very easily.

3. __setattr__(self, name, val)

在客户端代码中出现self.name=val时，这个魔术方法就会自动调用。显然，python有这个魔术方法的默认实现。

----------------------------

上述三个魔术方法是针对属性的，而下面两个是针对class的，也是property用到的。定义了这两个方法的class就称为 :ref:`descriptor <descriptor>`

1. __get__()
2. __set__()

Look inside the property
--------------------------
1. 定义class时使用了decorator function property()把“类方法”变成了“类属性”，类实例通过访问这些“类属性”来间接操作对应的“实例属性”。

2. `使用property()的初级版本代码 <https://www.programiz.com/python-programming/property#power>`_ 可以看清楚上述原理。

- 最重要的代码是class Celsius定义的最后一行temperature = property()

- the actual temperature value is stored in the private variable _temperature. The attribute temperature is a property object which provides **interface** to this private variable.

- 代码中的temperature是一个property object（属性对象），是一个类属性吗？但是，从客户端代码的执行情况来看，又像是一个实例属性？因为在执行__init__()中的self.temperature = temperature语句时，set_temperature()被执行了？