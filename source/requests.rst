Requests Module
==================
post表单
------------
.. code-block:: none
	:linenos:

	#复杂表单参数
	payload = {"paramJson": json.dumps({"docid": docid}),
	           "access_token":ACCESS_TOKEN}
	try:
	    #自动设置请求的content-type是application/x-www-form-urlencoded
	    r = requests.post(url, data=payload)
	except:
	    print("response error")