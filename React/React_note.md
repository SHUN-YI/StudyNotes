# React 学习笔记

### 1.react简介

*   **基本介绍**

    1.  一个构建用户界面的JavaScript库
    2.  一个把数据渲染成html视图的开源JavaScript库
    3.  FaceBook开发，已开源

*   **原生开发方式的缺点**

    1.  原生JavaScript操作DOM繁琐，效率低下
    2.  原生JavaScript直接操作DOM, 浏览器会进行大量重绘，消耗性能
    3.  原生JavaScript没使用组件化，复用率低下

*   **react的优点**

    1.  采用组价化模式，声明式编码，提高开发效率和组件复用率
    2.  在React Native中可以使用React语法进行移动端开发
    3.  使用了虚拟dom和优秀的diffing算法，尽量减少与真实dom的交互
    4.  使用jsx文件来写，开发者可以更好的创建虚拟dom,便于开发

*   **关于虚拟dom**

    1.  本质是Object类型的对象

    2.  虚拟dom比较“轻”，真实dom比较“重”（虚拟dom上的属性少很多）

    3.  虚拟dom最终会被react转为为真实dom渲染

    *   关于jsx要注意的几点

    1.  全称： JavaScript XML
    2.  react定义的一种类似于xml的js扩展语法：js+xml
    3.  本质是React.createElement(component,props,...children)方法的语法糖

*   **jsx语法规则**

    1.  定义虚拟dom不要写引号
    2.  标签中混入js \*\*\*表达式 \*\*\*时候使用花括号包裹
    3.  样式类名指定不要用class,用className  &#x20;
    4.  写内联样式时候，在style={{key\:value}}这样写。
    5.  创建虚拟dom时候只能有一个根标签
    6.  虚拟dom的标签必须闭合
    7.  标签首字母若是小写开头，则把标签转化为html中的同名元素，若找不到这个同名元素则报错
    8.  标签首字母若是大写，react则默认这是认为是开发者自己创的组件来渲染，若找不到组件则报错

### 2.基础案例

*   hello\_react案例

```javascript

<div id="test"></div>

// 引入核心库
<script type="text/javascript" src="react.development.js"></script>
<script type="text/javascript" src="react-dom.development.js"></script>
<script type="text/javascript" src="babel.min.js"></script>

<script type="text/babel"> // 此处需写babel,才能正确识别jsx语法
  // 1.创建虚拟dom
  const VDOM = <h1>hello,React</h1> // 不用加单引号，因为是jsx语法，这个并不是字符串
  
  // 2.渲染虚拟dom到页面 
  ReactDOM.render(VDOM, document.getElementById('test'))// 接受两个参数，虚拟dom和容器 
</script>
```

### 3.虚拟DOM的两种创建方式

*   使用**jsx**创建虚拟dom

```javascript
<script type="text/babel">
  // 1.创建虚拟dom
  const VDOM = <h1 id="title">hello,React</h1>
  // 2.渲染虚拟dom到页面 
  ReactDOM.render(VDOM, document.getElementById('test'))
</script>
```

*   使用**js**创建虚拟dom

```javascript
<script type="text/javascript">
  // 1.创建虚拟dom
  const VDOM = React.createElement('h1',{id:'title'},'Hello,React')//参数：标签名，标签属性，标签内容
  // 2.渲染虚拟dom到页面 
  ReactDOM.render(VDOM, document.getElementById('test'))
</script>
```

*   js创建虚拟dom很繁琐，特别多层标签嵌套的时候，故就用jsx的语法开发更方便

### 4.虚拟dom和真实dom

*   虚拟dom是什么呢

    1.  本质是Object类型的对象(一般对象)
    2.  虚拟dom比较"轻"，真实dom比较“重”，因为虚拟dom是react内部使用，无需真实dom上那么多的属性
    3.  虚拟dom最终会被react转化为真实dom，呈现在页面上

    ```javascript
    <script type="text/babel">
      // 1.创建虚拟dom
      const VDOM = <h1 id="title">hello,React</h1>
      // 2.渲染虚拟dom到页面 
      ReactDOM.render(VDOM, document.getElementById('test'))
      console.log(VDOM) //其实是个一般的js对象
    </script>
    ```

### 5.函数式组件

*   函数式组件， 用于定义简单组件

    ```javascript
    <script type="text/babel">
      // 1.创建函数式组件  
      function Demo(){
        // 这里的this是undefined 因为babel翻译代码的时候开启了严格模式
        return <div>简单组件 </div>
      }
      // 2.渲染虚拟dom到页面 
      ReactDOM.render(<Demo/>, document.getElementById('test')) //用标签包裹函数组件，且首字母必须大写
    </script>
    ```

    *   执行ReactDOM.render之后发生了什么

        1.  react解析了组件标签，找到了你定义的组件
        2.  发现组件是用***函数***定义的，随后调用该函数，把返回的虚拟dom转化为真实dom，随后呈现在页面

### 6.类基本知识（复习）

```javascript
class Person {
 // 构造器方法，接参数
 constructor(name,age) {
   // 此处this是指向类的实例对象
   this.name = name
   this.age = age
 }
 // 一般方法
 speak(){
   // 这个speak方法其实放在了类的原型对象上，给实例使用
   // 通过Person实例调用speak时，speak中this就是person实例
   console.log(`我是${this.name},年龄是${this.age}`) 
 }
}
let p1 = new Person('tom',18)
let p2 = new Person('jerry',19)

// 创建一个Student类，继承与Person
class Student extends Person {
 constructor(name,age,grade) {
    super(name,age) // 如果你继承了一个父类，并且写了构造器函数，那么必须写super
    this.grade = grade
 }
}
```

*   类中的构造器不是必须写的，要实例化一些初始化操作，比如添加指定属性值时候才写
*   如果你继承了一个父类，并且写了构造器，那么构造器里面必须写super
*   类中定义的方法，都是放在类的原型对象上，供给实例去使用

### 7.类式组件

*   创建类式组件

    ```javascript
    class MyComponent extends React.Component {
      // render放在Mycomponent的原型对象上，供实例使用
      // this指向Mycomponent实例对象<=>Mycomponent组件实例对象
      render(){
        return <div>适用于复杂组件的定义</div>
      }
    }
    // 渲染
    ReactDOM.render(<MyComponent/>,document,getElementById('test'))
    ```
*   执行`ReactDOM.render`之后发生了什么

    1.  react解析了组件标签，找到了你定义的组件
    2.  发现组件是用***类***定义的，随后new出来该类的实例，并通过该实例调用到原型上的render方法。
    3.  将render返回的虚拟dom转为真实dom，随后呈现在页面上
*   `Mycomponent`的实例对象上有几个重要属性

    1.  context
    2.  props
    3.  refs
    4.  state

### 8.对state的理解

*   何为简单组件，何为复杂组件

    1.  有state(状态)的组件就是复杂组件，反之则为简单组件
*   何为state(状态)

    1.  影响组件x\行为
*   初始化state及基本使用

    ```javascript
    // 1.创建组件
    class Weater extends React.Component{
     constructor(props) {
       super(props)
       // react要求state是一个对象，用来存放组件需要的数据集合
       this.state = { 
         isHot:true
       }
     }
     render(){
       const {isHot} = this.state //解构赋值，便于取值使用
       return <div>今天天气很{isHot ? '炎热':'凉爽'}}</div>
     }
    }
    ReactDOM.render(<Weater/>,document,getElementById('test'))
    ```

### 9.react的事件绑定





