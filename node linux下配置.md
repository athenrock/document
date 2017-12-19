服务器环境 centos7

开始没联网，所以自己再开发机上下载了nodejs安装包[Node.js安装包及源码下载地址](https://nodejs.org/en/download/。 )，因为我参照的是linux下面需要编译版的，所以下载node-vx.x.x-linux-x64.tar.gz

这些安装步骤网上教程很多，就不多说了，现在主要记录安装过程中出现的一些错误


1. 没联网啊，没联网，公司的网络需要验证才能登录，结果被我忽略了，结果npm install都无法继续，还查了半天

2. 连上网后，开始安装第一个碰到的错误：
	
	> electron@1.7.9 postinstall /data/www/proapi/node_modules/electron
	> node install.js
	
	/data/www/topshopcnapi/node_modules/electron/install.js:48
	  throw err
	  ^
	
	Error: connect ETIMEDOUT 54.231.120.107:443
	    at Object._errnoException (util.js:1024:11)
	    at _exceptionWithHostPort (util.js:1046:20)
	    at TCPConnectWrap.afterConnect [as oncomplete] (net.js:1182:14)
	npm WARN topshopcnapi@1.0.0 No description
	npm WARN topshopcnapi@1.0.0 No repository field.
	
	npm ERR! code ELIFECYCLE
	npm ERR! errno 1
	npm ERR! electron@1.7.9 postinstall: `node install.js`
	npm ERR! Exit status 1
	npm ERR! 
	npm ERR! Failed at the electron@1.7.9 postinstall script.
	npm ERR! This is probably not a problem with npm. There is likely additional logging output above.
	
	npm ERR! A complete log of this run can be found in:
	npm ERR!     /root/.npm/_logs/2017-12-08T01_38_42_713Z-debug.log


我添加了淘宝的[镜像](https://npm.taobao.org/)搞定的，具体就是执行了：	
	
	$ npm install -g cnpm --registry=https://registry.npm.taobao.org



2. 这个错误

	npm ERR! path /data/www/proapi/node_modules/home-path
	npm ERR! code ENOENT
	npm ERR! errno -2
	npm ERR! syscall access
	npm ERR! enoent ENOENT: no such file or directory, access '/data/www/proapi/node_modules/home-path'
	npm ERR! enoent This is related to npm not being able to find a file.
	npm ERR! enoent 
	
	npm ERR! A complete log of this run can be found in:
	npm ERR!     /root/.npm/_logs/2017-12-08T02_30_20_082Z-debug.log

没有权限 yong chomd 777 * 解决了
