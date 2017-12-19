React开发过程中碰到的问题

1. 控制兄弟元素
	
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
	

2. 阻止事件冒泡
	
网上有很多解决办法，可是都没有解决，我最后自己这么解决

我的结构

```html
<ul><li onClick={this.selected}><span>abcde</span><a>delete</></li></ul>
```

selected方式实现

```js
selected=(e)=>{
	//判断如果是<a>标签，则返回
	if(e.target&&e.target.matches('a')){
            return;
      }
}
```
