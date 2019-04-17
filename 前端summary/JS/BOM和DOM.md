##BOM与DOM
###BOM
BOM 是 browser object model 的缩写， 简称浏览器对象模型。 主要处理浏览器窗口相关
它描述了与浏览器进行交互的方法和接口， 可以对浏览器窗口进行访问和操作， 譬如可以弹出新的窗口， 回退历史记录， 获取 url……
#### BOM 对象包含哪些内容？
- `Window` JavaScript 层级中的顶层对象， 表示浏览器窗口。
- `Navigator` 包含客户端浏览器的信息。
- `History `包含了浏览器窗口访问过的 URL。
- `Location` 包含了当前 URL 的信息。
- `Screen` 包含客户端显示屏的信息。

#### 检测浏览器版本版本有哪些方式？
 - **根据 window.navigator.userAgent** // UA.toLowerCase().indexOf('chrome')
 - 根据 window 对象的成员 // 'ActiveXObject' in window

####offsetWidth/offsetHeight,clientWidth/clientHeight 与 scrollWidth/scrollHeight 的区别
 - offsetWidth/offsetHeight 返回值包含 content + padding + border，效果与 e.getBoundingClientRect()相同
 - clientWidth/clientHeight 返回值只包含 content + padding，如果有滚动条，也不包含滚动条
 - scrollWidth/scrollHeight 返回值包含 content + padding + 溢出内容的尺寸

###DOM
####操作DOM节点方法
```js
document.getElementById(id);           //返回对拥有指定id的第一个对象进行访问 
document.getElementsByName(name);      //返回带有指定名称的节点集合 
document.getElementsByTagName(tagname);   //返回带有指定标签名的对象集合 
document.getElementsByClassName(classname);  //返回带有指定class名称的对象集合 
document.createElement(eName);  //创建一个节点 
document.createAttribute(attrName); //对某个节点创建属性 
document.createTextNode(text);   //创建文本节点 
document.insertBefore(newNode,referenceNode);  //在某个节点前插入节点 
parentNode.appendChild(newNode);        //给某个节点添加子节点 cloneNode(true | false);  //复制某个节点  参数：是否复制原节点的所有属性 
parentNode.removeChild(node);  //删除某个节点的子节点 node是要删除的节点 
parentNode.replaceChild(newChild,oldChild) //替换某个节点 
```
####DOM事件流
三个阶段：事件捕获->处于目标阶段->事件冒泡
####事件冒泡与事件捕获
捕获从上到下， 冒泡从下到上。
先捕获，再到目标，再冒泡
**事件冒泡：**即事件开始时由最具体的元素接收，然后逐级向上传播至根节点
**事件捕获的具体流程**
从window -> document -> html -> body -> ... -> 目标元素
####阻止事件冒泡和默认事件
- event.preventDefault()
   - 阻止默认行为
- event.stopPropagation()
   - 阻止事件冒泡
- return false
	 - 同时阻止
####事件委托（使用event.target获取具体信息）
事件委托是指将事件绑定目标元素的到父元素上，利用冒泡机制触发该事件
优点：
 - 可以减少事件注册，节省大量内存占用
 - 可以将事件应用于动态添加的子元素上

但使用不当会造成事件在不应该触发时触发
```js
ulEl.addEventListener('click', function(e){
  var target = event.target || event.srcElement;
  if(target && target.nodeName.toUpperCase() === "LI"){
    console.log(target.innerHTML);
  }
}, false);
```
####attribute和property关系与区别
- attribute  基本返回值是字符串或者null
	- 只包含标签上的所有属性
- property  返回值可以是字符串、布尔值、对象等
	- 是dom元素作为对象所有附加内容（比如什么childNodes、firstNode）
### BOM 与 DOM 的关系
BOM包含DOM，所以document是window的属性