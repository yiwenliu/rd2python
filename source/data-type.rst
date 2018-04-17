Data type&Varialbe
====================
Data type
------------
分类方式1：

- simple data types ,like integers and strings.
- compound objects, which are objects containing other objects, like lists or class instances.


分类方式2：根据对象维护数据的可变性

- mutable objects,lists and dictionaries
- immutable object,：Number（数字）、String（字符串）、Tuple（元组）、Sets（集合），特点是，在对应的“类型对象”定义的所支持的操作完成之后，原来参与操作的任何一个对象都没有发生改变，例如，p32,PyInt_Type定义的int_add()操作

分类方式3：

- 定长对象：不同对象占用的内存大小不一样
- 变长对象：不同对象占用的内存可能不一样

slice operator：操作细节

list元素是simple data type和compound object时，行为的不同

Variable
----------
A variable is a way of referring to a memory location used by a computer program. A variable is a symbolic name for this physical location. 