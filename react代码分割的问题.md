react-router分隔 代码是个问题，

方法一：使用*bundle-loader*,router代码如下

```javascript
import homeContainer from 'bundle-loader?lazy&name=Home!../page/Home'
import helloContainer from 'bundle-loader?lazy&name=Hello!../page/Hello'

const Home = (props) => (
    <Bundle load={homeContainer}>       
      {(Container) => <Container {...props}/>}
    </Bundle>
  )

const Hello = (props) => (
<Bundle load={helloContainer}>    
    {(Container) => <Container {...props}/>}
</Bundle>
)

  const routes = {
    childRoutes: [{
        path: '/',
        component: require('../page'),
        indexRoute: {
           component:Home
           //getComponent:Home   
        },
        childRoutes:[
            {
                path:'/hello',
                component:Hello
            }
        ]
    }]
}
``` 


Bundle.js

```javascript
import React from 'react';
import PropTypes from 'prop-types';

class Bundle extends React.Component {
  state = {
    // short for "module" but that's a keyword in js, so "mod"
    mod: null
  }

  componentWillMount() {
    // 加载初始状态
    this.load(this.props);
  }

  componentWillReceiveProps(nextProps) {
    if (nextProps.load !== this.props.load) {
      this.load(nextProps);
    }
  }
  load = props => {
        this.setState({
            mod: null
        });
    props.load(mod => {
        this.setState({
            mod: mod ? mod : null
        });
    });
    }
    
  render() {
    // if state mode not undefined,The container will render children
    return this.state.mod ? this.props.children(this.state.mod) : null;
  }
}

Bundle.propTypes = {
  load: PropTypes.func,
  children: PropTypes.func
};

export default Bundle;
```

getComponent这个API在react-router3以后被废弃了，所以就不用了

当然bundle-loader可以在webpack.config.js中进行配置，就不用在代码中这么写了
用bundle-loader，这么做，我build以后，没有问题，但是用webpack-dev环境下，是有兼容问题的，所以我没采用这个


2.使用import()代替bundle-loader

router.js
```javascript
const Home = lazyLoad(() => import(/* webpackChunkName: "Home" */'../page/Home'));
const Hello = lazyLoad(() => import(/* webpackChunkName: "Test" */'../page/Hello'));

const routes = {
    childRoutes: [{
        path: '/',
        component: require('../page'),
        indexRoute: {
           component:Home
           //getComponent:Home   
        },
        childRoutes:[
            {
                path:'/hello',
                component:Hello
            }
        ]
    }]
}
```
lazyLoad.js

```javascript
import React from 'react';
    import Bundle from './Bundle';

    // 默认加载组件，可以直接返回 null 
    const Loading = () => <div>Loading...</div>;

    /*
       包装方法，第一次调用后会返回一个组件（函数式组件）
       由于要将其作为路由下的组件，所以需要将 props 传入
    */ 
    const lazyLoad = loadComponent => props => (
       <Bundle load={loadComponent}>
          {Comp => (Comp ? <Comp {...props} /> : <Loading />)}
       </Bundle>
    );

    export default lazyLoad;

```
因为import()返回的是一个Promise对象，所以Bundle组件的代码得改成异步的

```javascript
import React from 'react';
import PropTypes from 'prop-types';

class Bundle extends React.Component {
  state = {
    // short for "module" but that's a keyword in js, so "mod"
    mod: null
  }

  componentWillMount() {
    // 加载初始状态
    this.load(this.props);
  }

  componentWillReceiveProps(nextProps) {
    if (nextProps.load !== this.props.load) {
      this.load(nextProps);
    }
  }
  
 // 更改 load 方法为异步函数
  async load(props) {
    // 重置状态
    this.setState({
      mod: null
    });
    const mod = await props.load();
    this.setState({
        mod: mod.default ? mod.default : mod
     }); 
  }

  render() {
    // if state mode not undefined,The container will render children
    return this.state.mod ? this.props.children(this.state.mod) : null;
  }
}

Bundle.propTypes = {
  load: PropTypes.func,
  children: PropTypes.func
};

export default Bundle;
```

但是用这个import()很多环境支持不了，没办法还得寻求其他更兼容的办法

3.`require.ensure`又被找了回来，虽然对这个东西的使用一直比较隐晦，但是它确实能用

于是代码又改回来了
```javascript
import React from 'react';
import Bundle from './Bundle'

if (typeof require.ensure !== 'function') {
    require.ensure = function(dependencies, callback) {
        callback(require)
    }
}

const Home = (props) => (
    <Bundle load={(cb) => { 
        require.ensure([], require => { 
            cb(require('../page/Home')); 
        },'Home'); 
        }}> 
        {(Chat) => <Chat {...props}/>} 
    </Bundle>
);
const Hello = (props) => ( 
    <Bundle load={(cb) => { 
        require.ensure([], require => { 
                cb(require('../page/Hello')); },'Hello'); 
        }}> 
        {(Chat) => <Chat {...props}/>} 
    </Bundle> );
 const routes = {
    childRoutes: [{
        path: '/',
        component: require('../page'),
        indexRoute: {
           component:Home
           //getComponent:Home
        },
        childRoutes:[
            {
                path:'/hello',
                component:Hello
            }
        ]
    }]
}

export default routes;

```

这中间Bundle不用在使用异步的，所以组件改会方法一里面的


这个写有点复杂，既然方法二里面都有lazyload了，就用起来,于是代码就改为

```javascript
import React from 'react';
import lazyLoad from './lazyLoad';

if (typeof require.ensure !== 'function') {
    require.ensure = function(dependencies, callback) {
        callback(require)
    }
}
if (typeof require.ensure !== 'function') {
    require.ensure = function(dependencies, callback) {
        callback(require)
    }
}

const Home = lazyLoad((cb) => { 
            require.ensure([], require => { 
                cb(require('../page/Home')); 
            },'Home'); 
});
const Hello = lazyLoad((cb) => { 
    require.ensure([], require => { 
        cb(require('../page/Hello')); 
    },'Hello'); 
});


 const routes = {
    childRoutes: [{
        path: '/',
        component: require('../page'),
        indexRoute: {
           component:Home
           //getComponent:Home
        },
        childRoutes:[
            {
                path:'/hello',
                component:Hello
            }
        ]
    }]
}

export default routes;
```
这样就好写多了，而且好消息是，在我现在的架构(react@15+webpack@3+react-router@3)下，它全兼容。。。

