## 1.ECMA相关介绍

### 什么是ECMA

*   ECMA（European Couputer Manufacturers Association）中文名称为欧洲计算机制造上协会，该组织的目标是评估，开发和认可计算机标准，1994年该组织更名为Ecma国际

### 什么是ECMAScript

*   是Ecma国际通过ECMA-262标准化的脚本程序设计语言

## 2.ES6特性

### &#x20;1.let变量声明及声明特性

```javascript
let a
let b,c,d
let e = 100
let f = 521,g='哈哈哈',h=[]
```

*   变量不可重复声明，使用let重复声明一个变量会报错，var则不会
*   形成块级作用域（es5中有三种作用域-1全局2.函数3.eval）&#x20;

```javascript
{
  let A = '哈哈'
}
console.log(A); // 会报错，使用var定义则能正常读取
// if else while for 中使用let都会有块级作用域
```

*   不存在变量声明提升

```javascript
console.log(a) // 获取变量在var声明之前,虽然已经赋值,由于变量声明提升,会打印undefined
var a = '哈哈'
console.log(b) // 获取变量在let声明之前,虽然已经赋值,而且没变量声明提升,控制台会打印错误
let b = '嘿嘿'
```

*   不影响作用域链

### 2.cost变量声明

*   定义常量 `const SCHOOL= '尚硅谷'`
*   一定要赋值，否则会报语法错误
*   一般写成大写(潜规则-非强制要求)
*   如果声明的变量是基本数据类型，则不可修改，会报错
*   也有块级作用域
*   对数组和对象中的元素修改，不算对常量的修改，不会报错

### 3.变量的解构赋值

*   es6允许你按照一定模式从数组和对象中取值，对变量进行赋值，此为解构赋值
*   数组的解构

    ```javascript
    const F4 = ['小沈阳','刘能','赵四','宋小宝']
    let [xiao,liu,zhao,song] = F4
    console.log(xiao)
    console.log(liu)
    console.log(zhao)
    console.log(song)
    // 如此取值
    ```
*   对象的解构

    ```javascript
    const zhao = {
      name:'赵本山',
      age:70
      xiaoping: function(){
        console.log('演小品')
      }
    }
    let {name,age,xiaoping} = zhao
    ```
*   由此可见解构数组用中括号，解构对象用花括号
*   数组的解构按顺序依次赋值，对象则按照名称来解构

### 4.模板字符串

*   可用反引号申明，如`` let A = `A` ``

```javascript
let sekiro = '只狼'
let out `${sekiro}是一个忠心的忍者`
console.log(out) // 只狼是一个忠心的忍者
```

### 5.对象的简写

*   es6允许在大括号里面直接写入对象的属性和方法

    ```javascript
    let name = '尚硅谷'
    let change = function(){
       console.log('学习知识')
     }
    const school = {
     name,
     change,
     improve: function(){console.log('学习知识')}
     // improve(){console.log('学习知识')}对象中方法可以如此简    写，省略function和冒号
    }
    ```

### 6.箭头函数(常用-重点)

*   `let fn = ()=>{}`

    ```javascript
    let fn = (a,b) => { return a +b }
    ```
*   this是静态的。this始终指向函数声明的时候所在作用域下this的值，无法通过call和apply等方法改变.

    ```javascript
    function fn1(){this.name}
    let fn2 = ()=>{this.name}
    window.name = '尚硅谷'
    const SCHOOL = {name:'shangguigu'}

    //直接调用
    fn1() //尚硅谷
    fn2() //尚硅谷

    //call调用
    fn1.call(SCHOOL) //shangguigu
    fn2.call(SCHOL) // 尚硅谷（this指向是静态的）
    ```
*   无法作为构造函数实例化对象，new一个箭头函数的时候会报错
*   不能使用arguments对象
*   箭头函数的简写

    1.  可省略小括号，当形参有且只有一个的时候

        ```javascript
        let add = (n) => { return n + n }
        // 可写成如下-效果一样
        // let add = n => { return n + n }
        ```
    2.  还可省略花括号，当代码体只有一条语句的时候,此时return也必须省略，语句的执行结果就是返回值

        ```javascript
        let pow = (n) => {
         return n*n
        }
        let pow = n => n*n // 最简写形式
        ```
*   箭头函数适合于this无关的回调，定时器，数组方法等回调，不适合和this有关的回调，事件回调。

### 7.函数参数默认值设置

