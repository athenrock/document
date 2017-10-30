
#### 关于Tornado 中 HTTPServer 参数 xheaders的说明

Tornado获取客户端IP
tornado.httpserver.HTTPServer(Application(), xheaders=True)
是否通过self.request.remote_ip取到远端IP地址，否则只能是127.0.0.1
