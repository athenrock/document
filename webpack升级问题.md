我的webpack需要从1.13.x的版本升级到3.11.0 中间遇到的问题进行记录




1. postcss 错误

    Invalid configuration object. Webpack has been initialised using a configuration object that does not match the API schema.
    - configuration[0] has an unknown property 'postcss'. These properties are valid:
   object { amd?, bail?, cache?, context?, dependencies?, devServer?, devtool?, entry, externals?, loader?, module?, name?, node?, output?, parallelism?, performance?, plugins?, profile?, recordsInputPath?, recordsOutputPath?, recordsPath?, resolve?, resolveLoader?, stats?, target?, watch?, watchOptions? }
 - configuration[0].resolve.extensions[0] should not be empty.
   -> A non-empty string
 - configuration[1].resolve.extensions[0] should not be empty.
   -> A non-empty string


把webpack.config.js文件中`postcss: [autoprefixer({browsers: ['> 5%']})],` 先注释掉


2. resolve 默认错误

    Invalid configuration object. Webpack has been initialised using a configuration object that does not match the API schema.
 - configuration[0].resolve.extensions[0] should not be empty.
   -> A non-empty string
 - configuration[1].resolve.extensions[0] should not be empty.
   -> A non-empty string

这个问题是因为resolve不能再匹配`''`了需要修改 

    //修改第一个参数
    //resolve: {extensions: ['', '.js', '.json','.less', '.scss']},
    resolve: {extensions: ['*', '.js', '.json','.less', '.scss']},


3. Chunk.entry was removed

    Error: Chunk.entry was removed. Use hasRuntime()


这个是因为`extract-text-webpack-plugin`这个组件的版本与`webpack`的不一致，进行升级

    npm remove extract-text-webpack-plugin
    npm install --save--dev extract-text-webpack-plugin@3

用这两步执行不会出现灵异事件。。。


4. Babel报错`'-loader'`

    ERROR in Entry module not found: Error: Can't resolve 'babel' in 'E:\workspace\react\koa-topshop'
    BREAKING CHANGE: It's no longer allowed to omit the '-loader' suffix when using loaders.
                     You need to specify 'babel-loader' instead of 'babel',
                     see https://webpack.js.org/guides/migrating/#automatic-loader-module-name-extension-removed


把config文件中的所有babel相关文件都加上`'-loader'`


到此，所有的error都修改完了，build也成功了，`npm start`一下程序跑起来了，Listening on port 8080. Open up http://localhost:8080/ in your browser.消息也出来了，在浏览器中访问http://localhost:8080/ 。。。。。。。页面显示*Not Found* 什么鬼，我做了什么！！！


一天过去了。。。两天过去了。。。接近崩溃的时候，我突然发现一个东西

在react-router加载组件的时候是按需加载的，webpack@1.0在打包的时候打出来的包大概是这个样子

```
	var home = function home(nextState, callback) {
	    __webpack_require__.e/* nsure */(1, function (require) {
	        callback(null, __webpack_require__(98));
	    });
	};
```

而webpack@3.0打包出来的是这个样子
```
var Home = function Home(nextState, callback) {
    new Promise(function(resolve) { resolve(); }).then((function (require) {
        callback(null, __webpack_require__(157));
    }).bind(null, __webpack_require__)).catch(__webpack_require__.oe);
};
```
所以怎么都是*Not Found* ，我把按需加载先取掉，解决了


