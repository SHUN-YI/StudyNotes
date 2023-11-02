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

### 8. rest参数

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

### 12.生成器&#x20;

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























