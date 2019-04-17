
#HTML
##DOCTYPE作用以及标准模式与兼容模式区别
`<!DOCTYPE html>`
用于声明文档类型和DTD规范，位于首行，不是HTML标签，用于告诉浏览器解析器以什么文档标准来解析这个文档。（若不存在DOCTYPE或者格式错误会导致文档以兼容模式呈现）
`标准模式`
标准模式的排版和JS运作都是以W3C标准运行
`兼容模式`
页面以宽松的向后兼容的方式显示，以浏览器自己的方式解析，模拟老式浏览器行为，以防站点无法工作
##HTML全局属性
全局属性就是所有HTML共有的属性；
它可以作用于所有元素，即使某些属性对某些元素不起作用

##行内元素、块级元素、空元素
定义：CSS规范规定，每个元素都有对应的display属性，而空元素指的是没有内容的标签（比如img它就没有内容，是通过属性获得内容的）
- 块元素：div ul ol li dl dt dd p h1 h2...  
- 行元素：a b span img input select strong textarea
- 空元素：br hr img input link meta...
###img为行标签为何宽高还能起作用？
因为这样可设置宽高的行内元素被称为可替换元素，可替换元素就是浏览器根据元素的标签和属性，来决定元素的具体显示内容。
- textare
- input
##link和@import导入样式的区别
- link 是XHTML标签，除了加载CSS还能用于定义RSS、义 rel 连接属性等作用；而@import是 CSS 提供，只能用于加载CSS
- 页面加载的同时， link 会同时被加载；而@import 需要等到页面完全载入后才加载
- link 无兼容性问题， @import 是CSS后提出，低版本浏览器可能不支持
- link 支持用 JS 控制 DOM 改变样式，而@import 不支持

##浏览器内核的理解
浏览器内核主要分为两个部分：渲染引擎和 JS 引擎
`渲染引擎`
负责取得网页的内容（HTML），整理讯息（载入 CSS ），以及计算网页的显示方式然后渲染
`JS引擎`
解析和执行 js 来实现逻辑和控制 DOM 进行交互
`常见浏览器内核`
- IE （IE 内核）
- 火狐（Gecko）
- 谷歌/ Safari （webkit）
- Opera（Presto）

##HTML5变化
- 增加语义化元素
	- header footer nav main mark article section
	- 删除了一些纯样式标签，如：big font center（这些完全就是用于展示）
- 表单增强属性，如：url number color time...
- 新增API
	- 音视频（audio，video）
	- 图形（canvas）
	- 实时通信（websocket）
	- 本地储存（webStorage）

##HTML语义化的理解
- 语义化让页面内容更加结构化，结构更清晰，便于对浏览器、搜索引擎解析
- 即使没有CSS，也以一种文档格式显示，容易阅读
- 搜索引擎爬虫也依赖于 HTML 标记来确定上下文和各个关键字的权重，利于 SEO
- 方便后续维护，可读性较高


##em 与 i 的区别
- 效果均为斜体
- em 是语义化标签，表示强调
- i 是样式标签表示斜体

##单标签元素
input、img、br、hr、meta、link

##HTML和DOM的关系
- HTML只是一个字符串
- DOM由HTML解析而来
- JS可以维护DOM

##cookie, localStorage 和sessionStorage区别
###共同点
- 都是保存在浏览器端，且同源

###不同点
- cookie无论是否需要，都会在http请求中携带，增加不必要的资源消耗；而storage仅保存在本地
- cookie大小限制一般为4k，而webstorage一般大小为5m
- 数据时效期不同，sessionStorage仅在当前浏览器窗口关闭有效，localStorage始终保存在内存中，cookie需要手动设置保存
- cookie和localstorage在所有同源窗口均可共享，而sessionStorage仅在当前浏览器的当前页面（或者子页面）共享使用
- storage已经被封装好了，可以直接调用，cookie需要自己封装

##HTML 中 title 属性和 alt 属性
title 是鼠标放在上面是显示的值
alt 是信息不输出的时候显示的值

##为什么现在放弃了 table 标签
table 的缺点在于服务器把代码加载至本地，本来就是一行一行执行，但是 table 标签里的东西**需要全部下载完了之后才会加载**，影响网页效果

