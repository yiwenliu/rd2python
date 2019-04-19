json串的loads&dumps
=======================
经常碰到json串和dict互相转化的场景，总结如下。

json串转dict
----------------
flask服务器收到application/json的请求，要解析post参数，并且其中含有汉字。

.. code-block:: none
	:linenos:

	import ast
	from flask import request
	r = request.data.decode('utf-8')
	#不能用json.loads(r)，有bug
	args = ast.literal_eval(r)

dict转json串
----------------
当dict中包含汉字时，在dumps函数中要改变ensure_ascii的值

.. code-block:: none
	:linenos:

	json.dumps(js,ensure_ascii=False)