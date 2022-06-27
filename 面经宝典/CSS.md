## 设置盒子居中的几种方式

- Margin

  ```
  div{
  	width:100px;
  	height:100px;
  	margin:0 auto;
  }

  ```

- 利用定位让盒子水平垂直居中

  ```
  <div class="parent">
      <div class="child">

      </div>
  </div>

  .parent {
      background-color: #eee;
      width: 500px;
      height: 500px;
      margin: 10px auto;
      position: relative;
  }

  .child {
      width: 200px;
      height: 200px;
      background-color: lightcoral;
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);

  ```

- 利用 flex 布局实现盒子水平居中

  ```
  <style>
  	.box{
          border: 1px red solid;
          width: 100px;
          height: 100px;
          display:flex;
          justify-content: center;
          align-items: center;
      }
    .box1{
  		width:50px;
  		height:50px;
  		border:1px red solid;
  	}
  </style>
  <body>
  	<div class="box">
  	 <div class="box1"></div>
  	 </div>
  </body>

  ```

## flex 布局中，flex:1; 是什么作用？

让所有弹性盒模型对象的子元素都有**相同的长度**，且忽略它们内部的内容：

```
<style>
#main
{
	width:220px;
	height:300px;
	border:1px solid black;
	display:flex;
}

#main div
{
	flex:1;
}
</style>
</head>
<body>

<div id="main">
  <div style="background-color:coral;">红色</div>
  <div style="background-color:lightblue;">蓝色</div>
  <div style="background-color:lightgreen;">带有更多内容的绿色 div</div>
</div>
```

## 了解 grid 布局和 flex 布局吗

grid 布局：将容器划分成“行"和”列“，产生单元格，指定的项目所在的单元格，可以看作是二维布局。

flex 是弹性布局（轴线布局），只能指定”项目“针对轴线的位置，可以看作是一维布局，用来为盒状模型提供最大的灵活性，任何一个容器都可以指定为 flex 布局，行内元素也可以使用 flex 布局

## CSS3 新特性举几个列子

- 可以创建圆角边框
- box-shadow 属性 添加阴影
- background-image 背景图片 background-size 、背景线性渐变
- transition animation 过渡动画

## link 和 @import 的区别

- link 是 html 方式， @import 是 css 方式

  ```css
  <style>
  @import "../css/style.css";
  @import "//netdna.bootstrapcdn.com/font-awesome/4.7.0/css/font-awesome.min.css";
  </style>
  ```

- link 优先级要比 @import 高

- @import 必须在样式规则 之前， 可以在 css 文件中引用其他文件

- 在 vue 组件中也可以在 script 标签中 引入 css

## 清除浮动的几种方式

- 在浮动元素后边加一个div ，并设置 **clear:both** 样式

  ```js
  <div class="left" style='float:left'>左侧盒子</div>
  <div style='clear:both'></div>   // 在浮动元素后加  空标签 
  <div class="right">右侧盒子</div>
  
  
  ```

* 给浮动元素的父级 设置高度

  ```js
   <div class="father">
        <div class="left">左侧盒子</div>
    </div>
   <div class="center">中间盒子</div>
  
  .father {
        height: 100px;
      }
      .left {
        width: 100px;
        height: 100px;
        border: 1px solid rgb(196, 41, 41);
        float: left;
    }
    
  ```

* 给父元素添加 after 设置clear:both

  ```js
  <div class="father">
        <div class="left">左侧盒子</div>
      </div>
  <div class="center">中间盒子</div>
  <div class="right">右侧盒子</div>
  
  .left {
    width: 100px;
    height: 100px;
    border: 1px solid rgb(196, 41, 41);
    float: left;
  }
  .father:after {
    content: "";
    display: block;
    clear: both;
  }
  ```

* 父元素 overflow:auto / ov erflow:hidden

  ```js
  <div class="father">
        <div class="left">左侧盒子</div>
      </div>
  <div class="center">中间盒子</div>
  <div class="right">右侧盒子</div>
  
  .father{
    overflow:auto / hidden 
  }
  .left{
    float:left
  }
  ```

## display: inline-block 与 inline block 的区别

- 与 inline 相比，display: inline-block 允许在元素上设置宽度和高度, 将保留上下外边距/内边距
- 与 display: block 相比，主要区别在于 display：inline-block 在元素之后不添加换行符，因此该元素可以位于其他元素旁边。

## CSS 权重

- ！important > 内联样式 style > ID > 类属性、属性选择器或者伪类选择器 > 标签选择器

## sass 、less 是什么 ？

- 他们是**CSS 预处理器**，
- less 是一种**动态样式语言**，将 css 赋予了动态语言的特性，如变量、继承、运算、函数，LESS 既可以 在客户端上运行，也可在服务端运行。
  - 变量符不一样 ，less 是 @ ， 而 sass 是 $
  - sass 支持 条件语句，可以使用 if else for 循环等等。而 Less 不支持。
  - 结构清晰，便于扩展。完全兼容 CSS 代码，可以方便地应用到老项目中。

## px 和 em rem 的区别

