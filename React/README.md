# React

### react生命周期函数

一、初始化阶段：
 - getDefaultProps:获取实例的默认属性
 - getInitialState:获取每个实例的初始化状态
 - componentWillMount：组件即将被装载、渲染到页面上
 - render:组件在这里生成虚拟的DOM节点
 - componentDidMount:组件真正在被装载之后

二、运行中状态：
 - componentWillReceiveProps:组件将要接收到属性的时候调用
 - shouldComponentUpdate:组件接受到新属性或者新状态的时候（可以返回false，接收数据后不更新，阻止render调用，后面的函数不会被继续执行了）
 - componentWillUpdate:组件即将更新不能修改属性和状态
 - render:组件重新描绘
 - componentDidUpdate:组件已经更新

三、销毁阶段：
 - componentWillUnmount:组件即将销毁



### 当你调用setState的时候，发生了什么事？
当调用 setState 时，React会做的第一件事情是将传递给 setState 的对象合并到组件的当前状态。然后生成新的DOM树并和旧的DOM树对比，然后更新对应部分。



### 客户端渲染与服务端渲染
客户端渲染即普通的React项目渲染方式。
客户端渲染流程：
1. 浏览器发送请求
2. 服务器返回HTML
3. 浏览器发送bundle.js请求
4. 服务器返回bundle.js
5. 浏览器执行bundle.js中的React代码

CSR带来的问题：
1. 首屏加载时间过长
2. SEO 不友好

因为时间在往返的几次网络请求中就耽搁了，而且因为CSR返回到页面的HTML中没有内容，就只有一个root空元素，页面内容是靠js渲染出来的，爬虫在读取网页时就抓不到信息，所以SEO不友好

SSR带来的问题：
1. React代码在服务器端执行，很大的消耗了服务器的性能



### React 同构时页面加载流程
1. 服务端运行React代码渲染出HTML
2. 浏览器加载这个无交互的HTML代码
3. 浏览器接收到内容展示
4. 浏览器加载JS文件
5. JS中React代码在浏览器中重新执行



### react中key的作用
key是React中用于追踪哪些列表中元素被修改、删除或者被添加的辅助标识。在diff算法中，key用来判断该元素节点是被移动过来的还是新创建的元素，减少不必要的元素重复渲染。


### setState第二个参数的作用
因为setState是一个异步的过程，所以说执行完setState之后不能立刻更改state里面的值。如果需要对state数据更改监听，setState提供第二个参数，就是用来监听state里面数据的更改，当数据更改完成，调用回调函数。


### sass和less的区别
定义变量的符号不同，less是用@，sass使用$
变量的作用域不同，less在全局定义，就作用在全局，在代码块中定义，就作用于整哥代码块。而sass只作用域全局。


### react中的高阶函数
高阶函数就是一个纯js且没有副作用的函数。
高阶组件就是一个函数，且该函数接受一个组件作为参数，并返回一个新的组件。


### react生命周期中，最适合与服务端进行数据交互的是哪个函数
`componentDidMount`：在这个阶段，**实例和dom已经挂载完成，可以进行相关的dom操作**。


### react中组件传值
父传子（组件嵌套浅）：父组件定义一个属性，子组件通过this.props接收。

子传父：父组件定义一个属性，并将一个回调函数赋值给定义的属性，然后子组件进行调用传过来的函数，并将参数传进去，在父组件的回调函数中即可获得子组件传过来的值。


### react性能优化阶段函数是哪一个？
`shouldComponentUpdate`


### Es6中箭头函数与普通函数的区别？
 - 普通function的声明在变量提升中是最高的，箭头函数没有函数提升
 - 箭头函数没有属于自己的this，arguments
 - 箭头函数不能作为构造函数，不能被new，没有property
 - call和apply方法只有参数，没有作用域


### 在constructor中绑定事件函数的this指向
把一个对象的方法赋值给一个变量会造成this的丢失，所以需要绑定this，把绑定放在构造函数中可以保证只绑定一次函数，如果放在render函数中绑定this的话每次渲染都会去绑定一次this，那样是很耗费性能的。


### 使用箭头函数也就是异步函数的方式写setState
setState它是一个异步函数，他会合并多次修改，降低diff算法的比对频率。这样也会提升性能。


### shouldComponentUpdate(nextProps, nextState)
当父组件被重新渲染时即render函数执行时，子组件就会默认被重新渲染，但很多时候是不需要重新渲染每一个子组件的。这时就可以使用 shouldComponentUpdate 来判断是否真的需要重新渲染子组件。仅仅一个判断，就可以节约很多的消耗。
所以对于父组件发生变化而子组件不变的情况，使用shouldComponentUpdate会提升性能。
```js
shouldComponentUpdate(nextProps, nextState) {
    if(nextProps.content === this.props.content) {
        return false;
    } else {
        return true;
    }
}
```


### 使用PureComponent
`PureComponent`内部帮我们实现了`shouldComponentUpdate`的比较，其他和Component一样。但是在shouldComponentUpdate进行的是一个**浅比较**，看看官方文档是怎么说的。

浅比较只比较第一层的基本类型和引用类型值是否相同

如果数据结构比较复杂，那么可能会导致一些问题，要么当你知道改变的时候调用`forceUpdate`,要么使用`immutable`来包装你的state


### 无状态组件
无状态组件就是使用定义函数的方式来定义组件，这种组件相比于使用类的方式来定义的组件(有状态组件)，少了很多初始化过程，更加精简，所以要是可以使用无状态组件应当尽可能的使用无状态组件，会大幅度提升效率