*   es6允许给函数参数赋值初始值

    ```javascript
    function add(a,b,c){
     return a + b + b
    }
    let result = add(1,2) // NAN 因为你少传了一个参数，3+undefinde = NAN了
    -------------------------------------------------
    function add(a,b,c = 10){ //可以在此处给形参给初始值，一般具有默认值的放后面
     return a + b + b
    }
    let result = add(1,2) // 13 
    ```
*   与解构赋值结合使用

    ```javascript
    // 常规写法
    function connect(options){
     let host = options.host
     let userName= options.username
    }
    // 解构赋值写法
    function connect({host,username}){
     let host = host
     let userName= username
    }
    // 解构赋值写法加給默认值
    function connect({host,username,prot='2222'}){
     let host = host
     let userName= username
     let prot= prot
    }
    connect({host:'localhost',username:'root'})

    ```

### 8.rest参数

*   es6引入rest参数，用于获取函数的实参，用来代替arguments
*   获取实参的方式

    ```javascript
    // es5的方式
    function date() {
     console.log(arguments) //获取传递的argaments对象
    }
    date('1','2')
    // es6 rest方式-把传进的参数用数组包裹
    function date(...args) {
     console.log(args) // ['1','2']  变成数组了
    }
    date('1','2')
    // rest参数必须放在最后,放在前面或者中间会报错
    function date(a,b,...args) {
     console.log(args) // '1','2',['3','4','5']
    },
    date('1','2','3','4','5')
    ```

### 9.拓展运算符

*   ... 可以把数组转化为逗号分割的参数序列

    ```javascript
    const arr = ['刘备','关羽','张飞']
    function fn1(arr){
     console.log(arguments)
    }
    fn1(arr);
    fn1(...arr)// 等同于fn1('刘备','关羽','张飞')
    ```
*   注意和rest的区别

    1.  **rest实在形参上面加...**，这样把传进来的参数变成数组包裹
    2.  **这个是在实参上面加...**，这样把数组参数拆分传递了

    ```javascript
    let A = ['刘备','关羽','张飞']
    let B = ['孔明','徐庶','庞统','法正']
    let C = ['赵云','黄忠','马超']
    let D = [...A,...B,...C]
    console.log(D) // ['刘备','关羽','张飞','孔明','徐庶','庞统','法正','赵云','黄忠','马超']
    ```
*   把伪数组转化成真正的数组

### 10.Symbol

*   新引入的一种数据类型，表示独一无二的值。一种类似字符串的数据类型
*   值是唯一的，解决命名冲突问题
*   无法和其他数据进行运算，比较，控制台会报错
*   Symbol定义的对象属性无法使用`for...in`循环遍历，但是可以使用`Reflect.ownKeys`来获取对象的键名

    ```javascript
    // 创建Symbol
    let s = Symbol()

    let s2 = Symbol('尚硅谷')
    let s3 = Symbol('尚硅谷')
    console.log(s2===s3) // false

    let s4 = Symbol.for('尚硅谷')
    let s5 = Symbol.for('尚硅谷')
    console.log(s4===s5) // true

    ```
*   基本数据类型总结：undefined String Symbol Boolean null number
*   使用场景：给对象增加属性和方法-独一无二的

    ```javascript
    let game = {} // 假设一个对象里面东西很多，方法很多
    // 声明一个对象
    let methods = {
     up: Symbol(),
     down: Symbol()
    }
    // 情况1：如此添加方法
    game[methods.up] = function(){console.log('xx')}
    game[methods,down] = function(){console.log('xx')}

    // 情况2：这样增加方法
    let youxi = {
      name: '法环'
      [Symbol('say')]: function(){
        console.log('xx')
      }
    }:
    ```
*   Symbol内置值

    1.  `Symbol.hasInstance` 当其他对象使用`instanceof`运算符，判断该对象的实例时，会调用这个方法
    2.  `Symbol.isConcatSpreadable` 对象的`Symbol.isConcatSpreadable`属性等于的是一个布尔值，表示该对象用于`Array.prototype.concat()`时候，是否可以展开
    3.  `Symbol.replace`当对象被`str.replace(myObject)`方法调用时候，会返回该方法的返回值
    4.  `Symbol.search`
    5.  ......

### 11.迭代器

