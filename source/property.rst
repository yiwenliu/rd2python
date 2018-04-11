property
==========
Reference Link
^^^^^^^^^^^^^^^^
https://www.programiz.com/python-programming/property

what is property
^^^^^^^^^^^^^^^^^
`property is a built-in function to return a property object <https://www.programiz.com/python-programming/property#dig>`_

python为什么要引入property
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
python的OO实现方式（无论类还是其实例都是object）对“实例属性”的操作太有随意性，没有对属性public, protected和private的访问权限控制，任何使用了某个class的代码，实例化一个class object后，可以给这个object进行任意属性的操作（添加、修改等），哪怕这个属性根本和class无任何关系，例如 `An Example <https://www.programiz.com/python-programming/property#eg>`_。

引入property的目的：

- 对操作实例属性必须加以控制，包括，参数检查、类型检查、读写限制等

注意：虽然可以定义set和get方法，例如在这段代码中 `Using Getters and Setters <https://www.programiz.com/python-programming/property#using>`_, 但是，仍然无法强制客户端代码使用这两个方法来读写 **_temperature** 属性

- 提供一种访问实例属性的统一的简单的方法

想要达到的效果是，客户端代码可以直接使用“对象.属性名”来读写属性，但是程序会自动执行对应的get, set函数。

Look inside the property
^^^^^^^^^^^^^^^^^^^^^^^^^^
1. 定义class时使用了decorator function property()把“类方法”变成了“类属性”，类实例通过访问这些“类属性”来间接操作对应的“实例属性”。

2. `使用property()的初级版本代码 <https://www.programiz.com/python-programming/property#power>`_ 可以看清楚上述原理。

- 最重要的代码是class Celsius定义的最后一行temperature = property()

- the actual temperature value is stored in the private variable _temperature. The attribute temperature is a property object which provides **interface** to this private variable.

- 代码中的temperature是一个property object（属性对象），是一个类属性吗？但是，从客户端代码的执行情况来看，又像是一个实例属性？因为在执行__init__()中的self.temperature = temperature语句时，set_temperature()被执行了？