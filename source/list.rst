list
======
PyListObject in python2
-----------------------------

class list in python3
------------------------
class list([iterable])

初始化
---------

1. 初始化一个长度为N的list object

下面的代码示范的3种初始化list object的方法中，2种对的，1种错误。

.. code-block:: python
    :linenos:

    #1.用列表推导式初始化
    >>> lt = [None for i in range(5)]
    >>> lt
    [None, None, None, None, None]
    >>> len(lt)
    5
    #2.Using the type constructor
    >>> lt = list(range(5))
    >>> lt
    [0, 1, 2, 3, 4]
    #3.不可以直接指定list的长度
    >>> lt = list(5)
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    TypeError: 'int' object is not iterable

访问单个元素而不产生异常
--------------------------
访问列表中单个元素时，下标越界，会导致异常，IndexError: list index out of range

然而，切割列表时，即便start和end索引越界也不会出问题，只是返回空列表，[]

列表切割
---------
1. 列表切割在复制语句的右侧，会生成新的list object。所谓“全新的列表”，按照《python源码剖析》中p64“代码清单4-1”对创建PyListObject的2步过程的解释，就是ob_item指向了新建的“元素指针列表”。

.. code-block:: python
    :linenos:

    a = [1,2,3,4,5]
    b = a[1:] //[2,3,4,5]
    >>> b[0] = 7
    >>> b
    [7, 3, 4, 5]
    >>> a
    [1, 2, 3, 4, 5]

2. 列表切割在赋值语句的左侧，就是replace的操作，如果赋值语句右侧的list的长度“小于”左侧，还有remove的操作。参考《python源码》p72 list_ass_slice()函数的解释

.. code-block:: python
    :linenos:

    >>> c
    [10, 'a', 'b', 'c', 'd', 4, 5]
    #['b', 'c', 'd']换成了['f','g']
    >>> c[2:5] = ['f','g']  
    >>> c
    [10, 'a', 'f', 'g', 4, 5]

列表复制
---------------
1. 没有新建列表的复制

.. code-block:: python
    :linenos:

    >>> a
    [1, 2, 3, 4, 5]
    >>> c = a //c和a是同一个列表
    >>> c
    [1, 2, 3, 4, 5]
    >>> c[0] = 9
    >>> c
    [9, 2, 3, 4, 5]
    >>> a
    [9, 2, 3, 4, 5]

2. 新建了列表的复制

.. code-block:: python
    :linenos:

    >>> a
    [9, 2, 3, 4, 5]
    #列表切割在赋值语句的右侧的意义见上一小节
    >>> d = a[:]  
    >>> d
    [9, 2, 3, 4, 5]
    >>> d[0] = 1
    >>> d
    [1, 2, 3, 4, 5]
    >>> a
    [9, 2, 3, 4, 5]

list comprehension
------------------------
根据一份列表来制作另一份列表时，就用“列表推导式”。

1. 用[]包围的comprehension生成的是一个真正的list，
2. 用()包围的comprehension是一个generator object，而不是一个tuple。

.. code-block:: python
    :linenos:

    >>> a
    [10, 'a', 'f', 'g', 4, 5]
    >>> tu = (x for x in a)
    >>> tu
    <generator object <genexpr> at 0x0000000002B86C50>
    >>> type(tu)
    <class 'generator'>

3. 用{}包围的comprehension生成的是一个dict object