# CSS的学习

## 介绍一下BFC及应用

`BFC`特性:

1. 内部box会在垂直方向，一个接一个地放置。
2. Box垂直方向的距离由`margin`决定，在一个BFC中，两个相邻的块级盒子的垂直外边距会产生折叠。
3. 在BFC中，每一个盒子的左外边缘(`margin-left`)会触碰到容器的左边缘(`border-left`)(对于从右到左的格式来说，则触碰到右边缘)
4. 形成了BFC的区域不会与`float box`重叠
5. 计算BFC高度时，浮动元素也参与计算

生成BFC的还有:行内块元素、网格布局、contain值为layout、content或strict的元素等。

BFC的应用：

1. 利用特性4可以实现左图右文的效果：

```html
<body>
  <img src="img">
  <p>文字</p>
</body>
```

```css
img {
   float: left;
}

 p {
   overflow: hidden;
 }
```

2. 利用特性5可以解决浮动元素造成的父元素高度塌陷问题：

```html
<div class="parent">
    <div class="float">浮动</div>
  </div>
```

```css
.parent {
      overflow: hidden;
    }
.float {
      float: left;
    }
```

## 怎么让一个div水平垂直居中

使用`flex`或者`grid`：

```css
div-parent{
	display :flex;
    // display :grid;
}
div.child{
	margin: auto;
}
```

水平垂直居中有好多种实现方式，主要就分为两类不定宽高和定宽高，以在body下插入一个div为例

一、定宽高

使用`定位+margin`：

```css
element.style {
	position: absolute;
    left: 50%;
	top: 50%;
	margin-left: -250px;
    margin-top: -250px;
    width: 500px;
	height: 500px;
	background: yellow;
    z-index : 1;
}
```

使用`定位+transform`：

```css
element.style {
	position: absolute;
    left: 50%;
	top: 50%;
    width: 500px;
    height: 50Opx;
	background: yellow;
    z-index: 1;
	transform : translate3d( -50%,-50%,e);
}
```

二、不定宽高

不定宽高的方法基本都适用于定宽高的情况

这里把div的宽高按照内容展开

使用`定位+transform`同样是适用的：

```css
element.style {
	position: absolute;
    left: 50%;
	top:50%;
	background: yellow;
    z-index: 1;
	transform: translate3d(-50%,-50%,0);
}
```

使用`table-cell`的方式：

```css
.parent {
	display: table-cell;
    height: 200px;
	width: 200px;
	background-color: orange;
    text-align: center;
	vertical-align: middle;
}
.child {
	display: inline-block;width: 180px;
	height: 100px;
	background-color: blue;
}
```

```html
<div class='parent'>
	<div class='child'></div>
</div>
```

## 已知如下代码，如何修改才能让图片宽度为300px ?注意下面代码不可修改。

```html
<img src='1.jpg' style="width: 480px !important;">
```

方法一：

添加`max-width: 300px`

方法二：

```css
box-sizing: border-box;
padding: 0 90px;
```

方法三：

`transform: scale(0.625);`按比例缩放图片

方法四：

使用js：`document.getElementsByTagName('img')[O].setAttribute('style','width:300px  !important;')`

方法五：

```css
img {
	animation: test Os forwards;
}
keyframes test {
    from {
		width: 300px;
    }
	to {
		width: 300px;
    }
}
```

利用CSS动画的样式优先级高于limportant的特性

## 如何用css或js 实现多行文本溢出省略效果，考虑兼容性

对于单行：

```css
overflow: hidden;
text-overflow:ellipsis;
white-space: nowrap;
```

对于多行：

```css
display: -webkit-box;
-webkit-box-orient: vertical;
-webkit-line-clamp: 3; //行数
overflow: hidden;
```

js实现：

1. 使用split +正则表达式将单词与单个文字切割出来存入words
2. 加上 '...'
3. 判断`scrollHeight`与`clientHeight`，超出的话就从`words`中`pop`一个出来

```js
const p = document.querySelector('p')
let words = p.innerHTML.split(/(?<=[\u4e0o-\u9fa5])|(?<=\w*?1b)/g)
while (p.scrollHeight > p.clientHeight){
	words.pop()
	p.innerHTAL = words.join('')+ '...'
}
```

## 清除浮动

一、添加额外标签`clear`

```html
<style>
    .box1 {
        width: 200px;
        height: 200px;
        background-color: green;
        float: left;
    }

    .box2 {
        width: 300px;
        height: 300px;
        background-color: yellow;
    }

    .clear {
        clear: both;
    }
</style>
<body>
    <div class="box1"></div>
    <div class="clear"></div>
    <div class="box2"></div>
</body>
```

二、使浮动元素的父级变为`BFC块`

```js
<style>
    .parent {
        overflow: hidden;
    }

    .box1 {
        width: 200px;
        height: 200px;
        background-color: green;
        float: left;
    }

    .box2 {
        width: 300px;
        height: 300px;
        background-color: yellow;
        float: left;
    }
</style>
<body>
    <div class="parent">
        <div class="box1"></div>
        <div class="box2"></div>
    </div>
</body>
```

三、使用伪元素清除浮动：

```css
.clearfix:before,
.clearfix:after {
	content: "";
	display: table;
}
.clearfix:after {
    clear:both;
}
.clearfix {
   *zoom: 1; 
}
```

```css
.clearfix:before,
.clearfix:after{
  content: " ";
  display: inline-block;
  height: 0;
  clear: both;
  visibility: hidden;
}
.clearfix{
  *zoom: 1;
}
```

## 关于css禁用

```css
pointer-events: auto;
cursor: not-allowed;
```

鼠标事件的禁用

```css
pointer-events: none;
```

