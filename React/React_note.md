## React 学习笔记

### 简介

1.  一个构建用户界面的JavaScript库
2.  一个把数据渲染成html视图的开源JavaScript库
3.  FaceBook开发，已开源

### 原生开发方式的缺点

1.  原生JavaScript操作DOM繁琐，效率低下
2.  原生JavaScript直接操作DOM, 浏览器会进行大量重绘，消耗性能
3.  原生JavaScript没使用组件化，复用率低下

### react的优点

1.  采用组价化模式，声明式编码，提高开发效率和组件复用率
2.  在React Native中可以使用React语法进行移动端开发
3.  使用了虚拟dom和优秀的diffing算法，尽量减少与真实dom的交互
4.  使用jsx文件来写，开发者可以更好的创建虚拟dom,便于开发

### 关于虚拟dom

1.  本质是Object类型的对象
2.  虚拟dom比较“轻”，真实dom比较“重”（虚拟dom上的属性少很多）
3.  虚拟dom最终会被react转为为真实dom渲染

### &#x20;关于jsx要注意的几点

1.  全称： JavaScript XML
2.  XML早期用于储存和传输数据用的
3.  定义虚拟dom不要写引号
4.  标签中混入js表达式时候使用花括号包裹
5.  样式类名指定不要用class,用className  &#x20;
6.  写内联样式时候，在style={{key\:value}}这样写。
7.  创建虚拟dom时候只能有一个根标签
8.  虚拟dom的标签必须闭合
9.  标签首字母若是小写开头，则把标签转化为html中的同名元素，若找不到这个同名元素则报错
10. 标签首字母若是大写，react则默认这是认为是开发者自己创的组件来渲染，若找不到组件则报错

### 生命周期相关-旧的

##### 初始化阶段 - 由ReactDOM.render()触发----初次渲染

1.  constructor()构造器最开始调用
2.  componentWillMount()即将挂载时候调用
3.  render()挂载过程
4.  componentDidMount()挂载后调用

##### &#x20;更新阶段-----组件内部this.setState()或者父组件重新render触发

1.  shouldComponentUpdate()组件状态更新后触发，此函数react会默认调用并返回true，触发页面更新。若使其返回false则不更新页面
2.  componentWillUpdate()更新前触发
3.  render()
4.  componentDidUpdate()更新后触发

##### 卸载组件 ----使用ReactDOM.unmountComponentAtNode()触发

1.  componentWillUnmount() 页面即将卸载时候触发

### 生命周期相关-新的

###### 初始化阶段-阶段 - 由ReactDOM.render()触发----初次渲染

1.  constructor()构造器最先调用
2.  getDerivedStateFromProps获取派生的state在props ---- 极少使用
3.  render()挂载过程
4.  componentDidMount()挂载后调用

##### 更新阶段-----组件内部this.setState()或者父组件重新render触发

1.  getDerivedStateFromProps获取派生的state在props ---- 极少使用
2.  shouldComponentUpdate()组件状态更新后触发，此函数react会默认调用并返回true，触发页面更新。若使其返回false则不更新页面
3.  componentWillUpdate()更新前触发
4.  render()
5.  getSnapshotBeforUpdate()获取页面更新之前的快照，页面更新前的状态。可在页面更新前进行一些前置操作
6.  componentDidUpdate()更新后触发

