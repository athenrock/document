
#### 关于Tornado 中 HTTPServer 参数 xheaders的说明

Tornado获取客户端IP
tornado.httpserver.HTTPServer(Application(), xheaders=True)
是否通过self.request.remote_ip取到远端IP地址，否则只能是127.0.0.1



#### python 处理中文
在dict中包含中文时

    s= {"name":"测试商品，请勿购买"}
    print ujson.dumps(s).decode("unicode-escape")
    print ujson.dumps(s).decode("utf-8")
print
显示

	{"name":"测试商品，请勿购买"}
	{"a":"\u6d4b\u8bd5\u5546\u54c1\uff0c\u8bf7\u52ff\u8d2d\u4e70"}


可以参考http://blog.csdn.net/liuxincumt/article/details/8183391