*   迭代器是一种接口，为各种不同的数据结构提供统一的访问机制。任何数据结构只要部署了迭代器（Iterator）接口，就可以完成遍历操作

    1.  es6创造了一种新的遍历命令 for...of循环，Iterator接口主要供for...of消费

    2.  原生具备iterator接口的数据

        *   Array

        *   Arguments

        *   Set

        *   Map

        *   String

        *   TypedArray

        *   NodeList

            ```javascript
            const A = ['刘备','关羽','张飞']
            // for...of
            for (let v of A){
            	console.log(v) // 依次'刘备','关羽','张飞'
            }

            let iterator = A[Symbol.iterator]() //返回一个对象，包含一个next方法

            iterator.next() // {value:'刘备',done:false}
            iterator.next() // {value:'关羽',done:false}
            iterator.next() // {value:'张飞',done:false}
            iterator.next() // {value:undefined,done:true}
            ```

    3.  工作原理

        *   创建一个指针对象。指向当前数据结构的起始位置
        *   第一次调用对象的next方法，指针自动指向数据结构的第一个成员
        *   接下来不断调用next方法，指针一直往后移动，直到指向最后一个成员
        *   每调用一个next方法包含一个返回value和done属性的对象

            ***若想自定义遍历数据的时候，要想到迭代器-如下实现***

            ```javascript
            let obj = {
              name: '哈哈'
              stus: ['刘备','关羽','张飞' ]
            }
            // 正常情况下，我们想遍历对象，会采用for...in。若使用for...of遍历如上对象，则会报错。
            // 报错如：obj[Symbol.iterator] is not a function
            // 若非想要for...of直接遍历其中的stus,可做如下加工
            let obj = {
              name: '哈哈',
              stus: ['刘备','关羽','张飞' ],
              // 加了这个东西，可支持for...of遍历了
              [Symbol.iterator](){
                 let index = 0
                 let _this = this;
                 return {
                    next:function(){
                      if (index < _this.stus.length){
                         conset result = {value:_this.stus[index],done:false}
                         index++
                         return result
                      } elsSymbol.iterator
            ```

### 12.Generator

*   es6提供的一种异步解决方案，语法行为和传统函数不同
*   通过next()来控制代码执行
*   该方法返回一个迭代器，支持for..of遍历
*   生成器函数参数的传递，next可传递参数
    ```javascript
    function * gen(arg){
      console.log(arg) // 打印AAA, 因为你传的AAA
      let one = yield 111; 
      console.log(one)// 打印BBB
      let two = yield 222;
      console.log(two)// 打印CCC
      let three = yield 333;
      console.log(three)// 打印CCC
    }
    for (let v of ge()) { console.log(v)}

    let iterator = gen('AAA'); // 传的AAA
    iterator.next(); //第一个是启动
    iterator.next('BBB'); //next方法也能传参,传的BBB
    iterator.next('CCC'); // 
    iterator.next('DDD'); // 

    ```
*   异步编程解决 如文件操作，网络操作，数据库操作等

    ```javascript
    // 简单需求：1s后打印1,在等2s后打印2，在等3s后打印3

    // 方案1：回调方式，容易回调地狱
    setTimeout(() => {
     console.log(1)
     setTimeout(() => {
      console.log(2)
      setTimeout(() => {
       console.log(3)
      },3000)
     },2000)
    },1000)

    // 方案2：生成器实现
    function one(){
     setTimeout(() => {
       console.log(1)
       iterator.next()
      },1000)
    }
    function two(){
     setTimeout(() => {
       console.log(2)
       iterator.next()
      },2000)
    }
    function three(){
     setTimeout(() => {
       console.log(3)
       iterator.next()
      },3000)
    }
    function *gen(){
      yield one();
      yield two();
      yield three();
    }
    let iterator = gen();
    iterator.next()
    ```

### 13. Promise

*   es6引入的异步编程解决方案。语法上Promise是一个构造函数
*   用来封装异步操作并可以获取其成功和失败的结果

    ```javascript
    const p = new Promise(function(resolve,reject){
      // 此处写异步操作
      setTimeout(function(){
         let data = '数据'
         resolve(data)
      }, 1000)
    })
    p.then(function(){
     //成功
    },function(){
     //失败
    })
    ```

### 14.Set

