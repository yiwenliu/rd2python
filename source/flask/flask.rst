(Flask)
============
flask tutorial
-----------------
http://docs.jinkan.org/docs/flask/api.html#

flask返回json串的两种方式
-------------------------------
当要给client返回一个json串时，有两个选择，return(jsonify())和return(json.dumps())它们的区别是Content-Type，前者是application/json，后者是text/html

获取请求参数
---------------
- request.form.get("key", type=str, default=None) 获取表单数据，
- request.args.get("key") 获取get请求参数，
- request.values.get("key") 获取所有参数。推荐使用request.values.get().

常用的请求content-type和对应的flask解析方法
----------------------------------------------
flask.Request的官方解释
^^^^^^^^^^^^^^^^^^^^^^^^^^^
http://docs.jinkan.org/docs/flask/api.html#id4

application/x-www-form-urlencoded
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1）浏览器的原生form表单
2） 提交的数据按照 key1=val1&key2=val2 的方式进行编码，key和val都进行了URL转码

- request.form.get("key", type=str, default=None) 获取表单数据，

multipart/form-data
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
常见的 POST 数据提交的方式,我们使用表单上传文件时，

application/json
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
消息主体是序列化后的 JSON 字符串,可以方便的提交复杂的结构化数据，特别适合 RESTful 的接口。

如果请求的json中包含汉字，见下表格

+--------------------+----------------------------------------------------------+
| request.data       | b"{'from':'zh','to':'en','src_text':'\xe4\xbb\x8a\xe5'}" |
+--------------------+----------------------------------------------------------+
| request.get_json() | Failed to decode JSON object                             |
+--------------------+----------------------------------------------------------+

要想正确的把bytes变成dict，见如下代码：

.. code-block:: none
	:linenos:

	import ast
	r = request.data.decode('utf-8')
	#不能用json.loads(r)，这是json module的bug
	args = ast.literal_eval(r)