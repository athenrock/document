最近的新目标
公司要做新项目，选型用Recat Native，所以学习一下


首先，我是window系统，这就导致我很多地方有先天不足，好在网上已经有很多趟过坑的大牛们分享的经验。再次表示万分感谢。


我因为以前做.net，所以上手就先选型IDE，我装了Vs.code 和WebStorm，最后使用还是主要用webStorm。

## 环境安装

之前因为研究过一段时间NodeJs，机器上有个比较低版本的node，需要升级。用npm安装 <code>npm install -g n</code>结果老出现下面的错误

	...
	error Windows_NT 6.1.7601
	error argv "E:\\0Develop\\nodejs\\nodejs\\node.exe" "E:\\0Develop\\nodejs\\nodejs\\node_modules\\npm\\bin\\npm-cli.js" "install" "-g" "n"
	error node v4.2.3
	error npm  v2.14.7
	error code EBADPLATFORM
	error notsup Unsupported
	error notsup Not compatible with your operating system or architecture: n@2.1.4
	error notsup Valid OS:    !win32
	error notsup Valid Arch:  any
	error notsup Actual OS:   win32
	error notsup Actual Arch: x64
	verbose exit [ 1, true ]
	
	
这是因为我在window下 ，而n模块是不支持的，绕不过去了，只能去[node官网](https://nodejs.org/en/) 下载exe文件进行安装，好在顺利安装成功了。