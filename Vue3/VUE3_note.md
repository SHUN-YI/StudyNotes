# Vue3 学习笔记

## 1.Vue3简介

*   2020年9月18日，Vue.js发布3.0版本，代号：One Piece（海贼王）
*   耗时2年多、[2600+次提交](https://github.com/vuejs/vue-next/graphs/commit-activity)、[30+个RFC](https://github.com/vuejs/rfcs/tree/master/active-rfcs)、[600+次PR](https://github.com/vuejs/vue-next/pulls?q=is%3Apr+is%3Amerged+-author%3Aapp%2Fdependabot-preview+)、[99位贡献者](https://github.com/vuejs/vue-next/graphs/contributors)
*   github上的tags地址：<https://github.com/vuejs/vue-next/releases/tag/v3.0.0>

## 2.Vue3带来了什么

### 1.性能的提升

*   打包大小减少41%
*   初次渲染快55%, 更新渲染快133%
*   内存减少54%   ......

### 2.源码的升级

*   使用Proxy代替defineProperty实现响应式
*   重写虚拟DOM的实现和Tree-Shaking   ......

### 3.拥抱TypeScript

*   Vue3可以更好的支持TypeScript

### 4.新的特性

1.  Composition API（组合API）    
    *   setup配置
    *   ref与reactive
    *   watch与watchEffect
    *   provide与inject
    *   ......
2.  新的内置组件
    *   Fragment
    *   Teleport&#x20;
    *   Suspense
3.  其他改变    
    *   新的生命周期钩子
    *   选项应始终被声明为一个函数
    *   移除keyCode支持作为 v-on 的修饰符&#x20;
    *   ......

## 3.创建Vue3.0工程

### 1.使用 vue-cli 创建

官方文档：<https://cli.vuejs.org/zh/guide/creating-a-project.html#vue-create>

```bash
## 查看@vue/cli版本，确保@vue/cli版本在4.5.0以上
vue --version
## 安装或者升级你的@vue/cli
npm install -g @vue/cli
## 创建
vue create vue_test
## 启动
cd vue_test
npm run serve
```

### 2.使用 vite 创建

官方文档：<https://v3.cn.vuejs.org/guide/installation.html#vite>

vite官网：<https://vitejs.cn>

*   什么是vite？—— 新一代前端构建工具。
*   优势如下：  &#x20;
    1.  开发环境中，无需打包操作，可快速的冷启动
    2.  轻量快速的热重载（HMR）。
    3.  真正的按需编译，不再等待整个应用编译完成。

```bash
## 创建工程
npm init vite-app <project-name>
## 进入工程目录
cd <project-name>
## 安装依赖
npm install
## 运行
npm run dev
```

## 4.常用 Composition API

官方文档: <https://v3.cn.vuejs.org/guide/composition-api-introduction.html>

### 1.拉开序幕的setup

1.  Vue3.0中的一个新配置项，值为一个函数
2.  setup是所有组合式Api表演的舞台
3.  组件中使用的数据和方法都要配置在setup中
4.  setup函数有两种返回值

    *   返回一个对象，则对象中的属性和方法在模板中均可直接使用。（常见）
    *   返回一个渲染函数，可自定义渲染的内容。（少见）
5.  注意点

    *   尽量不要和vue2混用
    *   vue2配置（data，menhos，computed）中可以访问到vue3的setup中的属性和方法，但是vue3中无法访问到vue2中的配置
    *   如果有重名情况，按照vue3里setup为准
    *   setup不能是一个async函数，因为返回值就不再是一个return的对象了，而是promise。模板就无法获取到return对象中的属性

### 2.ref函数

1.  作用：定义一个响应式的数据
2.  语法：`cost xxx = ref(initValue)`

    *   创建了一个包含响应式数据的**引用对象**，即reference对象，简称ref对象
    *   在js中操作数据：`xxx.value`
    *   模板中读取数据不需要.value，直接`<div>{{xxx}}</div>`
3.  备注：

    *   接受的数据类型可以是基本类型，也可以是对象类型。
    *   基本类型的数据：响应式原理依然是依靠Object.defineProperty()中的get和set完成的
    *   对象类型的数据：内部依靠Vue3中的一个新函数-reactive函数。

### 3.reactive函数

1.  作用：定义一个对象类型的响应式数据（基本类型不要用它）
2.  语法： `cons xxx = reactive(initObject)` 接受一个对象或数组，返回一个代理对象Proxy对象
3.  reactive定义的响应式对象式深层次的
4.  内部基于ES6的Proxy实现，通过代理对象操作源对象内部数据进行操作

### 4.Vue3的响应式原理

1.  Vue2的响应式原理：

    *   通过`Object.defineProperty()`对属性的读取和修改进行数据劫持
    *   数组类型贼是通过重写更新数组的一系列方法来实现拦截。--对数组的变更方法进行了包裹

        ```javascript
        Object.defineProperty(data,'count', {
         get(){}
         set(){}
        }
        ```
    *   存在问题：

        1.  新增属性，删除属性界面不会更新。通过set方法触发更新
        2.  直接通过下标修改数组，界面也不会自动更新
2.  Vue3的响应式原理

    *   通过Proxy（代理）拦截对象中任意属性变化，包括属性值的读写，删除，添加等。
    *   通过Reflect（反射）对源对象的属性进行操作
    *   Proxy：<https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Proxy>
    *   Reflect：<https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect>

        ```javascript
        new Proxy(data, {
         // 拦截读取属性
         get(target,prop){
          return Reflect.get(target.prop)
         },
        // 拦截设置属性或者是新增属性
         set(target,prop,value){
           return Reflect.set(target,prop,value)
         },
        // 拦截删除属性
         deletProperty(target,prop){
           return Reflect.deletProperty(target.prop)
         }
        })
        ```

### 5. reactive对比ref

1.  从定义数据的角度对比

    *   ref用来定义**基本类型数据**
    *   reactive用来定义**对象或者数组类型数据**
    *   ref也能定义对象或数组能行数据，内部都活通过reactive转为代理对象
2.  原理对比

    *   ref通过`Object.definProperty()`的get和set来实现数据响应式
    *   reactive通过Proxy对象来实现响应式，并通过Reflect操作源对象的内部数据
3.  使用角度对比：

    *   使用ref定义的数据操作的使用.value来获取值，模板中读取不需要.value
    *   使用reactive定义的数据则都不需要.value

### 6. setup的注意点

1.  执行时机：在beforCreate之前执行一次，this是undefined
2.  setup的参数

    *   第一个参数是props对象，包含外部组件传递过来的，且组件内部声明接收的属性
    *   第二个参数是context对象，上下文对象，其中包含了

        1.  attrs：值是对象，包含组件外部传递过来，但是没有在props配置中声明的属性，相当于`this.$attrs`
        2.  solt：收到的插槽内容，相当于`this.$solts`
        3.  emit：分发自定义事件的函数，相当于`this.$emit`

### 7.计算属性和监视

1.  conputed函数

    *   和Vue2中的computed配置功能一致
    *   写法

        ```javascript
        import {computed} from 'vue'
        setup(){
          // 数据
          let person = reactive({
            firstName:'',
            lastName:''
          })
          // 计算属性简写-接受函数
          let name = computed(()=>{
            return person.firstName + person.lastName
          })
          // 计算属性完整写法-接受对象
          let name = computed({
              get(){
                 return person.firstName + '-' + person.lastName
              }
              //有修改计算属性的情况,value是修改后的值
              set(value){
                 const nameArr = value.split('-')
                 person.firstName = nameArr[0]   
                 person.lastName = nameArr[1]
              }
          })
        }
        ```
2.  watch函数

    *   与Vue2中的watch配置功能一致
    *   两个注意点

        1.  监视reactive定义的数据时候，oldValue无法正确获取，获取的还是新数据
        2.  默认开启深度监视，deep配置失效
        3.  监视reactive定义的响应式数据的某个属性时候，deep配置有效

            ```javascript
            let sum = ref(1)
            let msg= ref('哈哈哈')
            let person = reactive({
               name: '小李',
               age: 12,
               job: {
                work: 12
              }
            })
            //情况一：监视ref定义的响应式数据   
            watch(sum,(newValue,oldValue)=>{
                  console.log('sum变化了',newValue,oldValue)   
            },{immediate:true})    

            //情况二：监视多个ref定义的响应式数据   
            watch([sum,msg],(newValue,oldValue)=>{
                 console.log('sum或msg变化了',newValue,oldValue)
            })

            // 情况三：监视reactive定义的响应式数据,若watch监视的是reactive定义的响应式数据，则无法正确获得oldValue！！若watch监视的是reactive定义的响应式数据，则强制开启了深度监视
            watch(person,(newValue,oldValue)=>{
                 console.log('person变化了',newValue,oldValue)   
            },{immediate:true,deep:false}) //此处的deep配置不再奏效

            //情况四：监视reactive定义的响应式数据中的某个属性
            watch(()=>person.job,(newValue,oldValue)=>{
                 console.log('person的job变化了',newValue,oldValue)
            },{immediate:true,deep:true})

            //情况五：监视reactive定义的响应式数据中的某些属性，即多个属性
            watch([()=>person.job,()=>person.name],(newValue,oldValue)=>{
                 console.log('person的job变化了',newValue,oldValue)
            },{immediate:true,deep:true})

             //特殊情况
            watch(()=>person.job,(newValue,oldValue)=>{
                   console.log('person的job变化了',newValue,oldValue)
            },{deep:true}) //此处由于监视的是reactive素定义的对象中的某个属性，所以deep配置有效 
              
            ```
3.  watchEffect函数

    *   watch函数套路是：既要指明监视的属性，也要指明监视的回调
    *   watchEffect则不用指明监视的属性。回调中用到了哪个属性就监视哪个属性。
    *   watchEffect有些类似于计算属性computed

        1.  计算属性computed注重计算出来的值，及函数返回值。
        2.  watchEffect则比较注重过程，不写返回值

            ```javascript
            //watchEffect所指定的回调中用到的数据只要发生变化，则直接重新执行回调。
            watchEffect(()=>{
                   const x1 = sum.value
                   const x2 = person.age
                   console.log('watchEffect配置的回调执行了')
            })   
            ```

### 8.生命周期

1.  Vue3中可以继续使用Vue2的生命周期钩子，最后两个被更名了
2.  beforDestroy改名为beforUnmount
3.  destroy改名为unmount
4.  Vue3中也提供了Composition API形式的生命周期钩子

    ```javascript
    `beforeCreate`===>`setup()`
    `created`=======>`setup()` 
    `beforeMount` ===>`onBeforeMount`
    `mounted`=======>`onMounted`
    `beforeUpdate`===>`onBeforeUpdate`
    `updated` =======>`onUpdated`
    `beforeUnmount` ==>`onBeforeUnmount`
    `unmounted` =====>`onUnmounted`
    ```

### 9.自定义hook函数

1.  什么是hook:本质是一个函数，把setup中使用到的Composition API进行了封装
2.  类似vue2的mixin(混入）
3.  自定义hook的优势: 复用代码, 让setup中的逻辑更清楚易懂。

### 10.toRef函数

1.  作用：创建一个ref对象，其value的值指向对象中的某个属性
2.  语法：`const name = toRef(person,'name')`
3.  应用场景：把响应式对象中的某个属性单独提供给外部使用
4.  拓展：`toRefs`和`toRef`功能一致，但是前者克批量创建多个ref对象
5.  语法：`toRefs(person)`

## 5.不常用的Composition API

### 1. shallowRef与shallowReactive

1.   shallowReactive：只处理对象最外层属性的响应式（浅响应式）
2.  &#x20;shallowRef：只处理基本数据类型的响应式, 不进行对象的响应式处理
3.  什么时候使用?   -  如果有一个对象数据，结构比较深, 但变化时只是外层属性变化 ===> shallowReactive
4.  如果有一个对象数据，后续功能不会修改该对象中的属性，而是生新的对象来替换 ===> shallowRef

### 2.readonly与shallowReadonly

1.   readonly: 让一个响应式数据变为只读的（深只读)
2.  &#x20;shallowReadonly：让一个响应式数据变为只读的（浅只读）
3.  &#x20;应用场景: 不希望数据被修改时

### 3.toRow与makeRow

1.  toRaw：作用：将一个由\`\`\`reactive\`\`\`生成的**响应式对象**转为**普通对象**
2.  使用场景：用于读取响应式对象对应的普通对象，对这个普通对象的所有操作，不会引起页面更新。
3.  markRaw：作用：标记一个对象，使其永远不会再成为响应式对象
4.  应用场景:     1. 有些值不应被设置为响应式的，例如复杂的第三方类库等，当渲染具有不可变数据源的大列表时，跳过响应式转换可以提高性能。

### 4. customRef

1.  建一个自定义的 ref，并对其依赖项跟踪和更新触发进行显式控制。

    ```javascript
    <template>
       <input type="text" v-model="keyword">    
        {{keyword}}
    </template> 
    <script>
    import {ref,customRef} from 'vue'
    export default {
      name:'Demo', 
       setup(){
       // let keyword = ref('hello')
       //使用Vue准备好的内置ref
       //自定义一个myRef
       function myRef(value,delay){  
          let timer
          //通过customRef去实现自定义
          return customRef((track,trigger)=>{
             return{
                 get(){ 
                     track() //告诉Vue这个value值是需要被“追踪”的
                     return value               
                 },
                 set(newValue){                 
                     clearTimeout(timer)
                     timer = setTimeout(()=>{
                       value = newValue
                       trigger() //告诉Vue去更新界面
                     },delay)
                 }
             }
         })
      }
      let keyword = myRef('hello',500) //使用程序员自定义的ref
      return {
    	 keyword
       }	
      }
    }
    </script>
    ```

### 5.provide和inject

1.  作用：实现组件间通讯
2.  套路：父组件有一个 \`provide\` 选项来提供数据，后代组件有一个 \`inject\` 选项来开始使用这些数据 
3.  写法：想传递的组件

    ```javascript
    setup(){
      let car = reactive({name:'奔驰',price:'40万'}) 
      provide('car',car)
    }     
    ```
4.  想接受的组件

    ```javascript
    setup(props,context){
       const car = inject('car')
       return {car}
    } 
    ```

### 6.响应式数据判断

1.  isRef: 检查一个值是否为一个 ref 对象 
2.  isReactive: 检查一个对象是否是由 \`reactive\` 创建的响应式代理 
3.  isReadonly: 检查一个对象是否是由 \`readonly\` 创建的只读代理
4.  isProxy: 检查一个对象是否是由 \`reactive\` 或者 \`readonly\` 方法创建的代理

### 7.composition API优势

1.  Options API 存在的问题 使用传统OptionsAPI中，新增或者修改一个需求，就需要分别在data，methods，computed里修改 。
2.  Composition API 的优势 我们可以更加优雅的组织我们的代码，函数。让相关功能的代码更加有序的组织在一起。

## 6.新的组件

### 1.Fragment

在Vue2中: 组件必须有一个根标签 - 在Vue3中: 组件可以没有根标签, 内部会将多个标签包含在一个Fragment虚拟元素中 - 好处: 减少标签层级, 减小内存占用

### 2.Teleport

Teleport是一种能够将我们的**组件html结构**移动到指定位置的技术。   `vue   <teleport to="移动位置">`    

### 3.Suspence

1.  等待异步组件时渲染一些额外内容，让应用有更好的用户体验
2.  异步引入组件

    ```javascript
    import {defineAsyncComponent} from 'vue'
    const Child = defineAsyncComponent(()=>import('./components/Child.vue')) 
    ```
3.  使用`Suspense`包裹组件，并配置好`default`与`fallback`

    ```html
    <template>      
      我是App组件
      <Suspense>
        <template v-slot:default>
          <Child/>
        </template>
        <template v-slot:fallback>            
           加载中.....
        </template>
      </Suspense>      
    </template> 
    ```

## 7.其他

1.  全局API的转移
2.  Vue 2.x 有许多全局 API 和配置。例如：注册全局组件、注册全局指令等。

    ```javascript
    Vue.component('MyButton', {
       data: () => ({         count: 0       }), 
       template: '<button @click="count++">Clicked {{ count }} times.</button>'
    })
    //注册全局指令 Vue.directive('focus', {       inserted: el => el.focus()     }    
    ```
3.  Vue3.0中对这些API做出了调整：   - 将全局的API，即：Vue.xxx调整到应用实例（app）上 
4.  data选项应始终被声明为一个函数
5.  **移除**keyCode作为 v-on 的修饰符，同时也不再支持`config.keyCodes`
6.  **移除**v-on.native修饰符,父组件中绑定事件





































