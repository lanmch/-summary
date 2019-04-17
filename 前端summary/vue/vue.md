#vue
##vue引发的生态
- vuex
- vue-router
- vue-cli

##vue
###vue1和vue2的区别
https://www.jianshu.com/p/7e937be99d11
1. **模板**组件中，不支持片段代码，vue2中template必须有一个根元素来包住所有代码
2. **生命周期钩子**变化，比如：移除了v1的beforeCompile，替换ready为mounted
3. **循环参数变化**，v-for，v1是（key，value）遍历需加track-by="$index"（不然不绑定重复数据），v2是（Value，key）
4. **绑定HTML**，v1是{{{}}}，v2是v-html 

###v-bind和v-model区别
- v-bind用于绑定数据和属性
- v-model实现双向绑定，在表单外不起作用

####v-model指令实现原理
v-model实际是一个语法糖，他主要是通过v-bind绑定响应式数据，再出发input事件并传递数据。说白了就是在输入框绑定一个input事件(addEventListener)，input事件在用户输入时出发，触发时获取元素的value值然后再去赋值
```html
<input type="text" id="ipt1">
<input type="text" id="ipt2">
<script>
    var ipt1=document.getElementById('ipt1');
    var ipt2=document.getElementById('ipt2');
    ipt1.addEventListener("input",function(){
        ipt2.value=ipt1.value;
    })
</script>
```
###mvc和mvvm
`mvc`
v-model实际是一个语法糖，他主要是通过v-bind绑定响应式数据，再出发input事件并传递数据。说白了就是在输入框绑定一个input事件(addEventListener)，input事件在用户输入时出发，触发时获取元素的value值然后再去赋值
```html
<input type="text" id="ipt1">
<input type="text" id="ipt2">
<script>
    var ipt1=document.getElementById('ipt1');
    var ipt2=document.getElementById('ipt2');
    ipt1.addEventListener("input",function(){
        ipt2.value=ipt1.value;
    })
</script>
```
###mvc和mvvm
####什么是mvc
所有通信都是单向的：提交一次反馈一次，通信一次相互制约
- 视图界面(View)传送指令到控制器(Controller)
- 控制器完成业务逻辑，改变模型状态(Model)
- 模型进行数据保存，将新的数据发送到视图界面(View)

`mvvm`
- view和model关联起来称为viewmodel
- viewmodel负责把model的数据同步到view中显示出来，并且还负责把view修改同步到model中
- 各部分通信皆双向，view和model不发送直接联系，都是通过viewmodel传递

**mvvm就是model view viewmodel的缩写，它是通过viewmodel分别双向连接view和model，从而达到数据同步的效果**

`区别`
主要就是mvc中Controller演变成mvvm中的viewModel。mvvm主要解决了mvc中大量的DOM 操作使页面渲染性能降低，加载速度变慢，影响用户体验。和当 Model 频繁发生变化，开发者需要主动更新到View 。

###双向绑定
####原理
https://segmentfault.com/a/1190000006599500?utm_source=tag-newest
总的来说，vue实现双向数据绑定的原理就是利用了 Object.defineProperty() 这个方法重新定义了对象获取属性值(get)和设置属性值(set)的操作来实现的。
首先要对数据进行劫持监听，所以我们需要设置一个监听器Observer，用来监听所有属性。如果属性发上变化了，就需要告诉订阅者Watcher看是否需要更新。因为订阅者是有很多个，所以我们需要有一个消息订阅器Dep来专门收集这些订阅者，然后在监听器Observer和订阅者Watcher之间进行统一管理的。接着，我们还需要有一个指令解析器Compile，对每个节点元素进行扫描和解析，将相关指令对应初始化成一个订阅者Watcher，并替换模板数据或者绑定相应的函数，此时当订阅者Watcher接收到相应属性的变化，就会执行对应的更新函数，从而更新视图。
1. 定义一个数据监听器Observer，能够对数据对象的所有属性进行监听，若果有变动可以拿到最新数据并通知订阅者
2. 定义一个指令解析器Compile，对每个元素节点的指令进行扫描和解析，根据指令模板替换数据，以及绑定相应的更新函数
3. 实现一个Watcher，作为连接Observer和Compile的桥梁，能够订阅并受到每个属性变动的通知，执行更新函数

