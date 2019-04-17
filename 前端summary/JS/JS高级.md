#JS高级
##this
### this的指向有哪几种情况？
this代表函数调用相关联的对象，通常页称之为执行上下文。

1. 作为函数直接调用，非严格模式下，this指向window，严格模式下，this指向undefined；
2. 作为某对象的方法调用，this通常指向调用的对象。
3. 使用apply、call、bind 可以绑定this的指向。
4. 在构造函数中，this指向新创建的对象
5. 箭头函数没有单独的this值，this在箭头函数创建时确定，它与声明所在的上下文相同。

### 如果对一个函数进行多次 bind，那么上下文会是什么呢？

```js
let a = {}
let fn = function () { console.log(this) }
fn.bind().bind(a)() // => ?
```
不管我们给函数 bind 几次，fn 中的 this 永远由第一次 bind 决定，所以结果永远是 window。
```js
// fn.bind().bind(a) 等于
let fn2 = function fn1() {
  return function() {
    return fn.apply()
  }.apply(a)
}
fn2()
```

### 多个this规则出现时，this最终指向哪里？
首先，new 的方式优先级最高，接下来是 bind 这些函数，然后是 obj.foo() 这种调用方式，最后是 foo 这种调用方式，同时，箭头函数的 this 一旦被绑定，就不会再被任何方式所改变。
###bind、apply、call共同点和区别
- `bind：`返回一个函数，函数为传递的第一个参数，执行需要调用这个函数
```javascript
	var name = 'ddd'
    var a = {
        name:'luoshushu',
        fn:function(){
            console.log(this.name) 
        }
    }
    var obj = {
        name:'李四'
    }
    a.fn() //luoshushu。 this指向a对象
    console.log(a.fn.bind(window)) //f(){ console.log(this.name) } 
    console.log(a.fn.bind(obj)) //f(){ console.log(this.name) }
    a.fn.bind(window)() //ddd。this指向window（所以调用bind后需要加一个括号()才可以执行
    a.fn.bind(obj) ()//李四。this指向obj
```
- `call：`call方法调用一个函数，其具有一个指定的this值和分别地提供的参数（参数列表）
`fun.call(thisArg,arg1,age2,...)`
	**注意：**
	- 不传，或者传null、undefined，函数中的this指向window对象 
	- 传递另一个函数的函数名，函数中this指向这个函数的引用。 
	- 传递字符串、数值和布尔类型等基础类型，函数中的this指向其对应的包装对象，如：srting - number boolean 
	- 传递一个对象，函数中的this指向对象
- `apply：`apply方法调用一个函数，其具有一个指定的this值，参数为数组或者类数组的对象
`fun.apply(thisArg,[argsArray])`

**共同点和区别：**
- 均用于改变this指向
- call式逐个传递参数，而apply式传递参数组成的数组
- bind是返回对应的函数，另外两个是立即执行，所以bind回调执行的时候使用的bind()方法


##EventLoop
###为什么JavaScript是单线程？
JavaScript语言的一大特点就是单线程，也就是说，同一个时间只能做一件事。那么，为什么JavaScript不能有多个线程呢？这样能提高效率啊。

JavaScript的单线程，**与它的用途有关**。作为浏览器脚本语言，JavaScript的主要用途是**与用户互动，以及操作DOM**。这决定了它只能是单线程，否则会带来很复杂的同步问题。比如，假定JavaScript同时有两个线程，一个线程在某个DOM节点上添加内容，另一个线程删除了这个节点，这时浏览器应该以哪个线程为准？

所以，为了避免复杂性，从一诞生，JavaScript就是单线程，这已经成了这门语言的核心特征，将来也不会改变。

为了利用多核CPU的计算能力，**HTML5提出Web Worker标准**，允许JavaScript脚本**创建多个线程**，但是**子线程完全受主线程控制**，**且不得操作DOM**。所以，这个新标准并没有改变JavaScript单线程的本质。
###web/service worker
- 它本质上是JS上一个全新的线程，运行在与主线程不同的上下文。
- servicework线程被设计成异步，所以主线程中同步的API，不能再service worker中使用（如localstorage、XMLHttpRequest）。
- 主线程用于负责DOM，service worker作为子线程不能访问DOM
- 用途：
	- 它能够用来和服务器沟通数据（sw内置了fetch和push API）
	- 离线缓存
	- 消息推送
	- 后台消息传递
