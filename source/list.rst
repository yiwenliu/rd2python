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

排序
------
谈到list排序，肯定想到的就是class List的成员函数list.sort(cmp=None, key=None, reverse=False)，但是有一个隐含的至关重要的概念"sorting key"被忽视了，sort()真正上是依据list items对应的"sorting key"来对items排序的。

1. 默认情况下，list中每个item的值是"sorting key"

.. code-block:: python
    :linenos:

    # vowels list
    vowels = ['e', 'a', 'u', 'o', 'i']

    # sort the vowels
    vowels.sort()

    # print vowels
    print('Sorted list:', vowels) #Sorted list: ['a', 'e', 'i', 'o', 'u']

2. 如果想自定义"sorting key"，就要用到sort()中的key参数了，显然key参数的值肯定是一个函数，但是对这个函数有一些要求——key parameter to specify a function 

- to be called on each list element prior to making comparisons. 
- takes a single argument and returns a key to use for sorting purposes. 这个返回的key是“可比较的”就行，不一定是一个数值，例如list, tuple都行，但是dict不行。

.. code-block:: python
    :linenos:

    # take second element for sort
    def takeSecond(elem):
        return elem[1]

    # random list
    random = [(2, 2), (3, 4), (4, 1), (1, 3)]

    # sort list with key
    random.sort(key=takeSecond)

    # print list
    print('Sorted list:', random)
    #Sorted list: [(4, 1), (2, 2), (1, 3), (3, 4)]

.. _priority-sorting:

优先级排序
^^^^^^^^^^^^
有一份列表，其中的元素都是数字，要对其排序，排序时，要把出现在某个群组内的数字，放在群组外的那些数字之前。——言下之意，排序分为2个优先级（阶段），首先把属于某个群组中的数字放在列表前面，然后对前后两个部分分别排序。解决办法就是“使list item对应的sorting key带上prority信息”。

.. code-block:: python
    :linenos:

    numbers = [8, 3, 1, 2, 5, 4, 7, 6]
    group = {2, 3, 5, 7}

    def sortp(numbers, group):
        #把list item对应的sorting key扩充成(1, item) or (0, item)
        #元组的比较规则：首先比较下标为0的对应元素，如果相等，再比较下标为1的对应元素
        def helper(x):
            if x in group:
                return (0, x)
            else:
                return (1, x)
        numbers.sort(key=helper)
        #注意，并没有return numbers

    sortp(numbers, group)
    print(numbers)

遍历
-------
如果想在遍历list item同时返回item index，即返回一个元组(index, item), 该怎么办？

:ref:`带索引的迭代 <iterate-with-index>`

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

列表推导式
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

高级用法
^^^^^^^^^^^
参考了<effective python>8th

1. 使用2个条件,默认形成and
2. 2个循环, 多用于2维列表.但是如果是3维或者以上,请使用for语句
3. 列表推导式可生成多维列表
4. 循环可以搭配自己的条件来使用