###proxy和defineProperty区别
Object.defineProperty主要问题
a. 不能监听数组变化（push/pop/shift/unshift/splice/sort/reverse无法触发set方法，vue通过对这些方法重写来实现数组劫持）
b. 必须遍历对象的每个属性，配合Object.keys()方法进行遍历（返回自身属性数组），相当于是多了一层嵌套
c. 必须深层遍历嵌套的对象，指当一个对象为深层嵌套时，必须逐层遍历，知道把每个对象的属性都调用了Object.defineProperty方法位置

Proxy
a. 它不需要对数组的方法进行重写
b. 它针对的是整个对象，而不是某个属性，不需要遍历了
c. 支持嵌套，get里面会递归调用Proxy并返回

###vue列表通过索引赋值具有响应式吗，为什么（vue如何监听数组变异）
不会，因为数组不能通过defineProperty进行数据劫持，所以需要通过push/pop等函数来修改数组即可，或者使用vue.set()函数修改数组
Vue.set(target, key, value)
target：要更改的数据源（对象或数组）
key：要更改的具体数据（一般为索引）
value：重新赋的值

###v-for原理
其实就是通过Object.keys()来遍历对象，并进行结果的渲染，但是Object.keys()它不能保证他的遍历结果是按原来顺序的 所以需要增加一个key字段，比如{2:a,1:b}遍历结果为1,2

####key字段作用
因为v-for在更新已经渲染的元素列表时，采用就地复用策略（A,B,C..在工厂线，然后B请假C顶替B，D顶替C..而不是重新给所有人排序分配任务）
key来给每个节点做一个唯一标识，使用diff算范更高效地更新虚拟DOM。（key标识节点的时候，可以在插入新节点快速并高效地找到插入位置）

###jQuery与框架区别
1. 单向数据绑定和双向数据绑定
2. jQuery是使用$选取DOM对象，然后对其进行赋值、取值、事件绑定等操作，其实就是相对于原生更方便的选取和操作对象，数据和界面还是在一起的。
3. 框架主要是将视图层和数据层分离开了。对数据操作不需要引用相应的DOM对象。

###vue和react区别
**相似**
都是js框架、都是使用了虚拟DOM去映射真实DOM、都有路由，状态管理工具等、都采用组件式开发
**不同**
1. vue是MVVM模式而React严格上只是MVC模式
2. 组件写法不同，vue采用的是单个组件汇集html、css、js的模板形式，而react是采用jsx的方式构建组件
3. 数据绑定不同，vue是实现了数据的双向绑定，而react的数据流动是单向的
4. 虚拟DOM渲染不同，react当应用状态改变时会对全部组件重新渲染而vue会根据每个组件的依赖关系，不需要重新渲染整个组件树
5. state对象在vue中不是必须的，数据有data属性管理；而react中state对象不可变，需要使用setState方法更新状态

###vue生命周期
vue的生命周期总共分为8个
- 创建前/后： 在 beforeCreate 阶段，vue 实例的挂载元素 el 还没有。
- 载入前/后：在 beforeMount 阶段，vue 实例的$el 和 data 都初始化了，但还是挂载之前为虚拟的 dom 节点，data.message 还未替换。在 mounted 阶段，vue 实例挂载完成，data.message 成功渲染。
- 更新前/后：当 data 变化时，会触发 beforeUpdate 和 updated 方法。
- 销毁前/后：在执行 destroy 方法后，对 data 的改变不会再触发周期函数，说明此时 vue 实例已经解除了事件监听以及和 dom 的绑定，但是 dom 结构依然存在

###组件传值
###父传子
- 父组件标签上绑定属性，如：`<dad msg='dad'></dad>`
- 子组件通过`props`接收数据，如：`<child>{{msg}}</child>  props:['msg']`
####子传父
- 子组件通过$emit传参，如：`this.$emit('key',value)`
- 父组件通过v-on接收，如：`<child v-on:key='getChild'></child> getChild(data){console.log(data)}`

####兄弟组件互传
构建eventbus.js进行传值
- 发送方`bus.$emit('key',value)`
- 接收方`bus.$on('key',(data)=>{console.log(data)})`
###路由跳转
- 静态声明式 `<router-link to='/'>`
- 动态编程式`this.$router.push('/')`

