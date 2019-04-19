(Flask)
============
jsonify和json.dumps的区别
-------------------------------
当要给client返回一个json串时，有两个选择，它们的区别是Content-Type，前者是application/json，后者是text/html

获取请求参数
---------------
- request.form.get("key", type=str, default=None) 获取表单数据，
- request.args.get("key") 获取get请求参数，
- request.values.get("key") 获取所有参数。推荐使用request.values.get().
