Execution model
===================
Every programming language has an execution model. 

执行模式的组成部分
-----------------------
1. compiler, 
2. interpreter 
3. a runtime system（主要部分）

a runtime system
^^^^^^^^^^^^^^^^^^^^^
a runtime system, primarily implements portions of an execution model, 其行为包括了“非应用程序本身所属的”其他任何行为。 

以python为例，它的runtime system的行为包括：

- 在客户编写的程序真正执行前，初始化进程对象，线程对象，frame对象
- putting parameters onto the stack before a function call
- 应用程序和runtime environment交互的gateway,例如，OS环境变量, disk I/O

Python's execution model
-----------------------------
在 `官方文档 <https://docs.python.org/3/reference/executionmodel.html#>`_ 中，python的“执行模式”只有三大点：

1. Structure of a program——code block
2. Naming and binding——python的名字解析规则
3. Exception

python和C的执行模式的比较
-------------------------------

+--------------------------------------+-----------------------------------------+---------------------------+
|                                      | C                                       | Python                    |
+--------------------------------------+-----------------------------------------+---------------------------+
| an indivisible unit of work          | statement                               | code block                |
+--------------------------------------+-----------------------------------------+---------------------------+
| unit标志                             | ;                                       | function, module, class   |
+--------------------------------------+-----------------------------------------+---------------------------+
| execution manner                     | serially(one unit at a time)            | frame决定了一个code block |
|                                      |                                         |                           |
| (order in which those units of work) | and sequentially(前面结束了,后面才开始) | 执行结束后,该如何继续     |
+--------------------------------------+-----------------------------------------+---------------------------+