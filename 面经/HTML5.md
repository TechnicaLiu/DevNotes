## 说说title和alt属性

两个属性都是当鼠标滑动到元素上的时候显示的，alt是 img 的 特有属性，图片无法正常显示时候替代的文字。 而title属性 是对 dom 元素的一种类似注释说明 

## 行内元素有哪些？块级元素有哪些？ 空元素有哪些？

1. 行内元素有哪些？

   a、 b  、span 、img 、input 、select、strong 

2. 块级元素有哪些？

   div 、ul 、ol、 li、 dl、dt、dd 、h1、 h2、 h3、h4、 h5、 h6、 p

3. 空元素 ？

   即没有内容得HTML元素，空元素是在开始标签中关闭的，也就是空元素没有闭合标签。

    `<br>`   `<hr>`   `<img>`   `<input>`   `<link>`   `<meta>`   

## HTML5的离线储存怎么使用？他的工作原理是什么？

> 离线存储指的是：在用户没有与因特网连接时，可以正常访问站点或应用。在用户与因特网连接时，更新用户机器上的缓存文件

**原理：**

 HTML5的离线存储 是基于 一个新建的 `.appchache`文件 的缓存机制(不是存储技术)，通过这个文件上的**解析清单离线存储资源**，这些资源就会像cookie一样被存储了下来。之后当网络在处于离线状态下时，浏览器会通过被离线存储的数据进行页面展示

**使用方法：**

1. 新建一个和html同名的manifest文件，然后在页面头部加入manifest属性 ：

   ```js
   <html lang="en" manifest="demo.appcache">
   ```

2. 在manifest文件中编写需要离线存储的资源：

   ```json
   CACHE MANIFEST
       #v0.11
       CACHE:
       js/app.js
       css/style.css
       NETWORK:
       resourse/logo.png
       FALLBACK:
       / /offline.html
   ```

   * CACHE: 表示需要离线存储的资源列表
   * NETWORK ：表示列出的资源只有在有网的时候才会加载，即不会离线存储
   * FALLBACK ： 表示如果访问第一个资源失败，那么就使用第二个资源来代替，如上方的app.js加载不出 就直接 用offline.html来加载。

## head标签有什么作用，其中什么标签必不可少?

`<head>` 标签用于定义文档的头部，他是所有头部元素的容器。在文档的头部描述了文档的各种属性和信息，包括文档的标题，在Web中的位置以及和其他文档的关系等，这部分内容不会展示给读者。

head中一般有 `<base>` `<link>` `<meta>` `<script>` `<style>` `<title>` 

其中 `<title>` 定义了文档的标题，是必不可少的。

## HTML5新增的特性

* 绘画 `canvas`
* 用于媒介回放的 video 和 audio 元素
* 本地离线存储 `localStorage` 关闭浏览器后数据不丢失， `sessionStorage` 浏览器关闭后数据清除 
* 语义化更好的内容元素 ，如 article  footer  header  nav ..
* 表单控件  calendar  date  time email  
* 新的技术 webworker  webScoket 

## HTML W3C 标准

标签闭合、标签小写、不乱嵌套、使用外链 css和 js 、结构行为表现的分离

## document.write( ) 和 innerHTML 的区别 

document.write是直接将内容写入页面的内容流，会导致页面全部重写重绘；

innerHTML 将内容写入某个DOM节点，不会导致页面全部重绘。比document.write更为精确的替换指定区域内容

## WebWorker

>web worker 是运行在后台的 JavaScript，独立于其他脚本，不会影响页面的性能。您可以继续做任何愿意做的事情：点击、选取内容等等，而此时 web worker 在后台运行。

### 检测浏览器是否支持web worker

在创建 web worker 之前，请检测用户的浏览器是否支持它：

```js
if(typeof(Worker)!=="undefined")
{
    // 是的! Web worker 支持!
    // 一些代码.....
}
else
{
    //抱歉! Web Worker 不支持
}
```

### 创建 web worker 文件

在外部创建一个javascript脚本 名为demo：

```js
 //  postMessage(i); 用于向html页面传回一段消息。

var i = 0;

function timedCount() {
  i = i + 1;
  postMessage(i);
  setTimeout("timedCount", 500);
}
timedCount();
```

### HTML中使用

`onmessage` 用来监听接收数据，`terminate` 用来终止webworker，释放浏览器/计算机资源

