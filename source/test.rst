测试类型
=========
单元测试
-----------

链接：https://blog.csdn.net/huilan_same/article/details/52944782

定义：对类和函数进行测试。

- 原来都是写一个测试文件，其中直接实例化类对象或者调用函数，并打印输出。使用unittest module也是写测试文件，但是文件中定义test case class，test case class中的方法就是一个个的test case，由test case来实例化类对象或者调用函数，并且使用assertEqual来判断结果是否正确。
- 想要test case按照顺序执行，借助TestSuite
- 如果每次执行test case之前准备环境，或者在每次执行完之后需要进行一些清理，在test case class中添加方法，setUp() 和 tearDown()。
- 如果想要在所有case执行之前准备一次环境，并在所有case执行结束之后再清理环境，我们可以在test case class中添加方法，用 setUpClass() 与 tearDownClass()。

mock
--------
mock是辅助单元测试的一个模块。它允许您用模拟对象替换您的系统的部分，使用场景

- 测试一个请求了3rd api的类时，用request_mock模拟3rd api的响应。《python微服务开发》p55

模拟的实现方式：替换类方法。

参考链接：
- https://www.cnblogs.com/fnng/p/5648247.html
- https://segmentfault.com/a/1190000002965620#articleHeader8