*   es6提供了新的数据结构Set(集合)。类似于数组，成员的值都是唯一的。集合实现了iterator接口。故可以使用for..of和拓展运算符遍历
*   集合的属性和方法

    1.  size 返回集合的元素个数
    2.  add 新增一个元素，放回当前集合
    3.  delete 删除元素，返回boolean值
    4.  has 检测集合中是否包含某个元素，返回boolean值
    5.  clear清空

    ```javascript
    let s = new Set();
    let s2 = new Set(['刘备','关羽','张飞','张飞' ]) // ['刘备','关羽','张飞'] 自动去重

    //元素个数
    s2.size // 3
    //添加
    s2.add('诸葛亮')
    //删除
    s2.delete('诸葛亮')
    //检测-找不到返回false
    s2.has('刘备') // true 
    // 清空
    s2.clear() // []

    // 实践
    //去重
    let arr = [1,1,2,2,3,3,4,5,6]
    let arr1 = [1,2,7,8]
    //去重
    let result = [...new Set(arr)]
    // 交集
    let result = [...new Set(arr)].filter(item => {
     let s2 = new Set(arr1)
     if (s2.has(item)){
      return true
     } else {
      return false
     }
    })
    //并集
    let union = [...new Set(...arr,...arr1)]
    //差集
    let diff = [...new Set(arr)].filter(item=>!(new Set(arr2).has(item)));
    ```

### 14.Map

*   es6提供了Map数据结构，类似于对象，也是键值对的结合。但是键的范围不限于字符串，可用各种类型的值，包括对象来当做键。Map也实现了iterator接口。也可使用拓展与算符和for...of来遍历，Map有如下属性和方法

    1.  size 返回Map的元素个数
    2.  set 新增一个元素，返回当前Map
    3.  get 返回键名对象的键值
    4.  has 检测Map中是否包含某个元素，返回boolean值
    5.  clear 清空集合

```javascript
new m = new Map()
m.set('name','尚硅谷')
m.set('change', function(){console.log('改变')})
let key = {school:'北京大学'}
m.set(key,['北京'])
```

### 15.class

*   es6提供了更接近传统语言的写法，引入了class的概念，作为对象的模板，通过class关键字 ，可以定义类。基本上，es6的class可以看做语法题。他的绝大部分功能，es5也能做到，新的class写法只是让对象原型的写法更加清晰，更像面向对象编程的语法而已

    1.  class声明类
    2.  constructor 定义构造函数初始化
    3.  extends 继承父类
    4.  super 调用父级的构造方法
    5.  static 定义静态方法和属性
    6.  父类方法可重写

    ```javascript
    // 手机
    function Phone(brand,price) {
     this.brand = brand
     this.price = price
    }
    // 添加方法
    Phone.prototype.call = function(){
     console.log('打电话')
    }
    // 实例化对象
    let mi = new Phone('小米',2222)
    // 智能手机-创建一个二级构造函数,继承Phone这个一级构造函数
    function SmartPhone(brand,price,color,size) {
      Phone.call(this, brand, price)
      this.color = color
      this.size = size
    }
    // 设置子级构造函数的原型
    SmartPhone.prototype = new Phone
    SmartPhone.prototype.constructor = SmartPhone
    // 声明一些子类的方法
    SmartPhone.prototype.photo = function(){ console.log('拍照')}
    SmartPhone.prototype.playGame= function(){ console.log('打游戏')}


    // class方式
    class Phone {
      constructor(brand,price){
        this.brand = brand
        this.price = price
      }
      static name = '手机' //使用了static关键字，那么定义的属性和方法就只属于这个类，实例上无法访问
      // call:function(){} // 不可用这种写法
      call(){
       console.log('打电话')
      }
      // 用get和set也能监听到数据的读取和修改
      get type(){ 
         console.log('type被读取了')
         return '5g手机'
      }
      set type(newValue){
         console.log('type被修改', newValue)

      }
    }


    // class类继承
    class SmartPhone extends Phone {
      constructor(brand,price,color,size){
        super(brand,price)
        this.color = color
        this.size = size
      }
      photo(){ console.log('打游戏')}
      playGame(){ console.log('打游戏')}
    }
    ```

### 16.数值扩展

*   `Number.isFinite` 检测一个数值是否为有限数
*   `Number.EPSILO` 是Javascript表示的最小精度
*   `Number.isNaN` 检测一个数值是否为NaN
*   `Number.parseInt  / Number.parseFloat` 字符串转整数或浮点数
*   `Number.isInteger` 判断一个数值是否为整数
*   `Math.trunc` 把数值的小数部分抹去
*   `Math.sign` 判断一个数到底是正数，负数，还是零

### 17.对象方法扩展

*   `Object.is` 判断两个值是否完全相等，和===有些相似

    ```javascript
    Object.is(12,12) // true
    Object.is(NaN,NaN) // true
    NaN === NaN // false
    ```
