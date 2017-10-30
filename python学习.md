
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




#### python 数组合并

python3.0 中数据合并只能是  a+b 
extend 与append 合并后都是None


E:\workspace\python\planApi\views\index.html
E:\\workspace\\python\\planApi\\views


#### PyCharm 不能识别和补全Tornado模板语言

	每个项目下面会生成一个.idea/xxxx.iml的文件 其中有一段代码
	
	<component name="TemplatesService">
	<option name="TEMPLATE_CONFIGURATION" value="Jinja2" />
    <option name="TEMPLATE_FOLDERS">
      <list>
        <option value="$MODULE_DIR$/views" />
      </list>
    </option>
  </component>
	
	就因为 <option name="TEMPLATE_CONFIGURATION" value="Jinja2" />这句话在开始的时候没有自动不全，不知道为什么


