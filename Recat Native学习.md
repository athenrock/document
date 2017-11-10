最近的新目标
公司要做新项目，选型用React Native，所以学习一下


首先，我是window系统，这就导致我很多地方有先天不足，好在网上已经有很多趟过坑的大牛们分享的经验。再次表示万分感谢。
再次我开始学习的时候，这个文件命名是recat学习，在我的臆断里，recat和recat-native一个是一个的继承，其实错了，两个是不同概念

	React是一种思想，Facebook对于Web Components的理解与实现。其理念是“Learn once, write anywhere”。
	而React native 是用React的方式开发mobileApp。开始主要是为了IOS端，还有英ReactJS是web端的
	



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

2. 安装React Native

	去Facebook的 [github](https://github.com/facebook/react-native.git) 上下载 ,下载完后进入react-native目录下的react-native-cli进行安装
	
		npm install -g
		
	结果出错了，在这块其实我想说，用git的cmd(Terminal) 比window自带的cmd好用，指哪打哪	
	
	npm WARN E:\workspace\react\react-native\react-native-cli\node_modules\ansi-regex is not a child of C:\Users\Administrator\AppData\Roaming\npm
	npm ERR! path E:\workspace\react\react-native\react-native-cli\node_modules\ansi-regex
	npm ERR! code ENOENT
	npm ERR! errno -4058
	npm ERR! syscall rename
	npm ERR! enoent ENOENT: no such file or directory, rename 'E:\workspace\react\react-native\react-native-cli\node_modules\ansi-regex' -> 'E:\workspace\react\react-native\react-native-cli\node_modules\.ansi-regex.DELETE'npm ERR! enoent This is related to npm not being able to find a file.
	npm ERR! enoent

	这是神马情况，一脸懵逼网上搜了搜，有人说的rebuild一下，于是用命令：
		
		npm rebuild ansi-regex
	
	姑且试试.....一分钟后，不好使 这时候我才发现应该是我没有装ansi-regex，面壁三分钟
	
		npm install ansi-regex
		
	然后剩下的就简单了，少谁装谁
		
		
	

3. 安装Android Studio  (顺便提一句，我一开始没有装这个)
	
	从 [官网](http://developer.android.com/sdk/index.html) 或 [平台](http://www.android-studio.org/)上下载安装包安装，没有啥说的
	
	一路next ，除了要选择Type的时候需要选择一下，两个选项“Standard”和“Custom”，即标准和自定义，如果你本机的Android SDK没有配置过，那么建议直接选择“Standard”, 点击“Finish”按钮

因为我本地已经下载SDK并配置好了环境变量，所以我选择”Custom”，然后继续next。
	
然后就是无休止的等待，各种下包，**而且需要翻墙**，这也是我一开始不愿意直接装Android Studio的原因，最后各种坑，再加上windos这方面先天不足，只能忍着下载安装
	
	
	
4. IDE选择

我因为以前做.net，所以上手就先选型IDE，我装了Vs.code 和WebStorm，最后使用还是主要用webStorm。


5. 用Yarn、React Native的命令行工具（react-native-cli） 替代 npm
	
	作为一个window开发者最头痛的就是，这些工具，什么apt-get、yun、npm、pip，现在又出来了一个yarn，为啥就不能用一个
	
		npm install -g yarn react-native-cli
		
	安装完yarn后同理也要设置镜像源：
		
		yarn config set registry https://registry.npm.taobao.org --global
		yarn config set disturl https://npm.taobao.org/dist --global
		
	

6. 安装模拟器，官网推荐 Genymotion，以下是官方文档的描述
	
	>1.下载和安装Genymotion（genymotion需要依赖VirtualBox虚拟机，下载选项中提供了包含VirtualBox和不包含的选项，请按需选择）。
	>2. 打开Genymotion。如果你还没有安装VirtualBox，则此时会提示你安装。
	>3. 创建一个新模拟器并启动。
	>4. 启动React Native应用后，可以按下F1来打开开发者菜单
	
	值得一提的是，Genymotion现在使用必须注册，所以不要花时间想着怎么绕过去了。同理，安装很费时间，需要翻墙，需要网速。。。。（怀念window程序下载安装包，无脑next）
	
	
## 开始尝试

#### 代码编写，及环境
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

	
但是安装的很慢很慢，我就自己用npm安装，结果出现如下错误
	
	npm ERR! path E:\workspace\react\demo\node_modules\fsevents\node_modules\string_decoder\lib\string_decoder.js
	npm ERR! code EPERM
	npm ERR! errno -4048
	npm ERR! syscall lstat
	npm ERR! Error: EPERM: operation not permitted, lstat 'E:\workspace\react\demo\node_modules\fsevents\node_modules\string_decoder\lib\string_decoder.js'
	npm ERR!  { Error: EPERM: operation not permitted, lstat 'E:\workspace\react\demo\node_modules\fsevents\node_modules\string_decoder\lib\string_decoder.js'
	npm ERR!   stack: 'Error: EPERM: operation not permitted, lstat \'E:\\workspace\\react\\demo\\node_modules\\fsevents\\node_modules\\string_decoder\\lib\\string_decoder.js\'',
	npm ERR!   errno: -4048,
	npm ERR!   code: 'EPERM',
	npm ERR!   syscall: 'lstat',
	npm ERR!   path: 'E:\\workspace\\react\\demo\\node_modules\\fsevents\\node_modules\\string_decoder\\lib\\string_decoder.js' }
	npm ERR!
	npm ERR! Please try running this command again as root/Administrator.

	npm ERR! A complete log of this run can be found in:
	npm ERR!     C:\Users\Administrator\AppData\Roaming\npm-cache\_logs\2017-11-08T09_39_43_055Z-debug.log

好吧没权限，cmd用管理员方式打开，然后进入demo文件夹继续执行**npm install react**，结果有报错误

	npm WARN saveError ENOENT: no such file or directory, open 'E:\workspace\react\demo\package.json'
	npm WARN enoent ENOENT: no such file or directory, open 'E:\workspace\react\demo\package.json'
	npm WARN react No description
	npm WARN react No repository field.
	npm WARN react No README data
	npm WARN react No license field.
	
	
很明显，没有找到package.json文件，手动创建一个吧。。。


继续执行，结果

	npm notice created a lockfile as package-lock.json. You should commit this file.

	npm WARN demo No description
	npm WARN demo No repository field.
	npm WARN demo No license field.

	
查看了一下package.json发现里面添加了很多东西，其中有compilerOptions、exclude、dependencies

	```
	{
	  "compilerOptions": {
		"module": "commonjs",
		"target": "es5",
		"sourceMap": true
	  },
	  "exclude": [
		"node_modules"
	  ],
	  "dependencies": {
		"react": "^16.0.0"
	  }
	}

	```

这个时候我判断不了react是否已经安装成功了，反正package里面有了，姑且算成了吧，继续安装react-native，方法类似
	安装的时候第一个错误

		npm WARN deprecated connect@2.30.2: connect 2.x series is deprecated
	
	更新 connect 

		npm update connect
		
	查看版本
		
		npm -v connect
		
	执行升级 @5 应该标示自己的npm版本，我的是5.5.1所以我用@5	
		npm install -g npm@5

	上面的方式不行。。。。。还是老老实实的用install吧

		npm install connect
	
升级成功了！！！！！！！！可惜安装react-native各种的缺包，然后各种的copy。。。别闹了,老老实实的装react-native吧。

回过头把东西都装的差不多了继续咱们的尝试，把以前无脑建的项目干掉，用WebStrom重新创建一个demo，这时候的再创建的时候react-native-cli自动被选中了，开始愉快的编程了。




启动Genymotion，在Demo外面执行命令

	react-native init demo  
	
	注: demo是我的项目名称





	



	

	


参考资料: [React Native官网入门文档](http://reactnative.cn/docs/0.50/getting-started.html)