*   `Object.assign({},{})` 对象的合并，如果有相同的，参数后面的会把前面覆盖
*   `Object.setPrototypeOf `设置原型对象

### 18.模块化

*   模块化是指讲一个大的程序文件，拆分成许多小的文件，然后将小文件组合起来
*   模块化的好处

    1.  防止命名冲突
    2.  代码复用
    3.  高维护性
*   es6之前的模块化规范

    1.  `commonJs => NodeJs,Browserify`
    2.  `AMD => requireJS`
    3.  `CMD => seaJS`
*   es6模块化语法

    1.  import // 引入

        ```javascript
        // 通用导入方式
        import * as M1(文件名称) from "./M1.js"

        // 解构赋值形式
        import {A,fn1} from "./M1.js"
        //若是有重名情况可如下处理
        import {A as a,fn1} from "./M1.js" //A的值就赋给a了
        // 若引入的文件写法是默认暴露，可按下方形式引入
        import {default as m3} from "./M1.js"

        // 简写形式-要求引入的必须是默认暴露
        import m3 from "./M1.js"
        ```
    2.  export //暴露出去

        ```javascript
        // ==== 这是M1.js文件 ====

        // 分别暴露
        export let A = 'XXX'
        export function fn1 (){console.log('xxx')}

        // 统一暴露
        let A = 'XXX'
        function fn1 (){console.log('xxx')};
        export {A, fn1};

        // 默认暴露
        export default {
          school: 'xxx'
          change: function fn1 (){console.log('xxx')}
        }

        ```
*   babel对es6模块化的代码装换

    1.  考虑各种浏览器兼容问题需要用到bable来进行转化
    2.  babel是js的一个编译器

        ```javascript
        //安装工具 babel-cil babel-preset-env browserify
        //npx babel sec/js -d dist/js
        //打包 npx browserify dist/js/app.js -o dist/bundle.js
        ```

### 19.Proxy代理

*   作用是在对象和对象的属性值之间设置一个代理，获取该对象的值或者是设置该对象的值，以及实例化等等多种操作都会被拦截住。经过这一层之后我们可以统一处理。

    ```javascript
    let target = {}
    let proxy = new Proxy(target,{
     // 获取
     get(target, prop) {
       return target[prop]
     }
     // 修改
     set(target,key,value) {
      target[key] = value
     }
    })
    ```

### 20.Reflect反射

*   es6中新增的内置对象，用于实现一些与对象相关的操作。
*   Reflect 不是一个函数对象，所以它是不可构造的，也就是说它不是一个构造器，不能通过 new 操作符去新建或者将其作为一个函数去调用 Reflect 对象。

    1.  `Reflect.get()`获取对象身上某个属性的值
    2.  `Reflect.set()`在对象上设置属性
    3.  `Reflect.has()`判断一个对象是否存在某个属性
    4.  `Reflect.deleteProperty()`删除对象上的属性
    5.  `Reflect.getPrototypeOf()`获取指定对象原型的函数
    6.  `Reflect.setPrototypeOf()`设置或改变对象原型的函数
    7.  `Reflect.isExtensible()`判断一个对象是否可扩展 （即是否能够添加新的属性）
    8.  `Reflect.preventExtensions()`阻止新属性添加到对象
    9.  `Reflect.getOwnPropertyDescriptor()`获取给定属性的属性描述符
    10. `Reflect.defineProperty()`定义或修改一个对象的属性
    11. `Reflect.ownKeys()`返回由目标对象自身的属性键组成的数组
    12. `Reflect.apply()`对一个函数进行调用操作，同时可以传入一个数组作为调用参数
    13. `Reflect.construct()`对构造函数进行 new 操作，实现创建类的实例
    14. `Reflect.preventExtensions()`阻止新属性添加到对象

## ES7新特性

### 1.includes

*   includes方法用来检测数组中是否包含某个元素，返回boolean值

### 2.\*\*

*   指数操作符

```javascript
Math.pow(2,10) === 2**10 // true
```

## ES8新特性

### 1.async\&await

*   async声明一个async函数

    1.  async函数返回一个promise对象
    2.  promise对象的结果由async函数执行的返回值决定

    ```javascript
     // 返回的不是promise类型的对象，result则接收到一个成功的promise
    async function fn(){ 
      return '哈哈'
    }

     // 若抛出出了错误，如此则返回一个失败的promise
    async function fn(){ 
      throw new Error('出错')
    }

     // 若返回的是一个promise对象，则取决于你返回对象的状态
    async function fn(){ 
      return new Promise((resolve,reject) => {
    		resolve()
            reject()
      })
    }
    const result = fn()
    ```