- px 和 em 都是长度单位，px 值固定，相对于**显示器屏幕分辨率**而言。em 值不固定，是相对单位，其**相对应父级元素的字体**大小而言。
- rem 相对单位，**相对的只是 HTML 根元素**，既可以做到只修改根元素就成比例地调整所有字体大小，又可以避免字体大小逐层复合的连锁反应。如果不设置根元素的字体大小，则会默认以根元素的 16px 的大小计算值。

## 视口单位

**什么是视口？**

视口单位中的“视口”，桌面端指的是浏览器的可视区域，移动端指的是 Viewport 的 `Layout Viewport`

根据 CSS3 规范，视口单位主要包括以下 4 个 ：

1. **vw:1vw 等于视口宽度的 1%；**
2. **vh:1vh 等于视口高度的 1%；**
3. vmin: 选取 vw 和 vh 中最小的那个；
4. vmax: 选取 vw 和 vh 中最大的那个；

比如： 浏览器高度为 950px,宽度为 1920px ，1vh = 950/100 = 9.5px ，1vw=1920/100=19.2px

注意：**vw、vh 只是相对的视口宽度和高度，当手机平放时，vw 还是表示水平方向的，vh 还是表示竖直方向的。**

## 重排和重绘

部分渲染树（或者整个渲染树）需要**重新分析并且节点尺寸需要重新计算**。这被称为重排。
由于节点的几何属性发生改变或者由于样式发生改变，例如改变元素背景色时，屏幕上的部分内容需要更新。这样的更新被称为重绘。

## 什么情况会触发重排和重绘？

- 添加、删除、更新 DOM 节点
- 通过 display: none 隐藏一个 DOM 节点 **- 触发重排和重绘**
- 通过 visibility:hidden 隐藏一个 DOM 节点 - **触发 重绘**
- 移动或者给 页面中的 DOM 节点添加动画
- 添加一个样式表，调整样式属性
- 用户行为，例如调整窗口大小，改变字号，或者滚动

## 什么是 BFC? BFC 的布局规则是什么？如何创建 BFC？BFC 应用？

> BFC(block formatting context ) 块级格式化上下文，他是一个独立的渲染区域，只有块级盒子参与，它规定了内部的 盒子如何布局，并于这个区域外部毫不相干

**Box ：** css 布局的基本单位，直观点说就是一个页面是由很多个 Box 组成的，元素的类型和 display 属性 ，决定了这个 box 的类型 。 **不同类型的 Box，会参与不同的 Formatting Context .**

块级盒子 ： display 属性为 block .list-item,table 的元素，会生成 block-level box , 并且参与 blockd fomatting context ;

内联盒子 : display 属性为 inline , inline-block , inline-table 的元素，会生成 inline-level box. 并且参与 inline formatting context

**Formatting Context :** 它是页面中的一块 渲染区域，并且有一套渲染规则 ，它**决定了其子元素将如何定位**，**以及和其他元素的关系和相互作用。**最常见的 Formatting context 有 块级盒子和内联盒子  

**BFC 的布局规则 :**

- 内部的 Box 会在**垂直方向**，一个接一个的放置。
- box 垂直方向的距离由 margin 决定， 属于同一个 BFC 的两个 相邻 box 的 margin 会发生重叠。
- 每个盒子（块盒与行盒）的 margin box 的左边，与包含块 border box 的左边相接触(对于从左往右的格式化，否则相反)。即使存在浮动也是如此。
- **BFC 的区域不会与 float box 重叠。**
- BFC 就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。反之也如此。计算 BFC 的高度时，浮动元素也参与计算。

**如何创建 BFC?**

- 浮动元素、根元素、定位元素、行内块元素
- display 的值是 inline-block、table-cell、flex、table-caption 或者 inline-flex
- overflow 的值不是 visible

**实例：**

- 两个 P 元素 同时设置 margin，会发生 margin 重叠 ，我们只需要给第二个 P 元素 加个 div 并且 设置 overflow:hidden 即可 。
- 实现自适应布局 ，也可以给 右边的盒子 加 overflow:hidden
- 清除浮动 同理 。

## 实现一个两列等高布局

为了实现两列等高，可以给每列加上 padding-bottom:9999px;margin-bottom:-9999px ; 同时给父元素设置 overflow : hidden

