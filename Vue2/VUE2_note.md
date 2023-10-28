## &#x20;vue学习笔记

#### 一套用于构建用户界面的渐进式javaScript框架

框架作者：尤雨溪

### vue特点

1.  采用组件化模式，提高代码复用率，且让代码更好维护

2.  声明式编码，无需操作dom，无需直接操作dom，提高开发效率（区分于命令式编码）

3.  使用虚拟dom+优秀的diff算法，尽量复用dom节点，界面更流畅
    （通过diff算法对虚拟dom进行比对，相同的虚拟dom更新的时候无需再次替换）

### 创建vue实例

1.  el 用于指定当前vue实例服务于哪个容器。值通常为css选择器字符串
2.  data 中用于储存数据，数据供el所指定的容器去使用，值为对象类型

### 初识vue

1.  想让Vue工作，必须创建一个Vue实例，且需传入一个配置对象
2.  root容器中的代码依然符合html的规范，不过也混入的一些特殊的Vue语法
3.  root容器中的代码被称为-vue模板
4.  真实开发中只有一个Vue实例，并且会配合组件一起使用
5.  {{xxx}}中的xxx要写成表达式，并且要配合组件一起使用
6.  一旦data中单数据发生改变，页面中使用到改数据的地方会自动更新
7.  容器和实例必须是一一对应的关系
8.  vue模板语法有两大类，插值语法和指令语法

### 数据绑定

1.  v-bind 单项数据绑定（数据会同步到页面，页面不会同步到数据）
2.  v-mode 双向数据绑定（但是v-mode指令只应用到表单类元素，如select，input等等）
3.  v-mode\:value可简写为v-model，因为v-model默认收集的值就是value

### vue实例对象

（实例对象中有很多属性和方法，其中前缀为\$符号的可供给开发者使用，前缀为\_符号的方法作为框架自身使用）

1.  el的两种写法

    *   el可用vue实例对象原型上的\$mont方法去挂载
    *   new Vue的时候直接配置el属性
2.  data的两种写法
    *   data的第一种写法：对象式
    *   data的第二种写法：函数式
    *   注意由vue管理的函数不要写成箭头函数，写成箭头函数吼，this就不再是vue实例了

### 架构模型 <u>M</u> <u>V</u> <u>VM</u>

1.  M: 模型Model: 对应data的数据
2.  V：视图View：模板
3.  VM：视图模型ViewModel: Vue实例对象

*   开发者在dota中所写的属性，最后都出现在了vm身上
*   vm上面的所有属性及vue原型上的所有属性，在vue模板中都可以直接使用

### 数据代理 Object.defineProperty

示例：

```javascript
let person = {
    name: '张三'
    sex:'男'
}
Object.defineProperty(person,'age',
    {
        value:18
    }
)
```

这个代码就是重新给sex属性修改了值

Object.defineProperty 接受三个参数

1.  第一个参数，要添加属性的对象名称
2.  第二个参数，要添加的属性名称
3.  第三个参数，配置项

使用此方法修改值后，该属性不再可以被枚举，也无法修改，也无法删除

```javascript
let person = {
    name: '张三'
    sex:'男'
}
let number = 18
Object.defineProperty(person,'age',
    {
        value:18,
        enumberable:true, // 控制是否克枚举
        writable:true, // 控制是否可修改
        configurable:true // 控制是否克删除
        ... // 很多配置项
        // 当有人读取gerson的age属性时候，get函数（getter）就会被调用，返回值就是age的值
        get(){
            console.log(''有人读取age的属性了')
            return 'number'
        } 
        // 当有人修改person的age属性时候。set函数就会被调用，且会收到修改的具体值
        set(value){
            console.log('有人修改了age属性', value)
            number = value
        }
    }
)
```

在后面增加enumberable\:true后就可以被正常枚举了
在后面增加writable\:true后就可以正常修改了
在后面增加configurable\:true就可以正常删除了

根据以上代码段的写法，直接修改number的值的时候。由于get方法，age的值也会发生变化，从而起到绑定的效果。直接修改person的age时候，由于set方法的作用，number的值也会发生改变，就实现的双向绑定的效果了

### vue中的数据代理

