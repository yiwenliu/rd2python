Dict
=======
在pyton3中dict的用法有一些变化。

用key查找value而不产生异常
---------------------------------
dict_obj.get(key[, default])

Return the value for key if key is in the dictionary, else default. If default is not given, it defaults to None, so that this method **never raises a KeyError**.

Dict comprehension
------------------------
1. 用{}包围comprehension可以生成dict or  set object

<effective python> p16

Dict view object
--------------------
要遍历dict对象的keys，values或者每一个items，要用到dict的方法，.keys(), .values(), .items()。这3个方法返回的对象都属于Dict view object，它们的特点是 **when the dictionary changes, the view reflects these changes.**

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