###1. 控制兄弟元素
	
用全局的state属性解决，个人认为不是最优解决方式

```javascript 
componentWillMount(){
	this.setState({isDefault:1});
}	
change=(key)=>{
	this.setState({isDefault:key});
}	
render(){
	let that = this;
	let item = this.state.data.map((n,m)=>{
		let color = this.state.isDefault ===n.id ? "red" :"blue"
		return <Button style="color:{color}" key={m} onClick={e=>that.change(n.id)}>{n.name}</Button>
	});
	return <div>{item}</div>
	}

```

这样就实现了控制兄弟结点的行为
	

###2. 阻止事件冒泡
	
网上有很多解决办法，可是都没有解决，我最后自己这么解决

我的结构

``` html
<ul><li onClick={this.selected}><span>abcde</span><a>delete</></li></ul>
```

selected方式实现:一

```javascript 
selected=(e)=>{
	//判断如果是<a>标签，则返回
	if(e.target&&e.target.matches('a')){
            return;
      }
}
```
selected方式实现:二
```javascript 
selected=(e)=>{
	 e.preventDefault();
        //TODO:继续逻辑
}
```

###3. 路由  
	
>1.子路由  这个没有研究完，只是记录一下

```javascript 
const routes = {
    path: '/',
    component: App,
    indexRoute: {
        getComponent: Home,
    },
    childRoutes: [
        {
            path: 'list',
            getComponent: List,
        },
        {
            path: 'address',
	        indexRoute: {
                getComponent: Address,
	            },
	            childRoutes: [  //子路由可以这么配置
	                {
	                    path: 'edit',
	                    getComponent: EditAddress,
	                },
	                {
	                    path: 'add',
	                    getComponent: EditAddress,
	                }
	            ]
            }
        }
     ]
   }
```


###4. 页面跳转时传值 
使用react-router 中的Link传值
>1. props.params

```html
<Link to="/edit/1">编辑</Link>
```

```javascript
<Route path='/edit/:id' component={EidtPage}></Route>
  
class UserPage extends React.Component{
     constructor(props){
        super(props);
        console.log(this.props.params.name)
    }
}

```

>2. query

>3. state


```jsx
 <Link to={{ pathname: '/edit', state: { id: id }}}  >编辑</Link>
```
```javascript
var data = this.props.location.state;
var {id,name,age} = data;
```

查看了一下Link的源码支持者几个属性
```javascript
    propTypes: {
      to: oneOfType([string, object]),
      query: object,
      hash: string,
      state: object,
      activeStyle: object,
      activeClassName: string,
      onlyActiveOnIndex: bool.isRequired,
      onClick: func,
      target: string
  }
```
觉得不暴露参数前提下的只能放在state中


###5.页面中的控件问题
1. 生成的input控件无法输入

调查了半天，发现是因为初始化的时候，我给input控件的value进行的赋值，所以导致，input控件无法输入或进行修改





### 6. react 中inject 是什么意思
代码实例

```javascript
	<a className={inject(styles.closeButton)} onClick={onClose}>
              <CloseCircle diameter={40}/>
            </a>
```


### 7.react 判断进入不同子组件



### 8.三级联动

https://github.com/nickeljew/react-picker
https://github.com/petitspois/react-picker.git

https://ant.design/components/cascader-cn/



### 9.proptypes 
	
具体就是组件传值时，参数类型校验，参见React proptypes


### 10. 滚动条（scrollbar） 定位问题
具体场景，省市区三级联动时，选择靠下的选项时，组件再次打开，需要定位到选项那个位置	

具体参照下面代码：


```javascript
 componentDidUpdate(){
	let pickerMenu = document.querySelectorAll('.picker-menu');
	pickerMenu.forEach(function (item,k) {
		let scrollTop = item.querySelector(".selected").offsetTop || 0;
		if(item.scrollTop === 0){  //防止实时跳动
			item.scrollTop = scrollTop< 160 ? 0 : scrollTop - 40 ;
		}
	})
};

```

注意，是在*componentDidUpdate*接口里面写的


### 11.react操作元素

方法一: 使用ref，

```javascript
	selected=()=>{
	
		this.refs.divBox.innHtml="换一句话"+'<a onClick={this.selected}>换一下</a>'
	}

	create=()=>{
		return <div ref="divBox">这是一个div。<a onClick={this.selected}>换一下</a></div>
	}

```
方法二: 使用 querySelector、querySelectorAll 具体参考第10 




### 12 webpack dev 连通https





### 13 React 服务器渲染

[教你如何搭建一个超完美的服务端渲染开发环境](https://www.jianshu.com/p/0ecd727107bb)
[基于 React 的通用框架 Next.js：服务端 React]
	[原文:](https://scotch.io/tutorials/react-universal-with-next-js-server-side-react)
	[翻译:](http://www.zcfy.cc/article/react-universal-with-next-js-server-side-react-2158.html)
 




 
