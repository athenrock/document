最近的新目标
公司要做新项目，选型用React Native，所以学习一下



首先，我是window系统，这就导致我很多地方有先天不足，好在网上已经有很多趟过坑的大牛们分享的经验。再次表示万分感谢。




## 环境安装


1. 安装npm 安装nodejs(准确说是升级node)

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

2. 安装Android Studio
	从 [官网](http://developer.android.com/sdk/index.html) 或 [平台](http://www.android-studio.org/)上下载安装包安装，没有啥说的


3. IDE选择

我因为以前做.net，所以上手就先选型IDE，我装了Vs.code 和WebStorm，最后使用还是主要用webStorm。


## 开始尝试

创建一个目录，创建一个app.js,写下面一段话（当然这是我copy别人的）

	```
	import React, { Component } from 'react';
	import { Text } from 'react-native';

	class HelloWorldApp extends Component {
	  render() {
		return (
		  <Text>Hello world!</Text>
		);
	  }
	}
	```

当输入完后，发现IDE提示 react和react-native都找不到。安装一下，我没有搜索命令，直接点击提示前面的"小灯泡"->**"install [name]"** 进行安装。

至于为啥不识别，我判断是因为我跳过了上面的**2步**，没有安装Android Studio。

	
	


参考资料: [React Native官网入门文档](http://reactnative.cn/docs/0.50/getting-started.html)