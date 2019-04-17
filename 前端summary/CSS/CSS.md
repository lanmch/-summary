[TOC]
#前端面经CSS
##基础定义
###选择器类型
important|style|id|class&属性&伪类|伪元素|通配符
:-:|:-:|:-:|:-:|:-:|:-:
10000|1000|100|10|1|0
###盒模型
包括content padding border margin
####标准盒模型
width = content
####IE盒模型
width = content + padding + border
####区别与转化
box-sizing: content-box / border-box
###position
常见属性： static  relative  absolute  fixed
不常见属性： sticky  inherit  inital unset
tips: css如果最开始设置了position，js操作position值无效
###em和rem区别
em是基于使用em元素的字体使用大小
rem是基于根元素的字体大小
###块元素和行元素
###单行省略和多行省略
```css
.dan{
	overflow: hidden;
    text-overflow:ellipsis; //文本溢出显示省略号
	white-space:nowrap;
}
.duo{
	display: -webkit-box;    
	-webkit-box-orient: vertical;    
	-webkit-line-clamp: 3;    // 行数
	overflow: hidden;
}
```
##link和@import区别
1. link是一个标签，不仅可以加载CSS，还可以用来定义rel属性等，import只能加载CSS
2. link兼容性更高（标签），import是在CSS 二点几之后才提出，低版本浏览器不支持
3.  link引入css会在页面加载时同时进行（CSS tree），而import要等文档加载完毕才会去加载
4.  link引入的css可以通过js操作DOM改变样式，而import不能
##CSS哪些属性可以被继承
1. 字体相关：font-family/font-size/font-style/font-weight
2. 文本相关：text-align/text-indent/color
3. 列表相关：list-style(image/position...)
##CSS预处理器有哪些？有什么特性
sass(scss)/less
主要特性：相当于给css增加了编程的特性，方便维护、让css代码更简洁、更有可读性
##隐藏元素方法和区别
- display: none
- visibility: hidden
- opacity: 0

区别：
1. display: none会使元素脱离文档流，另外两个不会
2. 因此display: none会引起回流而另外两个引起重绘
3. display： none的子元素无法通过display: block显示而另外两个可以
##清除浮动
```css
// 定义伪类after和zoom
.clearfloat:after{display:block;clear:both;content:"";visibility:hidden;height:0}
.clearfloat{zoom:1}
// 加空标签clear:both
// 定义height
// 父元素overflow为hidden（多余被隐藏）或auto（多余出现滚动条）
```
##布局
###圣杯布局与双飞翼布局
###flex布局
###grid布局
https://www.html.cn/archives/8506
##垂直水平居中
```css
// 水平对齐+行高对齐
.test {
        text-align: center;
        line-height: 100px;
}
// 水平+垂直对齐
.parent {
    display: table-cell;
    text-align: center;
    vertical-align: middle;
}
.child {
    display: inline-block;
}
// 相对+绝对定位
.parent {
    position: relative;
}
.child {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    width: 80px;
    margin: auto;
}
// flex布局
// 相对自身平移，transform属性
.parent{
	position: relative;
}
.child{
	position: absolute;
	top: 50%;
	left: 50%;
	transform: translate(-50%,-50%)
}
```
##动画
###js动画和css动画的差别
1. js动画干扰性能，可能存在丢帧
2. js动画代码长，css相对简单
3. js动画效果相对丰富
4. js动画兼容性更强
##BFC
BFC，块级格式化上下文
主要的约束规则：
1. 内部盒子会在垂直方向上一个接一个放置
2. 同属一个BFC的两个盒子会发生margin重叠
3. 每个元素会与BFC盒子左边框重合
4. BFC区域不与浮动区域重叠
5. BFC会撑起父元素高度
6. BFC不会影响外部，外部也不会影响内部
形成条件：
1. 开启浮动
2. visibility不为visible
3. display的值为table-cell、table-caption、inline-block
4. position的值为fixed或absolute
##CSS实现响应式方式
1. 流动布局，各个位置浮动
2. 媒体查询
3. flex布局
4. grid布局
###CSS实现圆
border-radius
###CSS实现三角形
```css
div{
    width: 0;
    height: 0;
    border-top: 10px solid blue;
    border-left: 0;
    border-right: 0;
    border-bottom: 0;
}
```