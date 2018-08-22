python的初始化
================

python的初始化是什么时候发生的
------------------------------
1. 在命令行输入python，进入>>>提示符，等待用户输入python命令时
2. 运行$python xxx.py，而在xxx.py的第一条语句真正执行前

python初始化到底干了些什么
--------------------------
1. 构建1个PyInterpreterState object
2. 构建1个PyThreadState object
3. 构建3个PyModuleObject："__builtin__", "__main__", "sys"。这三个字符串即是module object本身的module name（参看PyModule_New()的实现，p323），也是PyInterpreterState->modules所指向的Dict的key
4. 构建1个PyFramObject，即，python虚拟机的执行环境，其中f_builtins, f_globals和f_locals三个namespace并非new(dict)，而是都指向了"__builtin__"或者"__main__"模块中的Dict对象。这种指向关系意味着，在一个code block执行过程中，如果修改了f_builtins, f_globals或者f_locals，其实是修改了PyInterpreterState->modules，进而有可能影响其他code block的执行环境的构建。
5. 激活python虚拟机