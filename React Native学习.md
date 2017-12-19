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



### 尝试运行

启动Genymotion，打开虚拟机。在Demo外面执行命令

	react-native init demo  
	
	注: demo是我的项目名称
	进demo目录，执行命令
	
		react-native run-android	
	

哦 NO！出错了，还是没有运行起来

	FAILURE: Build failed with an exception.

	* What went wrong:
	Unable to start the daemon process.
	This problem might be caused by incorrect configuration of the daemon.
	For example, an unrecognized jvm option is used.
	Please refer to the user guide chapter on the daemon at https://docs.gradle.org/2.14.1/userguide/gradle_daemon.html
	Please read the following process output to find out more:
	-----------------------
	Error occurred during initialization of VM
	Could not reserve enough space for object heap

	* Try:
	Run with --stacktrace option to get the stack trace. Run with --info or --debug option to get more log output.
	Could not install the app on the device, read the error above for details.
	Make sure you have an Android emulator running or a device connected and have
	set up your Android development environment:
	https://facebook.github.io/react-native/docs/android-setup.html



看样是gradle出问题了，可能是以为以前我机器上有个Gradle的东西，网上找了找也是建议干掉重新生成，找不到的同学用命令%USERPROFILE%，在这里面找一下
找到后干掉(先别傻乎乎的干掉，重命名一下)，干掉后，运行下面的命令	
	
	(if not exist "%USERPROFILE%/.gradle" mkdir "%USERPROFILE%/.gradle") && (echo org.gradle.daemon=true >> "%USERPROFILE%/.gradle/gradle.properties")
	
看了一下，里面生成了一个文件gradle.properties，就一句话
	
	org.gradle.daemon=true 	
	
	
继续刚才的尝试，在dome下运行 react-native run-android ,好了恼人的下载又开始了。
三十分钟后，尝试，再次失败，囧！！！

	FAILURE: Build failed with an exception.

	* Where:
	Build file 'E:\workspace\react\demo\android\app\build.gradle' line: 1

	* What went wrong:
	A problem occurred evaluating project ':app'.
	> java.lang.UnsupportedClassVersionError: com/android/build/gradle/AppPlugin : Unsupported major.minor version 52.0

	* Try:
	Run with --stacktrace option to get the stack trace. Run with --info or --debug option to get more log output.

	BUILD FAILED

	Total time: 6.951 secs
	Could not install the app on the device, read the error above for details.
	Make sure you have an Android emulator running or a device connected and have
	set up your Android development environment:
	https://facebook.github.io/react-native/docs/android-setup.html



一般出现**Unsupported major.minor version 52.0**这个问题的，都是因为JDK包版本的问题，查了一下环境变量，果然指的是JDK7，换成JDK8，再尝试


HOHO！！！成功了

囧，我新建的项目又不好使了，还是报**Unsupported major.minor version 52.0**的错误，我是各种修改各种查询，最后把环境变量里面的gradle干掉了，大兄弟终于报别的错误了
	
		
	* What went wrong:
	Execution failed for task ':app:mergeDebugResources'.
	> Error: java.util.concurrent.ExecutionException: java.io.IOException: 管道正在被关闭。

	* Try:
	Run with --stacktrace option to get the stack trace. Run with --info or --debug option to get more log output.

无语问苍天，我重启了机器，歪打正着，好了

### 模拟器运行问题

运行起来后，出现错误

<img src="https://github.com/athenrock/document/blob/master/images/react/err1.png" height="50%" width="50%" />
	
	React Native unable to load script from assets 'index.android.bundle'.Make sure your bundle is packaged correctly or you're running a package server
	
网上搜了一下，如下解决
1. 在工程目录冲创建assets文件 mkdir
	
	android/app/src/main/assets

2. 运行命令	

	react-native bundle --platform android --dev false --entry-file index.android.js --bundle-output android/app/src/main/assets/index.android.bundle --assets-dest android/app/src/main/res

3. 继续启动

	react-native run-android
	
	
OK! 启动成功
	
### react-native 的web尝试

[React Native 项目运行在 Web 浏览器上面 ](https://www.cnblogs.com/On1Key/p/5780577.html)
[react-web](https://github.com/taobaofed/react-web)

[react-native-web](https://www.npmjs.com/package/react-native-web)
[react-native-web](https://github.com/necolas/react-native-web)


1. 首先试用 react-web ,按照One1Key的blog配置，然后运行后出错



	ERROR in ./index.web.js
	Module not found: Error: Cannot resolve module 'react/lib/ReactMount' in E:\workspace\react\webdemo
	@ ./index.web.js 1:299-330
	
网上找的是因为“react/lib/ReactMount”在react@15.4.0版本移除。如果你还需要继续使用react高版本，可以在webpack配置alias解决这个问题

	alias: {
		'react/lib/ReactMount': 'react-dom/lib/ReactMount'
	}
	
各种出错。遂放弃。。。

2. 用react-native-web ,按照说明文档，进行配置，运行，ok没问题！！	
	




参考资料: [React Native官网入门文档](http://reactnative.cn/docs/0.50/getting-started.html)
