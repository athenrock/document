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

selected方式实现

```javascript 
selected=(e)=>{
	//判断如果是<a>标签，则返回
	if(e.target&&e.target.matches('a')){
            return;
      }
}
```

###3. 路由  & 子路由
	

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


###4. 页面跳转及传值 
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