*   await表达式

    1.  必须卸载async函数中
    2.  await右侧表达式一般是promise对象
    3.  await返回的是promise成功的值
    4.  await的promise失败了，就抛出异常，要通过try...catch捕获

    ```javascript
    let p = new Promise((resolve,reject) => {
    		resolve('成功')
    })
    async function fn(){ 
      try {
    	let result = await p
        console.log(result)
      }catch(e){
        console.log(e)
      }  
    }
    fn() // 成功
    ```

*   封装ajax请求
    ```javascript
     function sendAJAX(URL) {
      return new Promise((resolve,reject) => {
        const x new XMLHttpRequest();
        x.open('GET', URL);
        x.send();
        x.onreadystatechange = function(){
          if (x.readyState === 4) {
            if(x.status >=200 && x.status <300) {
              resolve()
            }else{
              reject(x.status)
            }
          }
        }
      })
     }

    async function main(){
     let result = await sendAJAX('https://......')
    }
    ```

### 2.对象方法扩展

*   `Object.keys()`获取对象所有的键
*   `Object.values()`获取对象所有的值
*   `Object.entries()`把对象改成数组，键值形式也改成数组形式，如下

    ```javascript
    let a = {
     name: '只狼',
     weapon: ['楔丸','拜泪']
    }
    object.entries(a) //把数据加工成如下
    [
     ['name','只狼'],
     ['weapon', ['楔丸','拜泪']]
    ]

    ```
*   `Object.getOwnPropertyDescriptors()`对象属性的描述对象

## ES9的新特性

### 1.扩展运算符和Rest参数

*   Rest参数和spread 扩展运算符在es6中已经引入，不过是只针对数组的
*   es9中为对象提供了像数组一样的rest参数和扩展运算符

    ```javascript
    function date({name,sex,...data}) {
     console.log(name) // '只狼'
     console.log(sex) // '男'
     console.log(data) // {career:'忍者', weapon:['楔丸','拜泪']}
    }
    date({
     name: '只狼',
     sex: '男',
     career: '忍者',
     weapon: ['楔丸','拜泪']
    })
    ```
*   扩展对象

    ```javascript
    const A = {name: '只狼'}
    const B = {sex: '男'}
    const C = {career: '忍者'}
    const D = {weapon: ['楔丸','拜泪']}
    // 合并对象
    const Sekiro = {...A,...B,...C,...D}
    ```

### 2.正则扩展

*   ...

## ES10新特性

### 1.对象扩展方法

*   Object.fromEntries 创建一个对象，参数需是二维数组或者map

    ```javascript
    const result = Object.fromEntries([
     ['name','只狼'],
     ['weapon', '楔丸,拜泪']
    ])
    // 转化如下
    {
     name: '只狼',
     weapon: '楔丸,拜泪'
    }

    // =================================

    const m = new Map()
    m.set('name','只狼')
    const result = Object.fromEntries(m)
    // m变成了
    {
     name: '只狼'
    }

    ```

### 2.字符串方法扩展

*   trimStart 清除字符串左侧空白字符
*   trimEnd 清除字符串右侧空白字符
*   trim(es5的方法，清除字符串两边的字符)

### 3.数组方法扩展

*   flat把多维数组转化为低维数组

    ```javascript
    const arr = [
     1,
     [2,3,4],
    ]

    arr.flat() // [1,2,3,4]

    //-----转化多维数组时候，从外面开始转-----

    const arr = [
     1,
     [2,3,4,[5,6,7,8]],
    ]
    arr.flat() // [1,2,3,4,[5,6,7,8]]

    // ------可传参，表示转化的深度--------

    const arr = [
     1,
     [2,3,4,[5,6,7,8]],
    ]
    arr.flat(2) // [1,2,3,4,5,6,7,8]
    ```
*   flatMap类似map方法和flat方法的结合

    ```javascript
    const arr = [
     1,2,3,4,5
    ]
    arr.flatMap(item => [item * 10])
    // 若使用map则返回[[10],[20],[30],[40],[50]]
    // 使用flatMap则返回[10,20,30,40,50]
    ```

### 4.Symbol扩展