vm.\_data = data

1.  通过vue对象来代理data对象中属性的操作读/写
2.  数据代理的好处，更加方便的操作data中的数据
3.  基本原理
    *   通过object.defineProperty()把data对象中的所有属性添加到vm上

    *   为每一个添加到vm上的属性，都指定一个getter/setter

    *   在geter/seter内部去操作data中对应的属性

### vue事件处理

1.  使用v-on 或 @ 绑定事件
2.  事件的回调需要配置在methods上
3.  methods中配置的函数，不要用箭头函数，否则this的指向就不是vm了
4.  methods中配置的函数，都是被vue锁管理的函数，this的指向是vm或者是组件实例对象
5.  @click='demo'和@click='demo(\$event)效果一样，后面这个可传递参数

### 事件修饰符

组织事件默认行为
@click.prevent可阻止默认事件，比如a标记使用此方式绑定事件，可以不触发a标签的跳转效果

vue中常见的事件修饰符

1.  prevent: 阻止默认事件
2.  stop:阻止事件冒泡
3.  once:事件只触发一次
4.  capture:使用事件的捕获方式
5.  self:只有event.target是当前操作的元素才是触发事件
6.  passive:事件的默认行为立刻执行，无需等待事件回调执行完毕

### 计算属性

1.  定义：要用的属性不存在，要通过已有属性计算而来
2.  原理：底层借助了object.defineProperty方法提供的getter和setter
3.  优势：与methods相比，内部有缓存机制，效率更高，调试方便
4.  计算属性就在vm上，直接读取即可
5.  如果要更改计算属性，必须写set函数去响应修改，且set要引起计算时候依赖数据去改变

`fullName:{
 get(){
  console.log('get被调用了') 
 }
}`

计算属性中的get作用？当有人读取这个fullName的时候，get就会被调用，返回值作为fullName的值

get什么时候被调用？

1.  初次读取fullName时候
2.  所依赖的数据发生变化时候

### 监视属性

监视属性配置项
immediate: true 初始化的时候调用一次

计算属性和data里的属性都可以监视

通过实例vm.\$watch('',{})也可以监视

监视多级结构中某个属性的变化写法
`   watch: {
     'numbers.a': {
     }
 `
这样写key值也是支持的

总结

1.  当监视函数属性变化时候，回调函数自动调用
2.  监视的属性必须存在才能进行监视
3.  监视的两种写法
    \* new Vue的时候直接传入watch配置
    \* 通过vm.\$watch来监视
4.  vue中的watch默认不检测对象内部值的改变
5.  配置deep\:true可检测对象内部值的改变

备注

1.  vue自身是可以检测对象内部值的变化的，但是vue提供的watch是默认不可以
    2.使用watch时候根据数据的具体结构，决定是否采用深度监视

### 条件渲染

v-show和v-if

v-show是控制display的值来控制显示和隐藏
v-if直接控制元素的有无来控制元素的渲染

### 列表渲染

v-for

##### key的作用于原理

1.  虚拟dom中key的作用：
    *   key是虚拟dom对象的标识，当状态中的数据发生变化时候，vue会根据新数据生成新的虚拟dom
    *   随后vue进行新的虚拟dom和旧虚拟dom的差异进行比较
2.  对比规则
    *   旧的虚拟dom会找到了和新的虚拟dom相同的key：
        1.  若新虚拟dom的内容没变，就直接使用之前的真实dom
        2.  若新虚拟dom的内容改变了，则生成新的真实dom,替换之前的真实dom
    *   旧的虚拟dom未找到和新虚拟dom相同的key
        1.  直接创建新的真实dom，渲染到页面
3.  使用index作为列表的key可能会导致的问题
    *   对数据逆序添加和删除等破坏顺序的操作，会产生没必要的真实dom更新，界面没问题，但是会效率低

    *   如果包含输入类dom可能导致错位问题

##### 更新导致的小问题

直接修改data中数组的某一项时候页面更新的问题，vue可以监听到对象里面的属性，但是监听不到数组里面的项，列： arr\[0] = xxx 这种写法就无法更新到页面了

那咋办呢？如何能让调整数组里的项也能触发更新呢？