```html
<body>
  <p>计数： <output id="result"></output></p>
  <button onclick="startWorker()">开始工作</button>
  <button onclick="stopWorker()">停止工作</button>

  <script>
    var w;
    function startWorker() {
      if (typeof (Worker) !== "undefined") {  // 判断浏览器是否支持worker 
        if (typeof (w) == "undefined") {
          w = new Worker("demo.js");  // 创建worker对象  
          w.onmessage = function (event) {
            document.getElementById("result").innerHTML = event.data;

          };
        } else {
          document.getElementById("result").innerHTML = "此浏览器不支持webworker功能"
        }
      }
    }

    function stopWorker() {
      w.terminate();   
      w = undefined;
    }
  </script>
</body>
```

### 注意事项

由于 web worker 位于外部文件中，它们无法访问下列 JavaScript 对象：

- window 对象
- document 对象
- parent 对象

## script标签中defer和async的区别

script标签如果**没有defer和async** ，浏览器会立即加载并执行相应的脚本，它不会等待后续加载的文档元素，读取就会开始加载和执行，这样就**阻塞了后续文档的加载**。

下图表示了三者的区别：

![img](https://gitee.com/youngstory/images/raw/master/img/1603547262709-5029c4e4-42f5-4fd4-bcbb-c0e0e3a40f5a.png)

蓝色代表js脚本网络加载时间，红色代表js脚本执行时间，绿色代表html解析。

defer和 async 属性都是去异步加载外部的js脚本文件，他们都不会阻塞页面的解析，区别如下;

1. **执行顺序**：多个带async属性的标签，不能保证加载的顺序；多个带defer属性的标签会按照加载顺序执行 
2. **脚本是否并行执行**：async属性，表示后续文档的加载和执行与js脚本的加载和执行是并行进行的。即异步执行，defer属性，加载后续文档的过程和js脚本的加载（只加载不执行）是并行执行，但是**js脚本的执行需要等到文档所有元素解析完之后才执行。**

## JavaScript脚本延迟加载的方式有哪些？

js延迟加载  等页面加载完成之后再加载，防止阻碍文档的加载。

1. script 标签添加 defer属性
2. script  标签添加 async 属性
3. 使用setTimeout延迟方法 
4. 让js最后加载 。将js脚本放在文档的底部，来使js脚本尽可能的在最后来加载执行。

## 常用的meta标签有哪些？

meta标签由`name`和`content`属性定义，用来描述网页文档的属性，比如网页的作者，描述，关键词等。

常用的meta标签：

1. charset 用来描述文档的编码类型

2. keywords 页面关键词

3. description 页面描述  

4. refresh 页面重定向和刷新 

5. viewport 适配移动端，可以控制视口的大小和比例 

   ```html
   <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1">
   
   content参数有：
   1.width viewport 宽度
   2.height viewport 高度
   3.initial-scale 初始比例
   4.maximum-scale 最大缩放比例 / minimum 
   5.user-scalable:是否允许用户缩放
   ```

6. 搜索引擎索引方式

   ```html
   <meta name="robots" content="index,follow" />
   index : 文件将被索引
   follow 页面上的链接可以被查询
   ```


## 渐进增强和优雅降级之间的区别

### 渐进增强

主要是针对低版本的浏览器进行页面重构，保证基本的功能情况下，再针对高级浏览器进行效果、交互等方面的改进和追加功能，以达到更好的用户体验。

### 优雅降级

一开始就构建完整的功能，然后再针对低版本的浏览器进行兼容。

### 区别

1. 优雅降级是从复杂的现状开始的，并试图减少用户体验的供给。
2. 而渐进式增强是从一个非常基础的，能够起作用的版本开始的，并在此基础上不断扩充，以适应未来环境的需要。

## 统计网页中有多少种标签

包含的知识点：如果获取所有DOM节点、伪数组如何转为数组、去重

1. 获取页面内的所有DOM节点

   ```js
   document.querySelectorAll('*')
   ```

2. 此时得到的是一个 NodeList集合，我们需要将其转化为数组 

   ```js
   [...document.querySelectorAll('*')]
   ```

3. 获取数组每个元素的标签名

   ```js
   [...document.querySelectorAll('*')].map(ele => ele.tagName)
   
   ```

4. 利用Set的不重复 原则，进行去重

   ```js
   new Set([...document.querySelectorAll('*')].map(ele=> ele.tagName)).size
   
   ```

5. 完成！ 成功拿到html标签个数。