*   Symbol.prototype.description 获取Symbol的字符串描述

    ```javascript
    let s = Symbol('aaa')
    s.description  // 'aaa'
    ```

## ES11新特性

### 1.私有属性

```javascript
class Phone {
  brand;
  #price;
  constructor(brand,price){
    this.brand = brand
    this.#price = price
  }
}
// 实例化
const mi = new Phone('MI','999')
console.log(mi.brand) // 可正常访问
console.log(mi.#price) // 理论上不可访问，但是写出来还是能访问，说好的私有属性呢？好怪？？
```

### 2.Promise.allSettled

*   接受一个promise数组，返回一个promise对象，返回的结果是成功的状态。成功的值是里面每一个promise的结果

    ```javascript
    // 声明两个Promise对象
    const p1 = new Promise(function(resolve,reject){
      // 此处写异步操作
      setTimeout(function(){
         let data = '数据1'
         resolve(data)
      }, 1000)
    })
    const p2 = new Promise(function(resolve,reject){
      // 此处写异步操作
      setTimeout(function(){
         let data = '数据2'
         resolve(data)
      }, 1000)
    })
    const result = Promise.allSettled([p])

    // 和Promise.all一样都是执行批量异步任务 ，如果每个任务你都需要结果,就用Promise.allSettled。如果需要每个任务都成功才能执行就用Promise.all
    ```

### 3.字符串方法扩展

*   metchAll 批量获取正则批量匹配的一个结果

### 4.可选链操作符

*   `?.`   应对对象类型参数，层级可能很深解决层级判断问题

    ```javascript
    function main(config){
     // 获取之前要判断数据是否有无，就很麻烦
     const dbHost = config && config.db && config.db.host
     // 有这个?.就行
     const dbHost =config?.db?.host

    }
    main({
     db:{
       host:'192.168.1.1',
       userName: 'root' 
     }
    })
    ```

### 5.动态import

*   import()函数，返回promise对象

    ```javascript
    import('./hello.js').then((resolve) => {
      resolve // 引入的模块
    })
    ```

### 6.Biglnt

*   语法：普通整型数字基础上，加一个`n`

    ```javascript
    let n = 12n;
    // n 变成大整型了

    Biglnt(n)
    // n 变成大整型了

    Biglnt(1.2) //报错。不可用浮点型

    // 大数值运算
    let max = Number.MAX.SAFE_INTEGET
    max + 1
    max + 2  // 错误结果，无法运算了

    Biglnt(max)
    Biglnt(max) + Biglnt(1)
    Biglnt(max) + Biglnt(2) //正确结果
    ```
*   主要用于大数值运算，计算的时候必须是BigInt类型和BigInt类型去计算，否则会报错

### 7.绝对全局对象

*   globalThis 若想对全局对象进行操作，可忽略环境，直接使用globalThis操作

### 8.空值合并运算符

*   `??` 是一个逻辑运算符，当左侧为null或者undefined的时候，返回右侧的操作数，否则返回左侧操作数
*   和||的区别就是||会把0和控字符串转化为false,显示右侧的值



    ```javascript
    (0 || '哈哈') // 哈哈
    (0 ?? '哈哈') // 0

    ```

## ES12新特性

### 1.逻辑赋值操作符

*   ??=
*   &&=
*   ||=

    ```javascript
    let a = true
    let b = false

    a = a && b //若想结果赋值给对比的某一项，以前的写法，a为false
    a &&= b //新写法 a也赋值为false了

    a = a || b
    a ||= b

    a = a ?? b
    a ??= b
    ```

### 2.数字分割符

*   这个新特性是方便程序员看代码出现的，如果数字比较大，不便于查看，它允许我们在数字之间添加下划线(\_)字符，使数字更具可读性

### 3.replaceAll

*   `replaceAll()` 方法用于在字符串中用一些字符替换另一些字符，或替换一个与正则表达式匹配的子串，该函数会替换所有匹配到的子字符串。

    ```javascript
    var str="Visit Microsoft! Visit Microsoft!";
    var n=str.replaceAll("Microsoft","Runoob");
    n // Visit Runoob!Visit Runoob!
    ```

### 4.Promise.race

*   `Promise.all()`方法用于将多个 `Promise` 实例，包装成一个新的 `Promise` 实例。

    `Promise.all()`全部子实例都成功才算成功，有一个子实例失败就算失败。
