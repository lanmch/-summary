##JS基础
###JS定义私有属性和共有属性以及静态属性
- 私有属性：通过var声明的属性，成为私有属性，私有属性的作用域仅在当前函数有效，对外不公开，即通过对象/类都无法调用到；
- 共有属性：可以通过实例传输，并且可调用
- 静态属性：无需实例化，就可以调用
```javascript
function User(name,age){
  this.name = name;//公有属性
  this.age = ag
}
User.prototype.getName = function(){//公有方法
  return this.name;
}

function User(name,age){
  var name = name;//私有属性
  var age = age;
  function alertAge(){//私有方法
     alert(age);
  }
  alertAge(age); //弹出26
}

function User(){}
User.age = 26;//静态属性
User.myname = 'fire子海';
User.getName =function(){//静态方法
  
  return this.myname;//如果这里使用this.name，返回的将是User，所有改用了myname，
}
```
###JS数据类型
基本数据类型
- number
- string
- boolean
- null
- undefined

复杂数据类型
- object
	- Object
	- Array
	- function

###JS最大和最小数
`Number.MAX_VALUE `,`Number.MIN_VALUE`
###堆和栈的区别（从如何储存数据来讲）
1. 普通数据类型的对象保存在栈里
2. 对于引用类型，会在栈里定义一个指针，把真正的对象定义在堆里，然后栈中的指针指向堆

###新增array
new Array() and  Array.of
区别：Array.of(10) = [10]  new Array(10) = array.length = 10
###判断Array
- instanceof方法
	- arr instanceof Array
- 构造函数属性
	- [].constructor == Array
- Array.isArray() 
	- Array.isArray([]) //true 
- return Object.prototype.toString.call(arr)

###instanceof原理
只要右边变量的 prototype 在左边变量的原型链上即可。所以，instanceof 在查找的过程中会遍历左边变量的原型链，直到找到右边变量的 prototype，如果查找失败，则会返回 false，告诉我们左边变量并非是右边变量的实例
###类数组对象
JS中，对于一个普通对象来说，所有键名均为正整数，同时拥有length属性，则为类数组对象，例如（arguments）
类数组=>数组： 
```js
Array.prototype.splice.call(arguments);
Array.prototype.slice.call(arguments);
Array.prototype.concat.apply([],arguments);
Array.from(arguments);
```
###arguments
1、arguments像数组，是一个Arguments对象实例。
2、arguments有length属性，代表传给函数的参数个数。
3、可以数组下标访问参数，如arguments[0]，无数组其它方法。
4、存储的是实参不是形参。
5、arguments可以转化为数组（使用类数组转化为数组的方法）


###合并两个对象方法
1. Object.assign(合并的对象，传入合并对象中的对象)
2. 遍历
3. $.extend(obj1,obj2) （jQ的方法）

###如何判断一个对象是否含有某些属性
- for...in 循环遍历对象自身和继承的可枚举属性（不含Symbol属性）
- Object.keys(obj) 返回一个数组，包括对象自身（不含继承的）所有可枚举的属性（不含Symbol属性）
- Object.getOwnPropertyNames(obj) 返回一个数组，包含对象自身的所有属性，不含Symbol属性，包括不可枚举属性
- Object.getOwnPropertySymblos(obj) 返回一个数组，包含对象自身的所有Symbol属性
- Reflect.ownKeys(obj)返回一个数组，包含对象所有的自身属性。

###document.ready()和window.onload()区别
- document.ready()是DOM树结构绘制完毕就执行（文件不一定加载完） <=先执行
- window.onload()是在所欲元素加载完才执行


###浅拷贝与深拷贝
- 浅拷贝
	- `=`
- 深拷贝
	- 数组：`slice`方法或`concat`方法（截取或拼接）
	- 对象：循环遍历赋值给新对象  或  先将对象`JSON.stringify()`再赋值再`JSON.parse()` 
###序列化
序列化指将对象转化为字符串方便存储
- `JSON.stringfy()`序列化
- `JSON.parse()`还原

注意：**如果对象中键值有为`underfine`，则序列化的时候会舍弃这对键值，还原也没了**

###js普通事件与绑定事件的区别
- 普通事件直接触发事件，多次重写会覆盖
- 事件绑定可以在同一个元素监听同一事件多次（使用`addEventListener`方法）

###defer async
- 都没有，浏览器会立即加载并执行指定脚本文件（不等待后续载入的文档元素，读到就加载执行）
- async，加载和渲染后续文档和js加载异步执行（但是js加载完毕就会执行脚本了）
- defer，加载后续文档与js加载异步执行（但js脚本会等文档加载完毕才执行，在DOMContentLoaded事件触发之前完成）
