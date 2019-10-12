在Flask中调用微服务
================================
场景
-----
Flask顶在最前面，对client的api请求，转发到不同的微服务。

使用Request
-----------------
传统做法
^^^^^^^^^^^^^
在Flask的“视图”中，调用request.get()或者request.post()

推荐做法
^^^^^^^^^^^^^
《python微服务开发》5.1.1，p106，使用request.session

1、创建一个session，书中称为“连接器”，保存一些公共请求头信息。当每次与其他服务交互时会重用session对象。
2、对于不同的主机（url)，定义不同的“传输适配器”，requests.adapter.HTTPAdapter（class定义见p105），定义“请求挂起时间”和“最大重连次数”，连接池参数（p107）。