![image-20210918094244194](https://techliuimg.oss-cn-beijing.aliyuncs.com/img/202110231655240.png)

## 码出一个三角形

```css
.box {
  width: 0;
  height: 0;
  border: 25px solid transparent;
  border-bottom: red;
}
```

## 单行文本/多行文本省略

单行文本思路：white-space overflow text-flow

```css
p {
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
```

多行文本思路：利用 -webkit- 自适应方法

```css
p {
  height: 150px;
  overflow: hidden;
  display: -webkit-box; // 弹性伸缩盒子模型显示
  -webkit-box-orient: vertical; // 设置伸缩盒子的子元素排序为垂直
  -webkit-line-clamp: 4; // 限制在一个块元素显示的文本的行数
}
```

## 圣杯布局和双飞翼布局

圣杯布局长这样 ！！！！![image-20210923195322237](https://techliuimg.oss-cn-beijing.aliyuncs.com/img/202110231655475.png)

**圣杯布局**基本要求：

- header 和 footer 各自占领屏幕的所有宽度，高度固定
- 中间的 container 是一个三栏布局
- 三栏布局两侧宽度固定不变，中间部分自动填充整个区域
- 优先加载三栏布局中的 center

代码实现：

```css
<header>头部 内容</header>
    <div class="clearfix wrapper">
        <div class="center">中间区域</div>
        <div class="left">左边</div>
        <div class="right">右边</div>
    </div>
<footer>底部 内容</footer>

<style>
    *{
        padding: 0;
        margin: 0;
        text-align: center;
        color: wheat;
    }
    header{
        background: rgb(106, 128, 218);
        height: 100px;

    }
    footer{
        background: rgb(206, 105, 79);
        height: 100px;
    }
    .center{
        background: rgb(82, 124, 72);
        width: 100%;  /* 撑满了container  把左右俩小盒子挤了下去  */
        height: 200px;
    }
    .left{
        background: rgb(171, 104, 173);
        width: 100px;
        margin-left: -100%;  /* 由于center设置了100%所以左盒子会被挤下来，这样设置会让它去上一层位置 */
        position: relative; /* 利用定位 左移为了不挡住 center的内容 */
        left: -100px;
        height: 200px;
    }
    .right{
        background: rgb(95, 143, 172);
        width: 100px;
        margin-left: -100px;
        position: relative;
        right: -100px;
        height: 200px;
    }
    /* 设置浮动，让这三个盒子放在一个水平上 */
    .left, .center, .right{
        float: left;
    }
    /* 清除浮动  */
    .clearfix::after{
        clear: both;
        content: '';
        display: block;
    }
    /* 大盒子设置 padding 100px  是为了给 左右两个小盒子空出位置  */
    .wrapper{
        padding: 0 100px;
    }

</style>
```

双飞翼布局和圣杯布局实现的效果一样，只是方法不同： 利用了 marign-left 和 margin-right 为左右盒子留空间。

代码实现：

```css
<header
  > 头部
  内容</header
  > <div
  class="wrapper"
  > <div
  class="center"
  > 中间区域</div
  > </div
  > <div
  class="left"
  > 左边</div
  > <div
  class="right"
  > 右边</div
  > <footer
  > 底部
  内容</footer
  > * {
  padding: 0;
  margin: 0;
  text-align: center;
  color: wheat;
}
header {
  background: rgb(106, 128, 218);
  height: 100px;
}
footer {
  background: rgb(206, 105, 79);
  height: 100px;
  clear: both;
}
.wrapper {
  width: 100%;
  float: left;
}
.center {
  background: rgb(82, 124, 72);
  margin: 0 100px; /*为左右盒子 留的位置 */
  height: 200px;
}
.left {
  background: rgb(171, 104, 173);
  width: 100px;
  float: left;
  margin-left: -100%; /* 由于wrapper设置了100%所以左盒子会被挤下来，这样设置会让它去上一层位置 */
  height: 200px;
}
.right {
  background: rgb(95, 143, 172);
  width: 100px;
  margin-left: -100px;
  float: left;
  height: 200px;
}
```

## css 动画和 js 动画的区别

|      | css 动画                                                                                                                    | js 动画                                                                               |
| ---- | --------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------- |
| 优点 | 1. 比 js 动画更流畅。2. 性能较好，浏览器会对 CSS 动画做一些优化 3. 代码相对简单                                             | 1.控制能力强，可以实现开始、暂停、终止等行为。2. 可实现的动画效果丰富 3. 无兼容性问题 |
| 缺点 | 1. 在动画的控制上不够灵活（不能添加事件绑定回调函数） 2. 只能实现简单动画，不能实现复杂逻辑的动画 3. 代码冗长 4. 兼容性不好 | 1. js 在浏览器的主线程中运行，线程可能会出现阻塞状态，导致丢帧。 2. 代码复杂度高      |

## css 中可继承属性和不可继承属性

> 所谓继承是指 html 元素可以从父元素那里继承一部分 css 属性，即使当前元素没有定义该属性。

不可继承的属性:

- display:规则元素应该生成的框的类型
- text-decoration 添加到文本的装饰
- text-shadow:文本阴影效果
- white-space 空白符的处理
- 盒子模型的属性： width、height、marign、border、padding
- 定位属性：float、clear、position、top、right、bottom、left、min-width、min-height、max-width、max-height、overflow、clip、z-index

## 哪些 css 属性可以触发 GPU 的硬件加速？

1. transform
2. opacity
3. filter

## 硬件加速的工作原理

浏览器接收到页面文档后，会将文档中的标记语言解析为 DOM 树。DOM 树和 CSS 结合后形成浏览器构建页面的渲染树。渲染树中包含了大量的渲染元素，每一个渲染元素会被分到一个图层中，每个图层又会被加载到 GPU 形成渲染纹理，而图层在 GPU 中 `transform` 是不会触发 repaint 的，这一点非常类似 3D 绘图功能，最终这些使用 `transform` 的图层都会由[独立的合成器进程进行处理](https://www.chromium.org/developers/design-documents/gpu-accelerated-compositing-in-chrome)。