###Event Loop详解
参考地址:[Event Loop 这个循环你晓得么？(附 GIF 详解)-饿了么前端](https://zhuanlan.zhihu.com/p/41543963)
事件循环机制执行顺序：
1. js解析方法时，将同步任务排队到执行栈中，异步任务排队到事件队列中。
2. 事件队列分为:
宏任务：setTimeout，setInterval，setImmediate，I/O，UI交互事件
微任务：process.nextTick，Promise.then
3. 浏览器环境中执行方法时，先将执行栈中的任务清空，再将微任务推到执行栈中并清空，之后检查是否存在宏任务，若存在则取出一个宏任务，执行完成检查是否有微任务，以此循环…
4. Event Loop在浏览器与node环境中的区别：
浏览器环境每次执行一个宏任务，再去检查微任务
node会清空当前所处阶段的队列，即执行所有task，再去检查微任务
**说白了，一句话：执行顺序：同步任务->微任务->宏任务**
```js
console.log(1);
    
 setTimeout(() => {
   console.log('setTimeout');
 }, 0);

 let promise = new Promise(resolve => {
   console.log(3);
   resolve();
 }).then(data => {
   console.log(100);
 }).then(data => {
   console.log(200);
 });
    
 console.log(2);
 // 1 3 2 100 200 setTimeout
```

##作用域与闭包
###变量提升和函数提升
- 变量提升：变量提升将变量声明提升到它所在的作用域顶部
- 函数提升：函数式声明会将函数提升到顶部
###作用域与作用域链
**作用域**就是变量与函数的可访问范围
当一个函数创建时，实际将函数的环境以及外部环境以一个对象的形式有序保存起来，这样局部作用域其实就好像一级一级得往上扣起来，也就是**作用域链**。
作用域链的第一个对象就是函数内部的作用域，然后依次第二个第三个就是父级环境以此循环

###上下文执行栈
**执行上下文**顾名思义是指程序代码执行时候的环境

程序开始执行时会进入全局上下文，全局上下文是执行上下文栈的第一个元素，当全局上下文执行过程中调用了一个函数，则该函数的执行上下文就会push到执行上下文栈中，当该函数又调用其他函数时，会进入新的执行上下文，并被push到栈中，当函数调用完成时会退出该执行上下文，进入外层的执行上下文继续执行，所以栈底始终是全局上下文，栈顶始终是当前程序正在运行的执行上下文。
###上下文执行栈和作用域链的区别（结合JS执行过程）
js代码执行
- 编译阶段
	- 语法分析
	- 可执行代码生成
	- **作用域确定**
- 执行阶段
	- **进入执行上下文生命周期（执行上下文栈）**
		- 创建阶段
			- 生成变量对象（arguments/检查function声明/检查var声明）
			- **建立作用域链**
			- 确定this指向
		- 执行阶段
			- 变量赋值
			- 函数引用
	- 代码执行
	- 垃圾回收

###闭包
回答方法：从JS的执行开始讲、一个函数执行前后的内存回收过程，再阐述闭包形成的原因，这样又会导致什么问题，应该怎么解决，最后再讲有什么用途。 
a.因为JS中函数执行前后会有对变量的内存分配和回收问题（函数声明时会产生一个独立的作用域） 
b.所以导致在部分函数作用域无法调用另一函数作用域中变量的情况，因此引发出闭包这个问题。 
c.所谓闭包，其定义就是有权访问另一个函数作用域中变量的函数，说白了就是函数可以访问他们所在外部函数的变量。 
d.闭包的作用就是可以读取函数内部的变量，以及让这些变量永远保存在内存中（如果不手动清除） 
e.而创建闭包的方法就是在函数内部定义另一个匿名函数并return出去 
f.闭包可以是局部变量一直储存在内存中，同时避免全局变量污染 
g.而创建闭包可能会产生内存泄漏问题，因为闭包所形成的变量会一直保存在内存中，所以清除闭包使用：将用完的函数或者变量置为null

####闭包实例
a. 使setTimeout传递的第一个函数可以带参数
b. 封装变量，通过return得到值

判断权限状态（其实可以不使用闭包直接写代码，但是因为用函数模块化开发更加符合标准，只要有函数就闭包了）
```js
function isFirstLoad(){
            var list=[];
            return function(option){
                if(list.indexOf(option)>=0){ //检测是否存在于现有数组中，有则说明已存在
                    console.log('已存在')
                }else{
                    list.push(option);
                    console.log('首次传入'); //没有则返回true,并把这次的数据录入进去
                }
            }
        }

var ifl=isFirstLoad();
ifl("zhangsan"); // 首次
ifl("lisi"); // 首次
ifl("zhangsan"); // 存在

```
##内存泄漏和垃圾处理
###内存泄漏
**定义：**
应用程序不再需要占用内存的时候，由于某些原因，内存没有被操作系统或可用内存池回收。
**造成原因：**
 - 未使用 var 声明的全局变量
 - 闭包函数
 - 控制台日志(console.log)
 - 移除存在绑定事件的 DOM 元素(IE)
 - 被遗忘的计时器或回调函数

###垃圾处理
**定义：**
垃圾回收机制就是，js有垃圾收集器会周期性地执行去清除未使用变量所占用的内存 
**常见方式：** 
`标记清除 `
是当变量进入环境时，将这个变量标记为“进入环境”。当变量离开环境时，则将其标记为“离开环境”。标记“离开环境”的就回收内存。 
工作流程： 
垃圾回收器，在运行的时候会给存储在内存中的所有变量都加上标记。 
去掉环境中的变量以及被环境中的变量引用的变量的标记。 
再被加上标记的会被视为准备删除的变量。 
垃圾回收器完成内存清除工作，销毁那些带标记的值并回收他们所占用的内存空间 
`引用计数 `
跟踪记录每个值被引用的次数 
工作流程： 
声明了一个变量并将一个引用类型的值赋值给这个变量，这个引用类型值的引用次数就是1。 
同一个值又被赋值给另一个变量，这个引用类型值的引用次数加1. 
当包含这个引用类型值的变量又被赋值成另一个值了，那么这个引用类型值的引用次数减1. 
当引用次数变成0时，说明没办法访问这个值了。 
当垃圾收集器下一次运行时，它就会释放引用次数是0的值所占的内存。


##原型与继承
###创建对象方法与区别
####方法
- 字面量创建
- 构造函数创建
- Object.create()

####new和Object.create()区别
- 前者创建的是Object，后者创建的是Function
- Object.create()不会继承Object.prototype原型链上的属性和方法 
- Object.create是内部定义一个对象，并且让F.prototype对象 赋值为引进的对象/函数 o，并return出一个新的对象
```javascript
Object.create =  function (o) {
    var F = function () {};
    F.prototype = o;
    return new F();
};
```
- new做法是新建一个obj对象o1，并且让o1的proto指向了Base.prototype对象。并且使用call 进行强转作用环境。从而实现了实例的创建
```javascript
var o1 = new Base()
//实质
var o1 = new Object();
o1.[[Prototype]] = Base.prototype;
Base.call(o1);
```


###原型与原型链
####原型
- JavaScript 的所有对象中都包含了一个 `__proto__` 内部属性，这个属性所对应的就是该对象的原型
 - JavaScript 的函数对象，除了原型 `__proto__` 之外，还预置了 prototype 属性
 - 当函数对象作为构造函数创建实例时，该 prototype 属性值将被作为实例对象的原型 `__proto__`。

####原型链
个人理解：
由于js里面万物皆对象，并且`__proto__`属性是所有对象都有的属性，所以因为`__proto__`和`prototype`的指向关系，会形成一条链条，可以递归访问`__proto__`属性，并且最终值为null
- 任何一个实例对象通过原型链可以找到它对应的原型对象，原型对象上面的实例和方法都是实例所共享的。
- 一个对象在查找以一个方法或属性时，他会先在自己的对象上去找，找不到时，他会沿着原型链依次向上查找。

**注意：** 函数才有`prototype`，实例对象只有`__proto__`， 而函数有的`prototype`是因为函数是Function的实例对象

####原型与原型链相关重要结论
1. 实例的构造函数属性`constructor`指向构造函数
2. 原型对象`prototype`的构造函数属性指向构造函数（可以把原型对象看作构造函数的一个实例）
3. 实例对象的`__proto__`属性指向构造函数的`prototype`

###继承

####属性拷贝：就是将对象的成员复制一份给需要继承的对象
```javascript
var superObj = {
  name: 'Li',
  age: 25,
  friends: ['小明', '小李', '小赵'],
  showName: function(){
    alert(this.name);
  }
}

// 创建需要继承的子对象
var subObj = {};
// 开始拷贝属性(使用for...in...循环)
for( var i in superObj ){
  subObj[i] = superObj[i];
}

console.log(subObj)
console.log(superObj)
存在问题：
如果继承过来的成员是引用类型的话,
那么这个引用类型的成员在父对象和子对象之间是共享的,
也就是说修改了之后, 父子对象都会受到影响.
```

####原型式继承：借用构造函数的原型对象实现继承
```javascript
// 创建父构造函数
function SuperClass(name){
  this.name = name;
  this.showName = function(){
    alert(this.name);
  }
}
// 设置父构造器的原型对象
SuperClass.prototype.showAge = function(){
  console.log(this.age);
}
// 创建子构造函数
function SubClass(){
}
//置子构造函数的原型对象实现继承
SubClass.prototype = SuperClass.prototype;

var child = new SubClass()
存在问题：
父构造函数的原型对象和子构造函数的原型对象上的成员有共享问题
只能继承父构造函数的原型对象上的成员, 不能继承父构造函数的实例对象的成员
```

#### 原型链继承：即子构造函数.prototype = new 父构造函数
```javascript
// 创建父构造函数
function SuperClass(){
    this.name = 'liyajie';
    this.age = 25;
    this.showName = function(){
        console.log(this.name);
    }
}
// 设置父构造函数的原型
SuperClass.prototype.friends = ['小名', '小强'];
SuperClass.prototype.showAge = function(){
    console.log(this.age);
}
// 创建子构造函数
function SubClass(){

}
// 实现继承
SubClass.prototype = new SuperClass();
// 修改子构造函数的原型的构造器属性
SubClass.prototype.constructor = SubClass;

var child = new SubClass();
console.log(child.name); // liyajie
console.log(child.age);// 25
child.showName();// liyajie
child.showAge();// 25
console.log(child.friends); // ['小名','小强']

// 当我们改变friends的时候, 父构造函数的原型对象的也会变化
child.friends.push('小王八');
console.log(child.friends);["小名", "小强", "小王八"]
var father = new SuperClass();
console.log(father.friends);["小名", "小强", "小王八"]
问题：
不能给父构造函数传递参数，父子构造函数的原型对象之间有共享问题
```
####借用构造函数：使用call和apply借用其他构造函数的成员, 可以解决给父构造函数传递参数的问题, 但是获取不到父构造函数原型上的成员.也不存在共享问题
```javascript
// 创建父构造函数
function Person(name){
  this.name = name;
  this.freinds = ['小王', '小强'];
  this.showName = function(){
     console.log(this.name);
  }
}

// 创建子构造函数
 function Student(name){
  // 使用call借用Person的构造函数
  Person.call(this, name);
 }

 // 测试是否有了 Person 的成员
 var stu = new Student('Li');
 stu.showName(); // Li
 console.log(stu.friends); // ['小王','小强']
```

####组合继承：借用构造函数+原型式继承
```javascript
// 创建父构造函数
function Person(name,age){
    this.name = name;
    this.age = age;
    this.showName = function(){
        console.log(this.name);
    }
}
// 设置父构造函数的原型对象
Person.prototype.showAge = function(){
    console.log(this.age);
}
// 创建子构造函数
function Student(name){
    Person.call(this,name);
}
// 设置继承
Student.prototype = Person.prototype;
Student.prototype.constructor = Student;
//为什么要修改construct指向，因为如果不修改，那么实例函数就变成了父函数的实例而不是子函数的实例
最好方法
唯一缺点：调用了两次父类构造函数，生成了两份实例（子类实例将子类原型上那份屏蔽了）
```

####寄生组合继承
```javascript
function Cat(name){
  Animal.call(this);
  this.name = name || 'Tom';
}
(function(){
  // 创建一个没有实例方法的类
  var Super = function(){};
  Super.prototype = Animal.prototype;
  //将实例作为子类的原型
  Cat.prototype = new Super();
})();

// Test Code
var cat = new Cat();
console.log(cat.name);
console.log(cat.sleep());
console.log(cat instanceof Animal); // true
console.log(cat instanceof Cat); //true
```

##节流和防抖
###节流
当持续触发事件时，在规定时间段内只能调用一次回调函数。如果在规定时间内又触发了该事件，则什么也不做,也不会重置定时器
场景
拖拽
监听滚动时间判断是否到页面底部自动加载更多
###防抖
指触发事件后在规定时间内回调函数只能执行一次，如果在规定时间内又触发了该事件，则会重新开始算规定时间。
场景：
每次resize/scroll触发统计的场景
文本输入验证（连续输入文字后发送ajax请求验证，验证一次就好）

##设计模式
https://segmentfault.com/a/1190000010914032
https://www.jianshu.com/p/c62229477fc7
- 单例模式
保证一个类仅有一个实例，并且提供一个访问它的全局访问点（调用一个类，任何时候返回的都是同一个实例），比如：全局缓存、登录状态
实现方法：使用一个变量来标志当前是否已经为某个类创建过对象，如果创建了，则在下一次获取该类的实例时，直接返回之前创建的对象，否则就创建一个对象。

- 工厂模式
定义一个用于创建对象的接口，这个接口由子类决定实例化哪一个类。该模式使一个类的实例化延迟到了子类。而子类可以重写接口方法以便创建的时候指定自己的对象类型。
比如：
类的设计者设计了羊，狗，猫3个类
类的使用者要创建“羊一”，“狗二”，“猫三”3个对象。
如果不使用工厂模式，类的使用者就要用羊，狗，猫3个类。
而如果使用工作模式，类的使用者通过工厂就可以创建“羊一”，“狗二”，“猫三”这3个对象。这里要求一个基类“动物类”。
动物类必须有羊，狗，猫3个类统一的行为，比如“吃东西”。这是类的使用者关心的。
而“吃东西”的具体行为是什么样的，羊，狗，猫可以互不相同。这是类的设计者关心的。
那么，把羊，狗，猫3个类隐藏起来不让类的使用者知道有什么好处呢？
答：减少类的使用者的负担。


- 发布订阅者模式
定义了一种一对多的依赖关系，让多个订阅者对象同时监听某一个主题对象。这个主题对象在自身状态变化时，会通知所有订阅者对象，使它们能够自动更新自己的状态。
比如
一个网站有好几个模块都要获取用户信息然后渲染。如果没用这种设计模式，我们就要处理ajax回调，然后写代码去渲染。但是如果新增一个模块就需要去修改之前的ajax回调，增加新增部分。所以团队开发比如ajax是A做的，新增模块是B做的，A不知道B具体要做什么，A就只需要说明拿到了用户信息，提供一个接口供其他人来订阅，在A完成之后，告诉之前这些跟他打过招呼的模块，传递这些有用的信息，具体使用与否或者使用哪些由他们决定

- 代理模式（es6 proxy）
为其他对象提供一种代理以控制对这个对象的访问。
代理模式使得代理对象控制具体对象的引用。代理几乎可以是任何对象：文件，资源，内存中的对象，或者是一些难以复制的东西。
**虚拟代理图片预加载**
如果直接给某个 img 标签节点设置 src 属性，由于图片过大或者网络不佳，图片的位置往往有段时间会是一片空白。常见的做法是先用一张loading 图片占位，然后用异步的方式加载图片，等图片加载好了再把它填充到 img 节点里，这种场景就很适合使用虚拟代理。


- 中介者模式
通过统一接口来维护一对多关系，且通信者之间不需要知道彼此之间的关系，只需要约定好API即可
比如说：
一个购买界面很多input或者select框，用户操作后要实时返回用户的操作，如果把这些内容都写到操作的回调中，其实不利于程序的扩展，如果以后新增某个选项，需要在每个输入框的回调中增加这个新的框的判断
所以这个时候应该把这些逻辑代码分离，让中介者去判断，如果有新的增加，只需要更改中介者的业务代码。当input或select触发回调事件，只需要向中介者发送请求，告诉它变化了，中介者收到后就会做出处理