答案：可用数组的方法 如push pop shift unshift splice sort reverse 这7个方法，或者直接用修改后的数组完全替换掉之前的数组，也能触发更新。
其实你使用的上面的7个方法已经被vue包装过了，所以它能够触发视图更新

`$set方法可以给data中的某个对象增加响应式的属性，但是有局限性，不能直接给data加响应式的属性
   $`set方法也可以使数组中的项触发更新，但是用的比较少

##### 总结

1.  vue会监视data对象中所有层次的数据

2.  通过setter实现监视，再new vue 就传入要监测的数据

3.  给对象后追加的属性，vue默认不做响应式处理

4.  如果需要给后加的属性增加响应式，用set或\$set

5.  set和\$set不能给根对象增加属性

6.  修改数组中某一项时候用push,pull,shift,unshift,splice,sort,reverse等方法，或者set和\$set

### 过滤器

定义：对要显示的数据进行特定格式化之后在现实
语法：

1.  注册过滤器：vue.filter(name.callback)或者new vue（filter:{})
2.  用过滤器：{{xxx | 过滤器名称}}
    备注
3.  过滤器可接受额外参数，也可串联
4.  不会改变原始数据

### 自定义指令

这个自定义指令里的函数啥事被调用

1.  指令与元素成功绑定时候(此时还未渲染到页面，有些对元素的操作可能执行失败，留心），这种情况下需用对象式的写法解决
2.  指令所在的模板被重新解析时候，就是data里面数据更新了的时候

##### 函数式

自定义指令有两个参数返回，一个是元素，一个是绑定的信息

    directives: {
     big(element,binding){
        element.innerText = binding,value * 10
     }
    }

列子：上面就是增加一个v-big指令，让数据放大10倍，使用时候就v-big="n"这样就行

##### 对象式

    directives: {
       big: {
          bind(){}, // 指令和元素绑定时候立刻调用
          inserted(){}, // 指令所在元素插入页面时候调用
          update(){} // 指令所在模板被成功解析时候调用
       }
    }

##### 总结

1.  指令名字再元素上可用小写和-连接
2.  指令相关的函数指向都是window,而不是this实例
3.  全局指令用vue.directives('指令名称',{}}，或vue.directives('指令名称',fn{}}
4.  指令定义时候不用加v-,使用时候加v-

### 生命周期

beforeCreate 创建前 无法访问data中的数据和方法
created 创建后 可以访问data中的数据和方法
beforMount 挂载前 vue解析模板，页面还没显示解析好的内容，无法对dom进行操作
mounted 挂载后 页面展示的是被vue解析后的dom
beforUpdate 更新前 数据是新的，页面是旧的，还未进行同步
Updated 更新后 数据同步后
beforDestroy 销毁前 此时方法和data都可用，可做一些解绑自定义事件，关闭定时器的操作
destoryed 销毁后 实例销毁

### 对组件的理解

传统方式存在的问题
1.依赖关系混乱，不好维护
2.代码复用率不高

组件：

1.  理解：用来实现局部特定功能效果的代码集合
2.  为什么： 界面功能复杂
3.  复用代码，结构清晰，利于维护，提高运行效率

vue中使用组件的步骤

1.  定义组件
2.  注册组件
3.  使用组件

如何定义一个组件

1.  使用Vue.extend(options)创建，其中options和nuw Vue时候传入的options几乎一样，但是有以下区别

*
                                          el不要写，因为所有组件都经过vm管理，由vmd的el决定于它使用在哪个容器中
*
                                          data必须写成函数，避免组件复用时候，数据存在引用关系
*
                                          使用telplate可以配置组件结构

如何注册组件
局部注册 new Vue时候传入 components选项
全局注册 写在Vue.component('组件名',组件）

##### 非单文件组件

1.  一个文件中包含多个组件

##### 单文件组件

1.  一个文件就是一个组件

##### 注意点

1.  vue的组件本质上是一个名为VueComponent的构造函数，且不是程序员定义的，而是Vue.extend生成的
2.  我们只需要写<组件名>\</组件名>, vue解析时候会帮我们创造组件的实例对象，也就是Vue帮我们执行
3.  每次调用Vue.extend(),返回的都是一个全新的组件
4.  this的指向，在组件时候，this指的是组件实例对象。在new Vue时候是vue实例对象
5.  一个重要的内置关系 VueCompont.prototype.*proto* === Vue.prototype，这是因为让组件实例对象可以访问到Vue原型上面的属性和方法

### ref属性

1.  被用来给元素或者子组件注册引用信息的
2.  应用在html标签上获取的是真实的dom元素，应用在组建标签上是组件的实例对象
3.  使用方式

*
                                          打标识时候 <组件名 ref="XXX"></组件名>
*
                                          获取： this.$refs.XXX

### props配置

功能：让组件接受外部传进来的数据

1.  传递数据时候写在标签上即可
2.  接受时候有三种方式，

*   只接受
*   限制类型，
*   限制类型，必要性和默认值

1.  props是只读的。vue底层会监测你对props的修改，如果进行的修改会发出警告，如果有修改的需求，可复制一份props的内容在data中一份，然后修改data中的数据

### mixin混入

功能： 可以吧多个组件的公用的配置提取成一个混入对象
使用方式

1.  定义混合
2.  使用混合，局部混合：mixins:\['xxxx']
3.  使用混合，全局混合：Vue.mixin(xxxx)

### 插件

功能：增强vue功能
本质：包含一个install方法的一个对象，install的第一个参数是vue，第二个及以后的是插件使用者传递的数据
定义插件：

    {
       install = function(Vue,options){
          Vue.filter(.....)
          Vue.directive(....)
          Vue.mixin(.....)
          .....
          // 还可以增加实例方法
          Vue.prototype.$myMethod = function(){...}
       }
    }

使用插件：Vue.use(插件名称)

### 组件自定义事件

##### 绑定

子传父

1.  通过父组件定义一个函数，然后用props给子组件传递这个函数,然后在子组件调用这个函数并且传参数，父组件就可以收到数据了！
2.  通过父组件在子组件上绑定一个自定义事件，子组件通过\$emit调用这个绑定事件，同时可以携带参数  --- 这个比较常用
3.  通过给子组件增加ref属性，父组件可以获取子组件的实例， 用`$on来监听自定义事件，使用$`once则只调用一次  --- 更灵活

##### 解绑

通过this.\$off('事件名称') 解绑自定义事件

##### 总结

1.  一种组件之间通讯的方式，适用于子组件传递父组件
2.  使用场景：A是父组件，B是子组件，B想给A传递数据，那么就给在A中给B绑定自定义事件（事件回调在A中）
3.  绑定自定义事件两种方式

*   在父组件中的子组件上用@
*   在父组件写方法，用`$refs.xxx.$`on('事件名称'，携带的参数)
*   若是想要时间只触发一次，用once修饰符或者\$once方法

1.  触发自定义事件： \$emit
2.  解绑自定义事件：\$off
3.  组件上面也可绑定原生dom事件，需要使用native修饰符，如@click.native=
4.  注意：使用`$refs.xxx.$`on触发自定义事件时候，用箭头函数，否则会有this的指向问题，最好直接写在methods中。

### 全局事件总线

1.  是一种组件通讯方式，适用于任意组件之间通讯

2.  安装全局事件总线

    new Vue({
    beforeCreate() {
    Vue.prototype.\$bus = this
    }
    })

3.  使用事件总线
    *   接受数据：A组件想接受数据，那么在A组件给\$bus绑定自定义事件，回调在A组件自身
    *   使用this.`$bus.$`on('xxx',this.demo)接受数据

4.  提供数据时候用this.`$bus.$`emit('xxx',想传递的数据）

5.  用这种方法传递数据时候，最好在beforeDestroy用\$on解绑当前组件用到的事件

### 消息的订阅和发布

1.  一种组件间通信的方式，适用于任意组件通信
2.  使用步骤
    *   安装库 pubsub  npm i pubsub-js
    *   引入库 import pubsub from 'pubsub-js'
    *   接受数据 A组件接受数据，则在A组件订阅消息，回调放在A组件自身              this.pid = pubsub.subscribe('xxxx', ()=>{})
    *   提供数据 pubsub.publish('xxxx', 数据参数）
    *   最好在beforDestory中用PubSub.unsubscribe(pid)取消订阅

### \$nextTick

在下一次dom更新结束后触发回调
用处： 在改变数据之后，基于更新后的dom进行一些操作时候，在nextTick中的回调函数执行

### 插槽

作用：让父组件可以向子组件指定位置插入html结构，也是组件通信的方式，适用于父传子
分类：

1.  默认插槽
2.  具名插槽
3.  作用域插槽，数据在组件的自身，但是数据生成的结构要组件的使用者来决定，通过scope来获取子组件里面的数据

### Vuex

概念：专门在vue中实现集中式状态管理的一个Vue插件，对vue应用中多个组件的共享状态进行集中式管理，也是组件的一种同通信方式，适用于任意组件通信

使用情况：

1.  多个组件依赖同一个状态
2.  来自不同组件的行为需要变更同一状态

四个重要属性：
state: 存放数据，供给其他组件读取或修改的
getters：类似于计算属性，对state中的数据进行加工的
Mutations：里面存放方法，用commit调用的，用于修改state的数据
Actions：里面也是存放方法，可用于异步操作，使用dispatch调用

##### 可模块化加命名空间

目的：代码更好维护，数据更清晰
修改 store.js
namespaced\:true // 开启命名空间

开启命名空间后读取数据
方式一:自己读取
this.\$store.state.模块名称.取的属性名称
方式二：mapState
...mapState('模块名称',\['属性名称','属性名称','属性名称')

开启命名空间后读取getter的数据
方式一：自己读取
this.\$store.getters\['模块名称/getter方法名称']
方式二：mapGetters
...mapGtters('模块名称',\['属性名称'])

开启命名空间后调用disPatch
方式一：自己调用
this.\$store.dispatch('模块名称/方法名称', 希望改成的参数）
方式二：借助mapActions
...mapActions('模块名称',{组件里面的事件名称，store里面的方法名称}）

组件调用commit和dispatch类似

### 路由

vue-router vue的一个插件库，实现spa应用

###### spa应用理解

1.  单页面的web应用
2.  整个页面只有一个完整的页面。index.html
3.  点击页面中的导航不会刷新页面，只会有页面局部更新
4.  数据通过ajax去请求

###### 路由的理解

1.  一个路由就是一组映射关系 key-value
2.  key是路径，value可能是function和component

###### 路由分类

1.  后端路由
    理解： value就是function,用于处理客户端的请求
    工作过程：服务器接收到请求时候，根据请求路径找到匹配的函数处理请求

2.  前端路由
    理解：value是component,用于展示页面内容
    工作过程：当浏览器路径改变时候，对应的组件就会展示

结束router-link作为前端切换

多级路由增加children配置项
跳转时候要写完整路径

###### 路由的props配置

第一种写法：props为对象，对象中的key-value组合会通过props传递给detail组件

第二种写法： props为布尔值true,则吧路由收到的所有params参数通过props传递给detail组件

第三种写法： props为函数，返回的key-value都会通过props传递给detail组件

###### 路由的生命周期钩子

activated: 路由组件激活时候触发
deactivated: 路由组件失活时候触发

###### 全局路由守卫

可以处理路由权限问题

写在ruoter文件里面
全局前置路由守卫-初始化的时候调用，路由切换之前被调用

router.beforEach((to,from,next)=>{
next()
})

回调中有三个参数。to,from,next
to：你跳转的目标路由信息
from :从哪跳转的路由信息
next: 放行

全局后置路由守卫-初始化和路由切换之后调用
fouter.afterEach((to,from)=>{})  没next

###### 独享路由守卫

beforEnter 用法beforEach使用方式一样

注意，独享路由守卫只有beforEnter，没有afterEnter。
独享路由守卫和全局路由守卫可以配合使用

###### 组件内部路由守卫

// 通过路由规则进入该组件时候调用
beforeRouterEnter

// 通过路由规则离开该组件时候调用
beforRouteLeave

### history模式和hash模式

区别：

1.  hash有#号，history无#号
2.  hash兼容性更好
3.  history部署需要后端人员支持