##网页基本信息
- 文档标题`<title>`
- 编码格式`<meta charset='utf-8'>`
- 视窗设置`<meta name="viewport" content="width=device-width, initial-scale=1.0">`
- 搜索引擎优化相关内容`<meta name="description" content=“帮助你深层次了解HTML文档结构”>`
- IE 浏览器版本渲染设置`<meta http-equiv="X-UA-Compatible" content="ie=edge">`
###viewport各属性值以及意义
width|height|initial-scale|maxinum-scale|minumun-scale|user-scalable
-|-|-|-|-|-
device-width设备宽度|与width类似|初始缩放比例|最大缩放比|最小缩放比例|是否可以手动缩放


##外链文件
- css 文件： `<link>`标签
- js 文件： `<script>`标签
为了网页性能，我们把CSS文件一般放到顶部，JS 文件放到底部（因为CSS不阻止DOM解析而JS会阻止）

##canvas和svg的区别
1. canvas时h5提供的新的绘图方法 
svg已经有了十多年的历史
2. canvas画图基于像素点，是位图，如果进行放大或缩小会失真 
svg基于图形，用html标签描绘形状，放大缩小不会失真
3. canvas需要在js中绘制 
svg在html中绘制
4. canvas支持颜色较svg多
5. canvas无法对已经绘制的图像进行修改、操作 

##echarts原理
基于JS的canvas开发
##浏览器兼容性
**1.** 不同浏览器的标签默认的margin和padding不同
通配符：*{margin: 0; padding: 0}
**2.** 块属性标签float后，又有横向的margin情况下，在IE6显示margin比设置的大
在float的标签样式控制中加入display:inline;转化为行内属性

3. 设置较小高度标签(一般小于10px)，在IE6、IE7，遨游中高度超出设置高度值
给超出高度的标签设置overflow:hidden;或者设置行高line-height小于你设置的高度

**4.** 图片默认有间距(几个img标签放在一块，有些浏览器会有默认的间距，有通配符也不起作用)
使用float属性为img布局
**5.** 字体大小定义不同(对字体大小small定义不同，Firefox为13px，而IE为16px，差别比较大)
使用指定的字体大小如14px或者使用em


##如何判断浏览器类型
`navigator.userAgent`
## 移动端项目需要注意的4个问题

### meta中设置viewport
阻止用户手滑放大或缩小页面，需要在 index.html中添加meta元素,设置viewport。
```html
<meta name="viewport" content="width=device-width,initial-scale=1.0,minimum-scale=1.0,maximum-scale=1.0,user-scalable=no">
```

### CSS样式统一问题
我们需要重置页面样式，因为在不同的手机浏览器上，默认的css样式不是统一的。 解决方法：使用reset.css重置所有元素的默认样式

### 一像素边框问题
https://www.cnblogs.com/lhb25/p/seven-method-for-1px-retina-screen.html
有的手机分辨率比较高，是2倍屏或3倍屏，手机上的浏览器就会把CSS中的1像素值展示为2个或3个物理宽度 
解决方法： 利用**scroll缩放的原理**将边框重置。当我们需要使用一像素边框时只需要在标签上添加对应类名，如设置底部一像素边框就在标签上加入"border-bottom"的class名
- 小数0.5px+媒体查询（兼容性不好只有ios8+支持）
- 伪类 + transform 实现
原理是把原先元素的 border 去掉，然后利用 :before 或者 :after 重做 border ，并 transform 的 scale 缩小一半，原先的元素相对定位，新做的 border 绝对定位。
```css
.scale-1px{
  position: relative;
  border:none;
}
.scale-1px:after{
  content: '';
  position: absolute;
  bottom: 0;
  background: #000;
  width: 100%;
  height: 1px;
  -webkit-transform: scaleY(0.5);
  transform: scaleY(0.5);
  -webkit-transform-origin: 0 0;
  transform-origin: 0 0;
}
```


###300毫秒点击延迟问题
在移动端开发中，某些机型上使用click事件会延迟300ms才执行，这样影响了用户体验。 解决方法： 引入[fastclick.js](https://www.jianshu.com/p/05b142d84780)。



