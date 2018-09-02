python IO
==============
Intro
------------
python程序运行时，需要从外界获取数据。这些underlying resource包括

- a real on-disk file
- standard input/output（键盘）
- in-memory buffers, 
- sockets, 
- pipes

python提供了一种统一的方式来对这些resource进行操作（读写）：

1. 首先，open()这些资源，返回 a file object，又可以称为stream或者file-like object
2. 然后，对这些资源的读写通过the file object的read()和write()方法来完成
3. 注意，针对不同的resource，open()需要设置不同的参数，如此，the file object在read, write时就能匹配 **the type of data you give to or get from the resource**.

I/O分类
-----------
I/O分类的基础共识
^^^^^^^^^^^^^^^^^^^^^^
1. python认为I/O边界的字符序列都是非unicode字符
2. python3有unicode字符序列类型str，和包含原始8位值的字符序列类型bytes。从stream读入的字符序列总得归入这两类中的一类。

text I/O
^^^^^^^^^^^
定义：In text mode (the default, or when 't' is included in the mode argument), the contents of the file are returned as str, the bytes having been first decoded using a platform-dependent encoding or using the specified encoding if given.

换言之：读入的非unicode字符序列得转化成str实例。但是，得知道编码方式才能decode。有两种方式得到编码方式，1）调用open()是的encoding参数；2）locale.getpreferredencoding()返回的平台相关的编码方式。

binary I/O
^^^^^^^^^^^^^
定义：Files opened in binary mode (including 'b' in the mode argument) return contents as bytes objects without any decoding.

换言之：读入的非unicode字符序列直接就是bytes实例。

文件I/O的正确做法
--------------------
始终用'b'来open()

因为
1. 对于非文本文件，例如，image文件，根本无法转化为unicode字符，读入python后，只能是bytes对象
2. 对于文本文件，编码方式不确定，容易导致读取字符序列后对其decode的失败，所以也先以bytes对象读入python中，想办法 :ref:`确定其编码方式 <detect-codec>` 后，再decode成str对象。