*   `Promise.race()`方法也是将多个 `Promise` 实例，包装成一个新的 `Promise` 实例。

    `Promise.race()`rece是赛跑机制，要看最先的promise子实例是成功还是失败。
*   `Promise.any()`方法同样是将多个 `Promise` 实例，包装成一个新的 `Promise` 实例。

    `Promise.any()`有一个子实例成功就算成功，全部子实例失败才算失败。

### 5.WeakRefs

*   ​WeakRef​​​是一个 Class，一个​​WeakRef​​​对象可以让你拿到一个对象的弱引用。这样，就可以不用阻止垃圾回收这个对象了。 可以使用其构造函数来创建一个​​WeakRef​​对象。

## &#x20;ES13新特性

### 1.顶层 await 表达式

*   以往我们总是在异步函数体内使用 await 表达式，自 ES13 后，我们也可以在一个模块的顶层使用它了。

    ```javascript
    // 异步加载资源
    import { fetchPerson } from '@/services/person';
    const person = await fetchPerson();

    // 异步加载组件
    await import('your-component-path');

    import { get } from '@/utils/request';

    export async function getPerson() {
        const person = await get('/person');
        return person;
    }
    ```

### 2.class新特性

*   在类的定义中，以 "#" 开头的标识符为私有标识符，由私有标识符定义的类元素被称为私有类元素，私有类元素只能在类中才能被访问到。类的属性、静态属性、方法、静态方法、访问器、静态访问器都可以被定义为私有类元素。
*   静态块，静态块为我们提供了更加灵活的静态类元素初始化渠道，我们可以在静态块中使用一系列的语句来完成静态类元素的初始化。我们可以利用 this 在静态块中访问类的其他静态属性（包括私有属性）。

    ```javascript
    class ClassA {
        // 静态属性
        static staticProperty;

        // 静态块初始化静态属性，捕捉错误
        static {
            try {
                this.staticProperty = getStaticProperty();
            } catch {
                console.log('Error');
            }
        }
    }
    ```
*   私有in操作符， in 操作符可以帮我们判断实例中是否存在属性，当新增了私有化类元素后，我们也可以和下面例子一样，在类定义内使用 in 操作符判断私有化类元素存在与否。

    ```javascript
    class ClassA {
        // 私有属性
        #privateProperty;

        // 利用 in 操作符判断私有属性是否存在
        static hasPrivateProperty(instance) {
            return #privateProperty in instance
        }

        constructor(privateProperty) {
            this.#privateProperty = privateProperty;
        }
    }

    const instance = new ClassA('private-property');
    ClassA.hasPrivateProperty(instance);
    // true
    ```



### 3.正则 /d 标志

*   ES13 新增的 d 标志对应正则实例的 hasIndices 属性，当设置 d 标志时，hasIndices 为 true 。使用 d 标志后，正则表达式的匹配结果将包含每个捕获组捕获子字符串的开始和结束游标。

    ```javascript
    // 字符串
    const str = "today is saturday";

    // 正则表达式
    const reg = /saturday/d;

    // 匹配结果输出游标
    const [start, end] = reg.exec(str).indices[0];

    console.log([start, end]); // [9, 17]
    ```

### 4.Error 对象的 cause 属性

*   在以往 Error 对象只能传递消息信息，现在 ES13 为 Error 增添了 cause 属性，它可以是任意数据类型，方便我们获取更多地错误信息。

    ```javascript
    try {
        // 抛出带 cause 属性的 Error实例
        throw new Error('faied message', { cause: 'cause' });
    } catch (error) {
        // 捕获 error 并输出 cause
        console.log(error.cause)
    }

    // 输出
    // cause
    ```

### 5.at 方法

*   利用下标访问数组元素时，下标会被转换为字符串，负数下标也对应着一个独立的数组元素，所以 Js 的数组是不支持关联访问的。ES13 新增了 at 方法，使得我们可以利用负数索引进行关联访问，例如以下示例，我们可以利用 at 方法和负数索引 -2 来访问倒数第二个元素。

    ```javascript
    const array = [5, 12, 8, 130, 44];

    array.at(-2); // 130
    ```

### 6.Object.hasOwn

*   判断这个实例上是否有属性，以代替之前的 Object.prototype.hasOwnProperty 方法。

    ```javascript
    const object = {
        count: 1
    };

    Object.hasOwn(object, 'count'); // true
    Object.hasOwn(object, 'toString'); // false
    Object.hasOwn(object, 'property'); // false
    ```

## &#x20;ES14新特性

### 更新中...