####参数传递
发送|接收
-|-
`this.$router.push({path:'/detail/${id}'})`|`this.$route.params.id`
`this.$router.push({name:'detail', params:{id:data}})`|`this.$route.params.id`
`this.$router.push({path:'/detail', query:{id:data}})`|`this.$route.query.id`

###自定义指令
####自定义指令钩子函数
- bind 只调用一次，指令第一次绑定到元素时调用（初始化动作）
- insterted 被绑定元素插入父节点时调用
- update 被绑定元素所在的模板更新时调用，而不论绑定值是否变化。
- componentUpdated 被绑定元素所在模板完成一次更新周期时调用。
- unbind 只调用一次， 指令与元素解绑时调用
####如何自定义
- 全局
```js
Vue.directive('focus',{
	XXX : function(el){
		//run code
	}
})
```
- 局部
```js
directives:{
	focus:{
		//run code
	}
}
```

###hash模式和history模式区别
- hash——即地址栏中的#符号，锚点，hash出现在URL中，但不会重新加载页面
- history——对历史记录栈进行修改，执行时修改URL单不会向后端发送请求
####history报错解决
服务器端需要进行配置，相当于在服务器端增加一个覆盖所有情况的候选资源，当URL匹配不到任何资源，就返回同一个html页面（这个页面就是app依赖的页面）
但是此时，将无法收到404页面，前端就需要做一个404页面，在routes里面加上path:'*',name:'notfoundcomponent',component:notfoundcomponent


###虚拟DOM原理

#### 为什么虚拟DOM能提升性能（为什么虚拟DON快）
- 虚拟DOM其实就是一个JavaScript对象。通过这个JavaScript对象来描述真实DOM。
- 采用虚拟DOM的话，减少了实际DOM操作次数，当数据变化的时候，只需要局部刷新变化的位置就好了

#### diff算法


##vuex
###vuex是什么？如何使用？什么场景使用？
- 它是vue 框架中状态管理。在 main.js 引入 store，注入。
- 新建了一个目录 store，并 export 。
- 场景有：单页应用中，组件之间的状态。音乐播放、登录状态、加入购物车

###vuex常见属性
五种，分别是`state`,`getter`,`mutation`,`action`,`module`
###mutation和action区别
1. mutation专注于修改state，理论上时修改state的唯一途径；而action适用于执行业务代码，进行异步请求（也可同步，mutation只能同步）
2. action提交的时mutation，不是直接进行状态的变更
###vuex的store特性
1、vuex有哪几种属性？
答：有五种，分别是 State、 Getter、Mutation 、Action、 Module


2、vuex的State特性是？
答：
一、Vuex就是一个仓库，仓库里面放了很多对象。其中state就是数据源存放地，对应于与一般Vue对象里面的data
二、state里面存放的数据是响应式的，Vue组件从store中读取数据，若是store中的数据发生改变，依赖这个数据的组件也会发生更新
三、它通过mapState把全局的 state 和 getters 映射到当前组件的 computed 计算属性中


3、vuex的Getter特性是？
答：
一、getters 可以对State进行计算操作，它就是Store的计算属性
二、 虽然在组件内也可以做计算属性，但是getters 可以在多组件之间复用
三、 如果一个状态只在一个组件内使用，是可以不用getters


4、vuex的Mutation特性是？
答：
一、Action 类似于 mutation，不同在于：
二、Action 提交的是 mutation，而不是直接变更状态。
三、Action 可以包含任意异步操作



5、Vue.js中ajax请求代码应该写在组件的methods中还是vuex的actions中？
答：
一、如果请求来的数据是不是要被其他组件公用，仅仅在请求的组件内使用，就不需要放入vuex 的state里。
二、如果被其他地方复用，这个很大几率上是需要的，如果需要，请将请求放入action里，方便复用，并包装成promise返回，在调用处用async await处理返回的数据。如果不要复用这个请求，那么直接写在vue文件里很方便。


6、不用Vuex会带来什么问题？
答：
一、可维护性会下降，你要想修改数据，你得维护三个地方

二、可读性会下降，因为一个组件里的数据，你根本就看不出来是从哪来的

三、增加耦合，大量的上传派发，会让耦合性大大的增加，本来Vue用Component就是为了减少耦合，现在这么用，和组件化的初衷相背。