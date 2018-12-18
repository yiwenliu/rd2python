Dict
=======
在pyton3中dict的用法有一些变化。

PyDictObject in python2
---------------------------
关联式容器
^^^^^^^^^^^^
dict属于“关联式容器”，书中提及的实现算法有两个，RB-Tree（红黑树）和散列表（hash table）。PyDictObject采用了后者。

散列表
^^^^^^^^^^
散列表的关键概念：

1. 散列表是一片连续的内存区域
2. 散列函数，散列值
3. 冲突解决算法，例如，“开链法”，“线性探测法”，python采用“开放定址”法。
4. 各种“冲突解决算法”都有对应的“二次探测函数”，“冲突探测链”
5. "冲突探测链"上的每一个点都是某个entry可能的位置
6. 为了支持"冲突探测链",entry实现了一个"状态转换图",p80,图5-2

对象定义
^^^^^^^^^^^^^
1. key-value对的结构体，PyDictEntry p79
2. PyDictObject结构体定义p80；图示p81，图5-3
3. 散列表的4个关键概念在PyDictObject的定义中都有对应的实现

何为冲突
^^^^^^^^^^

用key查找value而不产生异常
---------------------------------
dict_obj.get(key[, default])

Return the value for key if key is in the dictionary, else default. If default is not given, it defaults to None, so that this method **never raises a KeyError**.

字典表达式
------------------------
1. 用{}包围comprehension可以生成dict or  set object

<effective python> p16

遍历dict
--------------------
首先，dict并非iterable，所以不能直接用于"for loop"

1. 遍历dict对象的keys，dict.keys()
2. 遍历dict对象的values, dict.values()
3. 或者每一个items， dict.items()。

这3个方法返回的对象都属于Dict view object，它们的特点是 **when the dictionary changes, the view reflects these changes.**

.. code-block:: python
    :linenos:

    >>> ll
    {10: 10, 'a': 'a', 'f': 'f', 'g': 'g', 4: 4, 5: 5}
    >>> tt = ll.keys()
    >>> type(tt)
    <class 'dict_keys'>
    >>> vv = ll.values()
    >>> vv
    dict_values([10, 'a', 'f', 'g', 4, 5])
    >>> type(vv)
    <class 'dict_values'>
    >>> ii = ll.items()
    >>> type(ii)
    <class 'dict_items'>
    >>> ii
    dict_items([(10, 10), ('a', 'a'), ('f', 'f'), ('g', 'g'), (4, 4), (5, 5)])
    #############################################################################
    >>> ll[10]=20
    >>> ll
    {10: 20, 'a': 'a', 'f': 'f', 'g': 'g', 4: 4, 5: 5}
    #when dict object changed, the view object also changed
    >>> vv
    dict_values([20, 'a', 'f', 'g', 4, 5])

Dict.keys()等方法的返回值可以用于"for循环语句",说明返回对象是iterable,有关python3中"迭代器"的实现,请参看本文档的相应章节.

.. code-block:: python
    :linenos:

    >>> ll
    {10: 20, 'a': 'a', 'f': 'f', 'g': 'g', 4: 4, 5: 5}
    >>> tt = ll.keys()
    >>> tt
    dict_keys([10, 'a', 'f', 'g', 4, 5])
    #tt is iterable
    >>> for i in tt:
    ...     print(i)
    ...
    10
    a
    f
    g
    4
    5
    >>> it = iter(tt)
    #dict_keyiterator在python3的类图中的位置可以参见"Sequence"章节中的uml图
    >>> it
    <dict_keyiterator object at 0x00000000022E05E8>
    >>> from collections.abc import *
    >>> isinstance(it, Iterator)
    True