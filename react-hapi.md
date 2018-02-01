按照别人的搭建一个react-hapi架构

前面的详细说
1. 创建项目

	npm init
	
2. 创建几个文件夹后项目架构大概是这个样子
	
	root         //根目录
	--clinet     //客户端
	--configs    //配置文件	
	--server	 //服务器端
	--view		 //资源
	package.json

3. 首先在configs文件夹里面创建 webpack.dev.config.js 用来构建启动项

```javascript
	//__dirname是node.js中的一个全局变量，它指向当前执行脚本所在的目录
		module.exports = {//注意这里是exports不是export
			entry: __dirname + "/clinet/main.js",//唯一入口文件，就像Java中的main方法
			output: {   //输出目录
				path:path.resolve(__dirname,"../dist"),
				filename: "[name].js",
				chunkFilename: 'chunk.[name].js',
			}
		};

```

4. 安装插件 ,默认已经安装完node，npm，webpack
	
	1. 在本项目中安装webpack
		npm install --save-dev webpack
	2.安装react	
		npm install --save-dev react react-dom
	3.安装Babel
		npm install --save-dev babel-core babel-loader babel-preset-es2015 babel-preset-react babel-plugin-import
		使用Babel时，config里面配置需要写成'babel-loader',否则报错
		使用.babelrc文件时，如果没有babel-plugin-import ，则会报错
5. 在package.json 配置启动项

```
"scripts": {
    "start": "webpack --config ./configs/webpack.dev.config.js",
    "test": "echo \"Error: no test specified\" && exit 1"    
  },

```
这时候npm start ,如果没有错误，则会在项目根目录下生产一个dist的文件夹，里面有打包后的main.js的文件

6. 在client 文件夹下创建components->home.js

```javascript
import React, {Component} from 'react';

export default class home extends Component{
    render(){
        return <div>Hello world!</div>
    }
}
```
修改main.js
```javascript
import React, {Component} from 'react';
import ReactDOM from 'react-dom';

import Home from './components/home'

ReactDOM.render(
    <Home />,
    document.getElementById('content')
);
```

7. 在view文件夹下创建启动html

```html
<!DOCTYPE <!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <title>Page Title</title>
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <link rel="stylesheet" type="text/css" media="screen" href="../dist/main.css" />    
</head>
<body>
    <div id="content"></div>
    <script src="../dist/main.js"></script>
</body>
</html>

```

到此，如果用webstorm的话，浏览view下面的index.html,如果不报错的话，应该会出现hello world的界面
 
 
 
8. 在配置Module的时候，碰到less里面@import 的时候"../"报错

	这个是babel-loader的问题
	
	
###使用的一些脚手架

1.extract-text-webpack-plugin 主要是用来打包Css的

2.css拆包 暂时未实现

3.webpack-dev-server

4.css-modules-require-hook 还是样式加载的问题，less文件不能识别

5. npm install --save-dev babel-polyfill   解决ie9和一些低版本的高级浏览器对es6新语法不支持

6. asset-require-hook  对图片，静态资源的一些支持
7  npm install --save--dev babel-plugin-add-module-exports 对于 export default {} 支持不好，还得加个插件 



9.这个文章帮大忙了

https://cnodejs.org/topic/5660f8f9d0bc14ae27939b37

	

### 出现<!-- react-empty: 1 --> 的问题

### The root route must render a single element
网上说是因为es6 和CommonJs的书写规则的问题，es6必须是exprot default ,需要加.default 按照这个方式不行
对于 export default {} 支持不好，还得加个插件 babel-plugin-add-module-exports
```
use: {
	loader: "babel-loader?presets[]=react,presets[]=es2015,presets[]=stage-0",
	query:{
		plugins: ['transform-runtime', 'add-module-exports']
	}
},
```

	

