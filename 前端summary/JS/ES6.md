##es6
###babel原理
babel就是把浏览器不支持的代码编译成支持的代码（如ES6->ES5）
babel工作运行主要有三个阶段，分别是解析、转换、生成。
解析：将代码字符串解析成抽象语法树(AST)
转换：对AST树进行遍历转译
生成：根据变换后的抽象语法树再生成代码字符串

举个例子：比如要把人民币转化为美元，就是将人民币先拿到银行（看你有多少钱），在进行汇率运算，最后根据汇率生成对应的美元

###let、var、const区别
 - var声明变量存在变量提升（提升到作用域顶部），let和const不会
 - let、const 的作用范围是块级作用域，而var的作用范围是函数作用域
 - 同一作用域下let和const不能声明同名变量，而var可以
 - 同一作用域下在let和const声明前使用会存在暂时性死区
 - const
   - 一旦声明必须赋值,不能使用null占位
   - 声明后不能再修改
   - 如果声明的是复合类型数据，可以修改其属性

###Symbol数据类型
`Symbol()`主要用于解决属性名冲突的属性
比如同一个对象obj，A对其添加属性a，B也想对其添加属性a，如果不适用Symbol会产生覆盖问题
```
console.log(Symbol('a')==Symbol('a')) // false
```

###Set去重
`new Set()`数组去重，返回一个对象（或者说是类数组），再使用`Array.from()`进行对象到数组的转换

###es6新增
https://blog.csdn.net/wsymcxy/article/details/82913756
- 箭头函数
- 字符串模板（用反引号引用，变量使用${变量名}）
- 解构赋值
- rest参数`...`
- 变量定义方式
- 引入class类
- 引入super和extend关键字 


###class
####原理
https://www.jianshu.com/p/86267fab4878
在constructor构造方法，用来接收参数。定义方法与constructor同级，不加function
####class和function的区别
```
1. class不能声明同名函数，会报错；function声明同名函数会覆盖
2. class声明没有函数提升，而function有
3. class定义的类没有私有方法和私有属性
```

###forin forof forEach map区别
`forin`(更适合遍历对象，尽量不要遍历数组)
a. 遍历数组，遍历的是索引
b. 遍历的顺序可能不是数组本身的顺序
c. 会遍历数组内所有可以枚举的属性（如arr=[1,2];arr.name='haha';遍历时还会枚举出haha
`forof`
a.es6标准，遍历的是value
b.for of不能遍历对象，因为能够被for...of正常遍历的，都需要实现一个遍历器Iterator。而数组、字符串、Set、Map结构，早就内置好了Iterator（迭代器），它们的原型中都有一个Symbol.iterator方法，而Object对象并没有实现这个接口，使得它无法被for...of遍历。
`forEach`
a. 不会生成新数组，也不会改变原数组，仅仅适合一些遍历操作
`map`
a. 会返回一个新数组，但是原数组保持不变

###箭头函数与普通函数区别
- 箭头函数为匿名函数，不能作为构造函数（不能new）
- 箭头函数不能绑定`arguments`，需要用`...`解决参数问题
```js
let C =（...c) =>{
	console.log(c)
}
C(1,2,3) // [1,2,3]
```
- `this`指向箭头函数定义的环境
- 箭头函数没有`prototype`属性

###Promise
####为什么有Promise
- 解决回调地狱
- 同时管理成功回调和失败回调
####Promise含义
Promise对象用于一个异步操作最终结果的表示，它将传统的回调变为链式结构。简单来说，就是用于处理一步操作，处理成功就执行成功操作，处理失败就捕获错误或者停止后续操作。
Promise有三种状态，分别是pending、resolved、rejected。状态只能由pending向resolved转化或者pending向rejected转化，转化是单向的并且无法由resolved向rejected转化。
Promise包含了许多方法
- then方法，then()调用后返回一个Promise对象，意味着实例化后的Promise对象可以进行链式调用。并且then方法可以接收两个函数，一个是处理成功后的函数，一个是处理错误结果的函数。
- catch方法，catch()和then()方法一样，都会返回一个新的Promise对象，主要用于捕获异步操作出现的异常，因此我们常常省略then方法的第二个参数，把错误处理控制权转交给其后面的catch函数
- all方法，all()接收一个参数，这个参数必须可以迭代（如数组），只有所有的状态均为fulfilled，最终才会变为fulfilled，否则变为rejected
- race方法，race()方法和all类似，都接受一个可以迭代的参数，不同的是race的状态变化是一旦参数内有一个值的状态发生改变，该Promise的状态就是改变的状态
####（原生实现）手撕Promise
```
function PromiseM(){
    this.status='pending';
    this.msg='';
    var process=arguments[0];
    var that=this;
    process(function(){
        that.status='resolve';
        that.msg=arguments[0];
    },function(){
        that.status='reject';     
        that.msg=arguments[0];           
    });
    return this;
}
PromiseM.prototype.then=function(){
    if(this.status=='resolve'){
        arguments[0](this.msg);
    }
    if(this.status=='reject'&&arguments[1]){
        arguments[1](this.msg);
    }
}
``` 

###async和await(它的实现是基于Promise)
####定义
- async用来声明函数为异步，定义的函数会return一个promise对象，可以使用then方法添加回调函数
- await用于async定义的函数内部，其后面可以跟任意表达式，但是主要的一图是用来等待promise对象的状态被完成（如果await的是promise对象会造成会进行等待，如果是正常表达式则立即执行）

####例子
```js
async function f() {
    let promise = new Promise((resolve, reject) => {
        setTimeout(() => resolve('done!'), 1000)
    })
    let result = await promise // 直到promise返回一个resolve值（*暂停）
    alert(result) // 'done!' 
}
```

###Generator
####原理
generator生成器视为可以暂停和恢复的代码段，代码在执行的过程中可以交出控制权。其中yield是generator可以自行暂停，同时他也可以通过yield接收输入和发送输出。
####例子
```js
function *foo(x) {
  let y = 2 * (yield (x + 1))
  let z = yield (y / 3)
  return (x + y + z)
}
let it = foo(5)
console.log(it.next())   // => {value: 6, done: false}
console.log(it.next(12)) // => {value: 8, done: false}
console.log(it.next(13)) // => {value: 42, done: true}
```
 - 首先 Generator 函数调用和普通函数不同，它会返回一个迭代器

 - 当执行第一次 next 时，传参会被忽略，并且函数暂停在 yield (x + 1) 处，所以返回 5 + 1 = 6

 - 当执行第二次 next 时，传入的参数等于上一个 yield 的返回值，如果你不传参，yield 永远返回 undefined。此时 let y = 2 * 12，所以第二个 yield 等于 2 * 12 / 3 = 8
**重点：这儿其实就是指第二次传参会把第一次的yield整体代替了**
 - 当执行第三次 next 时，传入的参数会传递给 z，所以 z = 13, x = 5, y = 24，相加等于 42
- 对象中，value代表所得值，done代表是否执行到末尾return处
- 如果再next，返回{value: undefined, done: true}

