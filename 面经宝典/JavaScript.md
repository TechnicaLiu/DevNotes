# 基础

## 解释性语言和编译型语言的区别

1. **解释性语言**

   由专门的解释器对源程序逐行解释成特定平台的机器码并立即执行，注意！是在代码执行时才开始翻译和执行 。 效率低

   代表：Javascript、python 等

2. **编译型语言**

   使用专门的编译器，针对特定的平台，将高级语言源代码一次性的编译成机器码。执行效率高

   代表： C、C++ 等

## 说几条 JavaScript 的基本规范

- 不要在同一行声明多个变量
- 请使用 === / !== 来比较 true/false 或者数值
- 使用对象字面量替代 new Array 这种形式
- 不要使用全局函数
- Switch 语句 必须带有 default 分支
- IF 语句 必须使用大括号
- for-in 循环中的变量应该使用 let 关键字 明确限定作用域，从而避免作用域污染

## 小数精度的问题

**0.1+0.2= ?** 如果说是 0.3 那就错了。 **0.1+0.2 != 0.3**

因为这涉及到了计算机数字存储的问题 。**javascript 浮点数采用的是 64 位双精度 存储**，其中的 1 位表示符号位，11 位用来表示指数位，剩下的 52 位尾数位。

由于只能存储 52 位尾数位，所以会出现精度缺失，把它存到内存中再取出来转换成十进制就不是原来的 0.1 了

0.1 转换为二进制为 0.0001100110 ， 0.2 转换为 二进制位 0.001100110 ，两者加起来转换为十进制不等于 0.3

**如何让其等于 0.3？**

对其进行转化 四舍五入

```
(n1+n2).toFixed(2) // 注意： toFixed 为四舍五入
```

**为什么 0.2+0.3=0.5 呢？**

因为 0.2 和 0.3 都转化为二进制后再进行计算，得到的结果尾数大于 52 位，而实际取值只取 52 位尾数，且前 52 位尾数都是 0，所以最终结果转为十进制就是 0.5 了。

**那既然 0.1 不是 0.1 了，为什么在 console.log(0.1)的时候还是 0.1 呢 ？**

在 console.log 的时候会将二进制转换为十进制，十进制再转为字符串的形式，在转换的过程中发生了近似值，所以打印出来的是一个近似值的字符串 。

## 闭包

- **指有权访问另一个函数作用域中变量的函数** ，最常见的方式是 在一个函数内创建另一个函数，通过另一个函数访问这个函数的局部变量，利用闭包可以实现作用域链

- 闭包的特性
  - 函数内再嵌套函数
  - 内部函数可以引用外层的参数和变量 （作用域链原则）
  - 参数和变量不会被垃圾回收机制回收（访问函数内部变量，保持函数在环境中一直存在）
- 优点 ： 能够实现缓存和封装

  - 缺点 : 消耗内存，使用不当会内存溢出

  - 应用

    ```js
    function add() {
      var count = 0;
      function demo() {
        count++;
        console.log(count);
      }
      return demo;
    }
    var counter = add();
    counter();   //每次调用counter（）都在会原来的基础上+1
    -------------------------------------------------------------------------------
    //  for 循环经典面试题
    for (var i = 0; i <= 5; i++) {
      (function (i) {
        setTimeout(function () {
          console.log(i);
        }, 0)
      })(i)
    }
    ```

## 说说你对作用域、作用域链的理解

- 作用域规定**变量或者函数可使用范围**。

- 每个函数都有一个作用域链，作用域链的作用是 **保证 执行 环境里有权访问的变量 和函数是有序的，**作用域链的变量只能向上访问，变量访问到 window 对象即被终止，作用域链向下访问变量是不被允许的。

## 什么是事件委托？（事件代理）

- 不是每个子节点单独设置事件监听器，而是事件监听器设置在其父节点上，然后利用**冒泡原理**影响设置每个子节点
- 如 在 table 上代理所有 td 的 click 事件， 提高性能

## 谈谈你对 this 的理解 (四种绑定规则)

### 默认规则

普通函数 全局 this 立即执行函数 this 指向 window

```js
    console.log(this); // window
    console.log({} === {}); // false   引用地址不同

    function add() { // 普通函数中的this 也是指向 window
      console.log(this);
    }
    add()；

		// 立即执行函数
		(function(){
       console.log(this)  // window
    })();
```

闭包中的 this

```js
var a = 0;
var obj = {
  a: 2,
  foo: function () {
    console.log(this); // 2     this => obj对象
    function test() {
      //闭包 ： 当函数执行的时候导致函数被定义，并抛出
      console.log(this);
    }
    return test;
  },
};
// 函数执行this才有意义, 一个函数有一个this的指向
obj.foo()(); // obj.foo() 返回的是test,如果想获取test函数就必须再加个括号调用
// 上面就变成了 test()  独立调用  this指向window
```

### 隐式绑定规则

谁调用就指向谁

```js
var a = 0;
var obj = {
  a: 2,
  foo: function () {
    console.log(this); // 2     this => obj对象
    function test() {
      // 两个函数，this不干扰
      console.log(this); // 所以是windows
    }
    test();
  },
};
// 函数执行this才有意义, 一个函数有一个this的指向
obj.foo();
```

特殊情况 ：

```js
var a = 0;
function fo() {
  console.log(this);
}
var obj = {
  a: 2,
  foo: fo,
};
var bar = obj.foo; // obj.foo  相当于 fo 函数  但是没有调用，所以不存在this 指向
bar(); // 把 fo函数 丢给了 bar ,且调用，就变成了简单的 独立调用
```

### 显示绑定

call( ) apply( ) bind( )

```js
var a = 0;
function fo(a, b, c) {
  console.log(a, b, c);
}
var obj = {
  a: 2,
  foo: fo,
};
var bar = obj.foo;
obj.foo(1, 2, 3); // obj对象
bar.call(obj, 1, 2, 3); // obj对象
bar.apply(obj, [1, 2, 3]); // obj对象
bar.bind(obj)(1, 2, 3); // obj对象
```

### new 绑定

**构造函数中的 this 指向实例化的对象** 优先级最高 高于 显示绑定

```js
function foo(b) {
  this.a = b;
}
var obj1 = {};
var bar = foo.bind(obj1);
/* 
      obj1{
        a:2
      }
      
    */
bar(2);

console.log(obj1.a); // 2

var baz = new bar(3);
/* 
      baz{
        a:3
      }   
    */
console.log(obj1.a); // 2
console.log(baz.a); // 3
```

### 箭头函数中的 this

由外层函数的作用域 this 决定的

```js
var a = 0;

function foo() {
  console.log(this);
  var that = this; // 内部函数 test想用 外边的this可以用临时变量来接收赋值  // 用 箭头函数
  /* function test() {  
        console.log(that);
      }
      test() */
  var test = () => {
    // 箭头函数没有 this，所以它的this只能指向定义时所在的父级作用域中的this
    console.log(this);
  };
  test();
}
var obj = {
  a: 1,
  foo: foo,
};
obj.foo();
```

## 事件模型

- w3c 中定义事件的发生经历三个阶段： 捕获阶段 、目标阶段、冒泡阶段<img src="https://techliuimg.oss-cn-beijing.aliyuncs.com/img/202110231657132.png" alt="image-20210825074536746" style="zoom:80%;" />

- 阻止冒泡： 使用 stopPropagation( ) 方法

- 阻止捕获： 阻止事件的默认行为，例如 click a 链接后的跳转。 使用 preventDefault（ ） 方法

## Ajax 原理

> AJAX 是一种在无需重新加载整个网页的情况下，能够更新部分网页的技术。

Ajax 的原理简单来说是在用户和服务器之间加了—个中间层( AJAX 引擎)，通过 XmlHttpRequest
对象来向服务器发异步请求，从服务器获得数据，然后用 javascript 来操作 DOM 而更新页面。

<img src="https://techliuimg.oss-cn-beijing.aliyuncs.com/img/202110231657904.png" alt="image-20210825080545650" style="zoom:80%;" />

### 原生 ajax 过程实现 ：

- 创建 XMLHttpRequest 对象 xhr= new XMLHttpRequest( );

- 调用 open 方法 传入三个参数 （get/post , url , 同步/异步）

- 监听 onreadystatechange 事件，当 readystate 等于 4 且 status == 200 时返回 responseText

  ```js
  xhr.readyState   存有 XMLHttpRequest 的状态
      0: 请求未初始化  // open()尚未调用
      1: 服务器连接已建立  // open() 已调用
      2: 请求已接收	// 接收到头信息
      3: 请求处理中   // 接收到响应体
      4: 请求已完成，且响应已就绪  xhr.readyState == 4

  xhr.status   状态属性
      200  成功
      404  页面不存在
      403  禁止访问
      500  服务器错误
      304  从缓存中读取数据
  ```

- 调用 send ( [string] ) 方法 传递参数 ，string 只能传递 post 数据 同时要设置 请求头 设置 MIME 类型

  ```js
  // 当使用POST方法 提交 表单数据（find=ppizza&zipcode=012）时， 设置 以下这个值
  xhr.setRequestHeader("Content-type", "application/x-www-form-urlencoded");
  ```

### Ajax 解决浏览器缓存问题

- 在 ajax 发送请求前加上 anyAjaxObj.setRequestHeader("If-Modified-Since","0")。
- 在 ajax 发送请求前加上 anyAjaxObj.setRequestHeader("Cache-Control","no-cache")。
- 在 URL 后面加上一个随机数： "fresh=" + Math.random()。
- 在 URL 后面加上时间搓："nowtime=" + new Date().getTime()

### 实例

普通的 get 请求

![img](https://techliuimg.oss-cn-beijing.aliyuncs.com/img/202112141409994.jpeg)

使用表单编码数据发起一个 HTTP POST 请求

![image-20211228112542524](https://techliuimg.oss-cn-beijing.aliyuncs.com/img/202112281125635.png)

使用 JSON 编码主体来发起 HTTP POST 请求

```js
function postJson(url, data, callback) {
  var request = new XMLHttpRequest();
  request.open("POST", url);
  request.onreadystatechange = function () {
    if (request.readyState === 4 && callback) callback(request);
  };
  request.setRequestHeader("Content-Type", "application/json");
  request.send(JSON.stringify(data));
}
```

当 HTML 表单同时包含文件上传元素和其他元素时，我们需要采用 FormData 来 将请求主体分离成多个部分

```js
function postJson(url, data, callback) {
  var request = new XMLHttpRequest();
  request.open("POST", url);
  request.onreadystatechange = function () {
    if (request.readyState === 4 && callback) callback(request);
  };
  var formdata = new FormData();
  for (var name in data) {
    if (!data.hasOwnProperty(name)) continue; // 跳过继承属性
    var value = data[name];
    if (typeof value === "function") continue; // 跳过方法
    formdata.append(name, value); // 将数据追加到 formdata
  }
  // 当传入 FormData对象时，send() 会自动设置Content-Type头
  request.send(formdata);
}
```

### ajax axios 和 fetch 的区别 ？

axios 本质上对 原生 ajax 进行 promise 的封装，符合最新的 ES 规范通过 .then 来返回成功的结果，.catch 来接收失败的结果 。具有以下特征： 提供了一些并发请求的接口、拦截请求和响应、**自动转换 JSON 数据** 并且会自动设置默认请求头: Content-Type:application/json; charset=utf8;

```js
axios({
  method: "post",
  url: "/user/12345",
  data: {
    firstName: "Fred",
    lastName: "Flintstone",
  },
})
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });
```

而用 jquery 的 ajax 发送 post 请求，本质是模仿表单请求，数据会以**查询字符串格式**发送到后端（name=zs&age=18,类似这样的数据格式叫做查询字符串格式）默认请求头为：Content-Type:application/x-www-formdata-urlencoded;

fetch 同样使用了 ES6 中的 promise 对象。fetch 不是 ajax 的进一步封装，而是原生 js，没有使用 XMLHTTPRrequest 对象 。

```js
try {
	let response = await fetch(url);
	let data = response.json();
	console.log(data);
} catch(e) {
	console.log("Oops, error", e);221
}

// 基本语法
fetch('http://example.com/movies.json')
  .then(function(response) {
    return response.json();
  })
  .then(function(myJson) {
    console.log(myJson);
  });
最简单的用法是只提供一个参数用来指明想 fetch() 到的资源路径，然后返回一个包含响应结果的promise（一个 Response 对象）。
```

## js 的 基础数据类型 / 引用数据类型 && 包装对象

基本数据类型

- undefined 未被初始化的变量都有默认值 undefined

- null 代表变量有值，但是是一个空值

- Boolean 布尔型 true/false

- Number

- String

- Symbol 符号型 唯一的并且是不可修改用来定义独一无二的对象属性名

  ```js
  let sy = Symbol("key1");

  // 写法1
  let syObject = {};
  syObject[sy] = "kk";
  console.log(syObject); // {Symbol(key1): "kk"}
  // 写法2
  let syObject = {
    [sy]: "kk",
  };
  console.log(syObject); // {Symbol(key1): "kk"}

  // 写法3
  let syObject = {};
  Object.defineProperty(syObject, sy, { value: "kk" });
  console.log(syObject); // {Symbol(key1): "kk"}
  ```

引用数据类型

- Object
- Array
- Date
- Function

补充 ： **基本数据类型的数据是直接保存在栈 Stack 中，而引用数据类型的数据存储在堆 heap 中**，栈内存是系统自动分配空间的，而堆是由程序员分配释放的，用完以后要记得销毁，以免内存滥用

**包装对象** ： 基本数据类型是没有如 split 、toString 的方法的，在 JavaScript 中只要引用了字符串（数字、布尔值也是一样的）的属性，**JS 就会将字符串通过调用 new String(s)的方式转换成对象，这个对象继承了字符串的方法，并被用来处理属性的引用。**一旦属性引用结束，这个新创建的对象就会销毁。这个过程就叫包装对象

## 谈谈你对 AMD 和 Commonjs 的理解

- Amd 和 Commonjs 都是为了实现模块化编程而出现的‘
- Commonjs **同步加载 适用于服务器端，** Node 就是采用这种模式，它是同步加载不同模块文件
- Amd （Asynchronous Module Definition）**异步模块定义，适用于 浏览器端，**浏览器需要使用的 js 文件都存放在服务器端，从服务器端加载文件到浏览器是受网速等各种环境因素的影响的，如果采用同步加载方式，一旦 js 文件加载受阻，后续在排队等待执行的 js 语句将执行出错，会导致页面的‘假死’，用户无法使用一些交互。所以在浏览器端是无法使用 CommonJS 的模式的。

## 介绍 js 有哪些内置对象 ？

- Object 是 javascript 中所有对象的父对象
- 数据封装对象 ： Object 、Array、Boolean 、number 、String
- 其他对象: Function 、Arguments 、Math 、Date 、RegExp、Error

## 谈谈你对 ES6 的理解

- 新增模板字符串 ${ }
- 箭头函数 this 指向 window
- for-of 用来遍历数据- 例如数组中的值
- arguments 对象可被 不定参数 和默认参数完美代替
- ES6 将 promise 对象纳入规范，提供了原生的 Promise 对象。
- 增加了 let 和 const 命令， 用来声明变量
- 引入了 module 模块的概念

## sort 快速打乱数组

```js
let arr = [1, 2, 3, 4, 5, 6, 7, 8, 9];
arr.sort(() => {
  return Math.random() - 0.5;
  //利用 sort  return 大于 等于0 不交换位置，小于0 交换位置
});
console.log(arr);
[8, 9, 4, 6, 1, 5, 2, 3, 7];
```

## 跨域问题

> 跨域产生的原因：浏览器受到同源策略的限制，在不同域名、不同端口、不同协议等情况下不允许资源共享。

解决跨越的方法：

- jsonp

  通常为了减轻 web 服务器的负载，我们把 js、css，img 等静态资源分离到另一台独立域名的服务器上，在 html 页面中再通过相应的标签从不同域名下加载静态资源，而被浏览器允许，基于此原理，我们可以通过动态创建 script，再请求一个带参网址实现跨域通信。只适用于 get 请求 方式

  ```js
  //去创建一个script标签
  var script = document.createElement("script");
  //script的src属性设置接口地址 并带一个callback回调函数名称
  script.src = "http://127.0.0.1:8888/index.php?callback=jsonpCallback";
  //插入到页面
  document.head.appendChild(script);
  //通过定义函数名去接收后台返回数据
  function jsonpCallback(data) {
    //注意  jsonp返回的数据是json对象可以直接使用
    //ajax  取得数据是json字符串需要转换成json对象才可以使用。
  }
  ```

- CORS 请求 （设置后端请求头）

  浏览器一旦发现 ajax 请求跨越，就会加一些附加请求头

  ```js
   header(Access-control-Allow-Origin:  * ');
  //*代表允许访问的来源（所有），但是你在请求头携带cookie等东西时，必须要指明，也就是设置跨域白名单。

  header('Access-cjsontrol-Allow-Method: POST,GET');  //允许访问的方式

  ```

- 设置代理

  客户端发送请求时，不直接到服务器，而是先到代理的中间层，同理，当服务器返回数据时，先到代理的中间层。

- Nginx 反向代理 以及 Apache 逆向代理

  nginx 的功能就是把请求转发给后面的服务器，决定哪台目标主机来处理当前请求。

  安装 nginx ，配置 location ，前端的地址和后端的地址**用 nginx 转发到同一个地址**下，如 5500 端口和 3000 端口都转到 3003 端口下

## 如何改变普通函数里的 this 指向问题？

- call( ) 调用函数 + 改变 this 指向
- apply( ) 参数是数组
- bind( ) 不调用函数 + 改变 this 指向

## var、let、const 的区别

- var 定义的变量，没有块的概念，可以跨块访问，不能跨函数访问。可以**实现变量提升**在他的作用域内

- let 定义的变量，只能在块作用域里访问，不能跨块访问 ，且 声明的变量不能提升，同样存在**暂时性死区，只能在声明的位置后面使用**。并且**不能重复声明**

- const 用来定义常量，使用时必须初始化(即必须赋值)，只能在块作用域里访问，而且不能修改。

- **const 实际上保证的不是值不变，而是变量指向的那个内存地址不得改动。**对于简单类型的数据，值就是保存在变量指向的那个内存地址，因此等同于常量。但对于复合类型的数据，变量指向的内存地址，保存的只是一个指针，const 只能保证这个指针是固定的。至于它指向的数据结构是不是可变的，就完全不能控制了。

- **const 定义的对象属性是可以改变的** const 仅能保证指针不发生变化，修改对象的属性不会发生改变指针，所以是被允许的

  ```js
  const person = {
    name: "jiuke",
    sex: "男",
  };
  person.name = "test";
  console.log(person.name); // test
  ```

## 节流防抖

- 防抖和节流的作用都是**防止函数多次调用**，

- 节流：**每隔一段时间执行一次**，通常用在高频率触发的地方，降低频率。如鼠标滑动、拖拽

  ```js
  //定义：当持续触发事件时，保证隔间时间触发一次事件。
  //1. 懒加载、滚动加载、加载更多或监听滚动条位置；
  //2. 百度搜索框，搜索联想功能；
  //3. 防止高频点击提交，防止表单重复提交；
  function throttle(fn, wait) {
    let pre = 0;
    return function (...args) {
      let now = Date.now();
      if (now - pre >= wait) {
        fn.apply(this, args);
        pre = now;
      }
    };
  }
  function handle() {
    console.log(Math.random());
  }
  window.addEventListener("mousemove", throttle(handle, 1000));
  ```

- 防抖：**一段时间内连续触发，不执行。直到超出限定时间执行最后一次。** 如 input 模糊搜索 ，以及用户输入 ， 只需要在输入完成后做一次输入校验即可。

  ```js
  //定义：触发事件后在n秒内函数只能执行一次，如果在n秒内又触发了事件，则会重新计算函数执行时间。
  //搜索框搜索输入。只需用户最后一次输入完，再发送请求
  //手机号、邮箱验证输入检测 onchange oninput事件
  //窗口大小Resize。只需窗口调整完成后，计算窗口大小。防止重复渲染。
  const debounce = (fn, wait, immediate) => {
    let timer = null;
    return function (...args) {
      if (timer) clearTimeout(timer);
      if (immediate && !timer) {
        fn.call(this, args);
      }
      timer = setTimeout(() => {
        fn.call(this, args);
      }, wait);
    };
  };
  const betterFn = debounce(() => console.log("fn 防抖执行了"), 1000, true);
  document.addEventListener("scroll", betterFn);
  ```

## 如何理解前端模块化

前端模块化就是复杂 的文件编程一个一个独立的模块，比如 js 文件等等，分成独立的模块有利于重用和维护，这样会引来模块之间相互依赖的问题，所以有了 commonJS 规范，AMD、CMD 规范等等，以及用于 js 打包的工具 webpack

## 作用域链

在执行上下文中访问到父级甚至全局的变量，是作用域链的作用。

作用域链可以理解为一组对象列表，包含父级和自身的变量对象，因此我们可以通过作用域链访问到父级里声明的变量或者函数 。

由两部分组成：[[scope]] 属性： 指向父级变量对象和作用域链 ，也就是包含了父级的[[scope]] 和 AO , 和 AO 自身活动对象 （当变量对象所处的上下文为 active EC 时，称为活动对象）

## AST

抽象语法树 （Abstract Syntax Tree) 是将**代码逐字母解析成 树状对象** 的形式。这是语言之间 的转换、代码语法检查，代码风格检查，代码格式化，代码高亮，代码错误提示，代码自动补全等等的基础 。

## babel 编译原理

- **Parse(解析)**：将源代码转换成更加抽象的表示方法（例如抽象语法树）
- **Transform(转换)**：对（抽象语法树）做一些特殊处理，让它符合编译器的期望
- **Generate(代码生成)**：将第二步经过转换过的（抽象语法树）生成新的代码

## 图片的懒加载和预加载

预加载：提前加载图片，当用户需要查看时可直接从本地缓存中渲染

懒加载：主要目的就是作为服务器前端的优化，减少请求数/延迟请求 ，减轻服务器压力

## mouseover 和 mouseenter 的区别

- mouseenter 鼠标移动到元素上时触发 只会触发自身盒子 因为不会冒泡 。 同样和 mouseleave 搭配使用
- mouseover 鼠标经过自身盒子 以及 子盒子都会触发 与 mouseout 搭配使用

## clientHeight,scrollHeight,offsetHeight , 以 及 scrollTop,offsetTop,clientTop 的区别？

- clientHeight: 表示可视区域的高度，不包含 border 和 滚动条
- offsetHeight : 表示可视区域的高度，包含了 border 和 滚动条
- scrollHeight : 表示了所有区域 的高度，包含了因为滚动被隐藏的部分
- clientTop ： 表示边框 border 的厚度，在未指定的情况下一般为 0
- scrollTop : 滚动后被隐藏的高度

## eval 是做什么 ？

eval（string）函数可以计算某个字符串，并执行其中的 javascript 代码 。

```
eval("x=10;y=10;document.write(x*y)")
// 100
```

## 性能优化

- 减少 HTTP 请求
- 使用内容发布网络 （CDN）
- 添加本地缓存
- 压缩资源文件
- 将 CSS 样式表 放在顶部，把 javascript 放在底部
- 避免使用 CSS 表达式
- 减少 DNS 查询
- 使用外部 javascript 和 css
- 避免重定向
- 图片 lazyLoad

## Virtual DOM 作用是什么？

Virtual DOM 其实就是一棵以 JavaScript 对象( VNode 节点)作为基础的树，用对象属性来描述节点，实际上它只是一层对真实 DOM 的抽象。最终可以通过一系列操作使这棵树映射到真实环境上。原生 DOM 操作的运行速度 远没有 javascript 的运算速度快，因此，把大量的 DOM 操作搬运到 Javascript 中，运用 patching 算法来计算出真正需要更新的节点，最大限度地减少 DOM 操作，从而显著提高性能。

其实**虚拟 DOM 在 Vue.js 主要做了两件事：**

- 提供与真实 DOM 节点所对应的虚拟节点 vnode
- 将虚拟节点 vnode 和旧虚拟节点 oldVnode 进行对比，然后更新视图

## 简单介绍一下 promise

Promise 是一个对象，保存着未来将要结束的事件，解决回调地狱，他有两个特征：

- 对象的状态不受外部影响，Promise 对象代表一个异步操作，有三种状态，pending 进行中，fulfilled 已成功，rejected 已失败 ，**只有异步操作的结果才可以决定当前是哪一种状态**，任何其他操作都无法改变这个状态。
- 一旦状态改变，就不会再变，promise 对象状态改变只有两种可能，从 pending
  改到 fulfilled 或者从 pending 改到 rejected，只要这两种情况发生，状态就凝固了，不会再改变，这个时候就称为定型 resolved,

## 判断数据类型的方法

- typeof 返回值有 Number、String、Boolean、undefined、Object、Function、Symbol, 缺点 : 数组、对象、null 都会被判断为 object

- instanceof 缺点：只能判断对象是否存在于目标对象的原型链上

- constructor 所有对象都会从它的原型上继承一个 `constructor` 属性：

  ```js
  var o = {};
  o.constructor === Object; // true

  var o = new Object();
  o.constructor === Object; // true

  var a = [];
  a.constructor === Array; // true

  var a = new Array();
  a.constructor === Array; // true

  var n = new Number(3);
  n.constructor === Number; // true
  ```

- Object.prototype.toString.call( ) 最好的检测方式

## 对象深拷贝与浅拷贝

```js
//浅拷贝
1. Object.assign(target,source)
2.递归迭代
function simpleClone(obj) {
         var newObj = {};
         for (i in obj) {
           newObj[i] = obj[i]
         }
         return newObj;
}
4. Object.create()

//深拷贝1
function deepClone(obj) {
      if (!obj || typeof obj !== "object") return;
      let newObj = Array.isArray(obj) ? [] : {};
      for (let key in obj) {
        if (obj.hasOwnProperty(key)) { // 判断某个对象中是否有某个属性
          newObj[key] = typeof obj[key] === "object" ? deepClone(obj[key]) : obj[key];
        }
      }
      return newObj;
}
// 深拷贝2
  var obj = {
      myname: 'Abin',
      myage: 18,
      myhobby: 'computer'
    }
    var deepCopy = JSON.parse(JSON.stringify(obj));
    console.log(deepCopy === obj);// false
// 深拷贝3
es6对象扩展运算符。
 var newObj = {
      ...obj
    }
```

## 数组扁平化

将一个多维数组转化为一维数组

1. Array.prototype.flat()

   ```js
   const arr = [1, 2, [3, 4]];
   console.log(arr.flat()); // 1,2,3,4
   ```

2. 迭代

   ```js
   const res = [];
   const fn = (arr) => {
     for (let i = 0; i < arr.length; i++) {
       if (Array.isArray(arr[i])) {
         fn(arr[i]);
       } else {
         res.push(arr[i]);
       }
     }
   };
   ```

3. reduce 方法

   ```js
   function flatten(arr) {
     return arr.reduce((result, item) => {
       return result.concat(Array.isArray(item) ? flatten(item) : item);
     }, []);
   }
   ```

## JS 函数柯里化

将**多变量函数拆解为单变量的多个函数的依次调用**,可以从高元函数动态地生成批量的低元的函数。简单讲：只传递给函数一部分参数来调用它，让它返回一个函数去处理剩下的参数。你可以一次性地调用 curry 函数，也可以每次只传一个参数分多次调用。

![image-20210910194645342](https://techliuimg.oss-cn-beijing.aliyuncs.com/img/202110231658965.png)

作用：

- 参数复用
- 提前返回
- 延迟运行

## get 请求传参长度的误区

误区：get 请求参数的大小存在限制，而 post 请求的参数大小是无限制的。

实际上 HTTP 协议未规定 get 和 post 的请求长度是多少，给 get 请求参数的限制是来源于浏览器 或 web 服务器。 **浏览器或 web 服务器限制了 url 长度。**

## 如何判断一段脚本是运行在浏览器上还是服务器上 ？

根据浏览器的独有全局属性 **window document xmlHttpRequest** 等 进行 判断，如果不等于 undefined 则表示运行在 浏览器上 。 根据服务器的独有全局属性 **process** 来判断是否在 服务器上 。

## 去除字符串首尾空格

- 使用正则表达式 以及 str.replace( )
- 使用字符串下的方法 trim( )

```js
var str = "  hello,world  ";
console.log(str.trim());

let str2 = "  Abin   ";
let reg = /(^\s*)|(\s*$)/g;
console.log(str2.replace(reg, ""));
```

## for..in 和 for..of 的区别 ？

相同点：都是用来遍历**属性**的

**不同点**：

- for in 是遍历属性的，而**for..of 是用来遍历具有 iterator 接口的数据结构如 set map arguments**

  ```js
  // for of 遍历对象   Object.keys
  function Foo() {
    this.name = "Abin";
    this.age = 20;
  }
  var foo = new Foo();
  for (var keys of Object.keys(foo)) {
    console.log(keys); // name  age
  }

  // 获取object或者具有iterator接口的数据类型 的 size
  var obj = {
    name: "abin",
    age: 14,
    hoby: "cat",
  };
  var len = Object.keys(obj).length;
  console.log(len); // 3
  ```

- 在遍历数组时，for in 会输出 索引值，而 for..of 会输出数组每一位的值

- 在 for in 和 for of 中是可以使用 break 、continue 的，而 forEach 是不可以的。

## 为什么 typeof null 是 Object ?

因为在 JavaScript 中，不同的对象都是使用二进制存储的，如果**二进制前三位都是 0 的话，系统会判断为是 Object 类型**，而 null 的二进制全是 0，自然也就判断为 Object 。其他标识符有： 000 对象、1 整型、010 双精度类型、100 字符串、110 布尔类型

## Array.prototype.slice.call

Array.prototype.slice.call (arguments) 能够将**具有 length 属性的对象**转成数组，你只要将该 slice 方法绑定到这个对象上

```js
// 类数组
var a = {
  length: 2,
  0: "first",
  1: "second",
};

var result = Array.prototype.slice.call(a);
console.log(result); // [ 'first', 'second' ]

var aa = {
  length: 2,
};
var result1 = Array.prototype.slice.call(aa);
console.log(result1); // [undefined,undefined]
```

## 字面量创建对象和 new 创建对象有什么区别 ？ new 内部实现了什么?

字面量：

- 字面量创建对象更简单，方便阅读
- 不需要作用域解析，速度更快

new 内部：

- 创建一个新的对象
- 使新的对象的`__proto__`指向原函数的 prototype
- 改变 this 指向（指向新的 obj ）并执行该函数，执行结果保存起来作为 result
- 判断该 result 是不是 null 或 undefined,如果是则返回之前的新对象，如果不是则返回 result

**手写一个 new**

```js
function myNew(fn, ...args) {
  //创建一个空的对象
  let obj = {};
  // 使空对象的隐式原型指向原函数的显示原型
  obj.__proto__ = fn.prototype;
  // this指向obj
  let result = fn.apply(obj, ...args);
  // 返回
  return result instanceof Object ? result : obj;
}
```

## 字面量、new 出来的对象和 Object.create(null) 创建出来的对象有什么区别 ？

- 字面量和 new 创建出来的对象会继承 Object 的方法和属性，他们的隐式原型会指向 Object 的显示原型。
- 而 Object.create(null) 创建出来的对象原型为 Null ,作为原型链的顶端，自然也没有继承 Object 的方法和属性

## Typeof NaN 结果 ？

NaN 是 not a number 的缩写，表示“不是一个数字”

NaN 和任何变量都不相等，包括 NaN 自己。判断一个变量是不是 NaN，可以用 isNaN( ) 函数

## 如何校验 JSON 格式 ？

如果数据是 JSON 格式，那么他通过 JSON.parse()之后返回的是 object 类型。

```js
var data = { code: 0, msg: "success" };
function isJson(data) {
  var obj = JSON.parse(data);
  if (typeof obj == "object" && object) {
    return obj;
  }
}
isJson(data);
```

## 如何判断一个对象是空对象？

1. 使用 JSON 自带的 stringify 方法来判断:

   ```js
   if (Json.stringify(Obj) == "{}") {
     console.log("空对象");
   }
   ```

2. 使用 ES6 新增的 Object.keys（）来判断：

   ```js
   if (Object.keys(Obj).length < 0) {
     console.log("空对象");
   }
   ```

## 对 rest 参数的理解

扩展运算符被用在函数形参上时，它还可以把一个分离的参数序列整合成一个数组：

```js
function mutiple(...args) {
  // ...args  [1,2,3,4]
  let result = 1;
  for (var val of args) {
    result *= val;
  }
  return result;
}
mutiple(1, 2, 3, 4); // 24
```

## JavaScript 类数组对象的定义 ？

一个拥有 length 属性和若干索引属性的对象称为 类数组对象，不同于数组，不能使用数组的方法。

常见的 类数组对象有 arguments 和 DOM 方法的结果 。

```js
var arrlike = { 1: 1, 2: 2, 3: 3, z: "z", a: "a", length: 10 };
```

如何使用它呢？ 那就用数组的方法把它**转换成普通数组**。

```js
Array.prototype.slice.call(arrayLike);
Array.prototype.splice.call(arrayLike, 0);
Array.prototype.concat.apply([], arrayLike);
Array.from(arrayLike);
```

## js 数组/字符串基础变换

<img src="https://techliuimg.oss-cn-beijing.aliyuncs.com/img/202110271548896.png" alt="image-20211027154841289"  />

## 操作 dom 元素并修改样式文本

```js
<div class="demo">
    <p id="p1">这是一个段落</p>
  </div>
  <script>
    var para = document.createElement('p'); // 创建元素
    var node = document.createTextNode('这是一个新的段落');
    para.appendChild(node); // 追加节点
    var element = document.getElementsByClassName('demo')[0];
    element.appendChild(para);
    // element.insertBefore(新元素,旧元素)  // 在前面追加
    //element.removeChild(para) // 移除元素
    para.style.color = 'pink' // 设置颜色
    para.setAttribute('class', 'p2')// 用getAttribute来获取
  </script>
```

![image-20211030105520535](https://techliuimg.oss-cn-beijing.aliyuncs.com/img/202110301055807.png)

# 高级

## 如何实现一个 sleep 效果 ？

> 实现程序的延时

通过 Promise 实现

```js
function sleep(ms) {
  var temp = new Promise((resolve) => {
    console.log(111);
    setTimeout(resolve, ms);
  });
  return temp;
}
sleep(500).then(function () {
  console.log(222);
});
```

## JavaScript 原型 prototype ** proto **

- 每一个构造函数都有一个 prototype 属性，也称为**原型对象**，构造函数中共有的方法都在存在这个对象中，从而所有实例对象都可以共用这个方法。
- 每一个对象都有一个 ** proto ** 属性， 指向构造函数中的 prototype ，所以实例对象可以访问 构造函数中 prototype 属性中的方法

## `Funciton.__proto__`(getPrototypeOf)是什么 ？

获取一个对象的原型，在 chrome 中可以通过 `__proto__`的形式，在 ES6 中可以通过 getPrototypeOf 的形式

Function 是由什么对象来继承的呢 ？ 可以试一试,结果显示，**Function 的原型 还是 Function**

```3# js
Function.__proto__ == Function.prototype // true
```

## Object.getPrototypeOf( )

Object.getPrototypeOf() 方法返回指定对象的原型 。 相当于返回 指定对象的 `__proto__`

## ES6 中的数组高级方法

- forEach()它只是对数组中的每一项运行传入的函数。这个方法没有返回值，本质上与使用 for 循环迭代数组一样

- filter( ) 过滤，对数组中的每一项运行给定函数，返回该函数会返回 true 的项目组组成的数组

- map( ) 映射，返回一个数组而这个数组的每一项都是在原始数组中的对应项上运行传入函数的结果

- some( ) 数组中的每一项运行给定函数，只要传入的函数对数组中的某一项返回 true，就会返回 true

- every( ) 数组中的每一项运行给定函数，如果该函数对每一项都返回 true，则返回 true

- reduce( ) 归并方法

  ```js
  arr.reduce(function (prev,cur,index,array){...}, value)
  (1) prev当前的上一个值,(2) cur 当前值,(3) index 当前值索引值,（4） array 原数组,(5) value  (prev初始值)

  // 数组中的成员值的累加求和
  let sum = array1.reduce(function(prev,cur,index,array){
  			 console.log(prev,cur,index,array);
  			 return prev + cur;
  		   },0);
  		   console.log(sum);
  ```

## Map 和 Object 的区别

- 一个 object 的键只能是字符串或者 Symbol,但一个 Map 的键可以是任意值
- Map 中的键值是有序的(FIFN 规则)，而添加到对象中的键则不是。
- Map 的键值对个数可以从 size 属性获取，而 Object 的键值对个数只能手动计算。Object.keys(obj)
- Object 都有自己的原型，原型链上的键名有可能和你自己在对象上的设置的键名产生冲突。

## 什么是 generator ？ 什么是 async.. await ?

generator**带指针的函数**， yield 执行下一个指针任务 ，作用： 可以将异步转为同步。

注意： **yield 没有返回值 undefined**,但如果，next() 带参数， 是上一个 yield 所在的表达式的返回值 。

generator 是怎么使用的 ？

- 首先生成器是一个函数，用来返回迭代器的。
- 调用生成器后不会立即执行，而是通过返回的迭代器来控制这个生成器的一步一步执行的。
- 通过调用迭代器的 next 方法来请求一个一个的值，返回的对象有两个属性，一个是 value，也就是值，另一个是 done ，是个布尔类型 ，done 为 true 说明生成器函数执行完毕，没有可返回值了。
- done 为 true 后继续调用迭代器的 next 方法 ，返回值 value 为 undefined.

```js
generator 使用 , next() 不带参数
function* demo() {
    yield 1;
    yield 2;
    yield 3;
    return 'over';

}
let it = demo();
console.log(it.next());  // { value: 1, done: false }
console.log(it.next());  // { value: 2, done: false }
console.log(it.next());  // { value: 3, done: false }
console.log(it.next());  // { value: 'over', done: true}

------------------------------------------------------------------
generator使用例子2：
 yield 没有返回值 undefined,但如果，next() 带参数， 是上一个 yield 所在的表达式的返回值 。
 function* demo2(x) {
    let y = yield x * (x + 1);
    let z = yield (y / 3);
    return x + y + z;
}

let it2 = demo2(3);
console.log(it2.next()); // { value: 12, done: false }
console.log(it2.next(12)); // y = 12 { value: 4, done: false }
console.log(it2.next(4));//  z = 4 { value: 19, done: true }  (3 + 12 + 4) =19 */

console.log(it2.next()); // { value: 12, done: false }
console.log(it2.next()); // y = undefined { value: NaN, done: false }
console.log(it2.next());//  z = undefined { value: NaN, done: true }
```

async...await 是 generator 的语法糖，实际操作比如 koa 框架用 可以将异步转为同步 ajax 数据通讯

特点：

- async 的函数返回的是 Promise 的对象
- 使用 await 时当前的函数一定是 async

- 在 async 方法中，第一个 await 之前的代码会同步执行 ，await 之后的代码是异步执行

  ```js
  async...await 的使用例子：
  let fs = require('fs').promises; // 返回Promise对象
  async function fn2() {
      let data1 = await fs.readFile('data1.txt');
      let data2 = await fs.readFile(data1);
      return data2.toString();
  }
  fn2().then(res => {
      console.log(res);
  }, err => {
      console.log(err);
  })
  ```

## 什么是 iterator 迭代器？

**迭代器是一种接口**，是一种机制。

**为各种不同的数据结构提供统一的访问(读取)机制**，任何数据结构只要部署 iterator 接口，就可以完成遍历操作（即依次处理该数据结构的所有成员）

Iterator 的作用有三个：

- 为各种数据结构，提供一个统一的、简便的访问接口；
- 使得数据结构的成员能够按某种次序排列；
- 主要供 “for .. of ”消费。

```js
原生具备 Iterator 接口的数据结构如下。

* Array
* Map
* Set
* String
* 函数的 arguments 对象
* NodeList 对象

// 接口实现 Iterator
// 遍历器对象本质上就是一个指针对象。每次调用next方法都会返回数据结构的当前成员的信息 (value 和 done 两个属性的对象 )
function getIterator(list) {
    var i = 0;
    return {
        next: function() {
            var done = (i >= list.length);
            var value = !done ? list[i++] : undefined;
            return {
                done: done,
                value: value
            };
        }
    };
}
var it = getIterator(['a', 'b', 'c']);
console.log(it.next());// {value: "a", done: false}
console.log(it.next());// {value: "b", done: false}
console.log(it.next());// {value: "c", done: false}
console.log(it.next());// "{ value: undefined, done: true }"
console.log(it.next());// "{ value: undefined, done: true }"
console.log(it.next());// "{ value: undefined, done: true }"


//ES6里规定，只要在对象的属性上部署了 Iterator接口，具体形式为给对象添加Symbol.iterator属性，此属性指向一个迭代器方法，这个迭代器会返回一个特殊的对象 - 迭代器对象。
//Iterator 的应用例子2：
var arr = [100,200,300];
var iteratorObj = arr[Symbol.iterator]("Symbol.iterator");
//得到迭代器方法，返回迭代器对象
console.log(iteratorObj.next());


// 数组中也内置了 iterator，我们可以用 forof直接遍历属性
let arr = ['aa', 'bb', 'cc'];
for (let key of arr) {
  console.log(key);
}

// aa bb cc
```

## 如何将普通对象转成可迭代对象

```js
var obj = {};
for (var k of obj) {
  // 不可迭代
}

var iterableObj = {
  items: [100, 200, 300],
  [Symbol.iterator]: function () {
    var self = this;
    var i = 0;
    return {
      next: function () {
        var done = i >= self.items.length;
        var value = !done ? self.items[i++] : undefined;
        return {
          done: done,
          value: value,
        };
      },
    };
  },
};

//遍历它
for (var item of iterableObj) {
  console.log(item); //100,200,300
}
```

## 箭头函数特点

- 函数体内的 `this`对象，就是定义时所在的对象（外层函数的 this）。
- 不可以当作构造函数，也就是说，不可以使用 `new` 命令，否则会抛出一个错误。
- 没有原型
- 不可以使用 `arguments` 对象，该对象在函数体内不存在。如果要用，可以用 rest 参数代替 。
- 不可以使用 `yield` 命令，因此箭头函数不能用作 generator 函数

```js
let obj = {
  name: "小明",
  age: 22,
  sayName() {
    setTimeout(() => {
      console.log(`我是${this.name}`);
    }, 500);
  },
};
obj.sayName(); // 我是小明
```

## es6 中 class 语法及 与 prototype 的关系

- **constructor( ) 方法，是初始化自动调用的的构造方法** 没有显示定义，也会默认添加

- this 关键字则代表实例对象。

- **类的所有方法都定义在 类的 prototype 属性上面**，实例的属性除非显式定义在 其本身（定义在 this 对象上），否则都是定义在原型上 （定义在 class 上）

- 类的所有实例共享一个原型对象。

  ```js
  var p1 = new Point(2, 3);
  var p2 = new Point(3, 2);
  p1.__proto__ === p2.__proto__; //true。
  //p1和p2都是Point的实例，它们的原型都是Point.prototype，所以__proto__属性是相等的。
  ```

- prototype 对象的 constructor( )属性，直接指向“类”的本身。

  ```js
  Person.prototype.constructor === Person; // true
  ```

- 类的内部所有定义的方法，都是不可枚举的。es5 构造函数可以枚举。

  ```js
  class Point {
    constructor(x, y) {
      // ...
    }
    toString() {
      // ...
    }
  }
  Object.keys(Point.prototype);
  // []
  Object.getOwnPropertyNames(Point.prototype);
  // ["constructor","toString"]
  ```

## 描述 ES6 中类的静态方法，静态属性。

类相当于实例的原型，所有在类中定义的方法，都会被实例继承。

如果在一个方法前，加上`static` 关键字，就表示该**方法不会被实例继承，而是直接通过类来调用**，这就称为“**静态方法**” 。

**静态属性指的是 Class 本身的属性**，即 Class.propName,而不是定义在实例对象（this）上的属性。

```js
class Foo {
  static classMethod() {
    return "hello";
  }
}
Foo.classMethod(); // 'hello'
var foo = new Foo();
foo.classMethod();
// TypeError: foo.classMethod is not a function
```

特点：

- 直接在类上调用 类名.方法
- 如果静态方法包含 this 关键字，这个 this 指的是类，而不是实例
- 父类的静态方法，可以被子类继承。

```js
class Foo {
  static classMethod() {
    return "hello";
  }
}
class Bar extends Foo {}
Bar.classMethod(); // 'hello'
```

## class 类的继承

- **class 可以通过 extends 关键字实现继承**，这比 ES5 的通过修改原型链实现继承，要清晰和方便很多 。

- **子类必须在 constructor 方法中调用 super 方法**，否则新建实例时会报错。这是因为子类自己的 this 对象，必须先通过父类的构造函数完成塑造，得到与父类同样的实例属性和方法。然后再对其进行加工，加上子类自己的实例属性和方法。如果不调用 super 方法，子类就得不到 this 对象。

- 如果**子类没有定义 constructor 方法，这个方法会被 默认添加**。也就是说不管有没有显示定义，任何一个子类都有 constructor 方法

  ```js
  class ColorPoint extends Point {
    constructor(x, y, color) {
      super(x, y); // 调用父类的constructor(x, y)
      this.color = color;
    }

    toString() {
      return this.color + " " + super.toString(); // 调用父类的toString()
    }
  }

  // 静态属性
  class Foo {
    // ...
  }
  Foo.prop = 1;

  // 新写法
  class Foo {
    static prop = 1;
  }
  ```

## class 中 super 关键字的用法

super 这个关键字，既可以当作函数使用，也可以当作对象使用。在这两种情况下，他的用法完全不同。

- 第一种情况：super 作为函数调用时，代表父类的构造函数。ES6 要求，子类的构造函数必须执行一次 super 函数。

- 第二种情况：super 作为对象时，在普通方法中，指向父类的原型对象，**在静态方法中，指向父类。**

  ```js
  注意： super虽然代表了父类A的构造函数，但是返回的是子类B的实例，即super内部的this指的是B的实例，因此super()在这里相当于A.prototype.constructor.call(this)。
  例子：
  class A {
    constructor() {
      console.log(new.target.name);
    }
  }
  class B extends A {
    constructor() {
      super();
    }
  }
  new A() // A
  new B() // B
  ```

## 类的 prototype 属性和 proto 属性

大多数浏览器的 ES5 实现之中，每一个对象都有 `__proto__`属性，指向对应的构造函数的 prototype 属性。Class 作为构造函数的语法糖，同时有 `prototype`属性和 `__proto__`属性，因此同时存在两条继承链。

- 子类的`__proto__`属性，表示构造函数的继承，总是指向父类。
- 子类的`prototype`属性的`__proto__`属性，表示方法的继承，总是指向父类的 prototype 属性；0

```js
class A {}

class B extends A {}

B.__proto__ === A; // 类的继承true
B.prototype.__proto__ === A.prototype; // true
```

## 说一说 JS 中常用的继承方式有哪些？以及各个继承方式的优缺点

### **原型链继承**

优点：父类新增方法属性，子类都能访问到，简单易于实现

缺点：**子类的实例共享了父类构造函数的引用属性** 不能传参

```js
// 定义一个动物类
function Animal(name) {
  this.name = name || "Animal";
  // 实例方法
  this.sleep = function () {
    console.log(this.name + "正在睡觉");
  };
}
// 原型方法
Animal.prototype.eat = function (food) {
  console.log(this.name + "正在吃" + food);
};

// 原型链继承
// 将父类的实例作为子类的原型
// 缺点： 子类的实例共享了父类构造函数的引用值(数组 对象等)  不能传参
function Cat() {}
Cat.prototype = new Animal();
Cat.prototype.name = "cat";

var cat = new Cat();
cat.eat("fish"); //cat正在吃fish
cat.sleep(); //cat正在睡觉
```

### 构造函数继承

优点：子类构造函数 借用 父类的构造函数 解决了引用值共享的问题

缺点： 没有办法拿到原型上的方法；

```js
// 由于原型链继承  Cat.prototype = new Animals()  存在引用共享的问题 所以 采用构造函数
// 借用构造函数继承
function Animals() {
  // this.a = 1;
  this.a = [1, 2, 3, 4, 5];
}
Animals.prototype.say = function () {
  console.log("会叫");
};

function Cat() {
  Animals.call(this);
}

var cat1 = new Cat();
var cat2 = new Cat();
cat1.a.push(6);
console.log(cat1.a); // 1 2 3 4 5 6
console.log(cat2.a); // 1 2 3 4 5
```

### **组合继承**（伪经典继承）

优点：可传参

缺点：调用了两次父类的构造函数，造成了不必要的消耗

```js
// 组合继承
// 构造函数继承+ 原型链继承
// 缺点： 调用了两次父类的构造函数，造成了不必要的消耗，父类方法可复用
// 优点可传参，不共享父类引用属性
function Animals() {
  // this.a = 1;
  this.a = [1, 2, 3, 4, 5];
}
Animals.prototype.say = function () {
  console.log("会叫");
};

function Cat() {
  Animals.call(this);
}
Cat.prototype = new Animals();
var cat1 = new Cat();
var cat2 = new Cat();
console.log(cat1);
cat1.a.push(6);
cat1.say(); // 会叫
console.log(cat2.a); // 1 2 3 4 5
```

### **寄生组合继承**（经典继承）

优点：可传参，不会创建多余的实例占用内存 ，可通过 Object.assign 实现多继承

缺点：ES5 的方法 Object.create（）

```js
// 寄生组合继承
function Animals() {
  // this.a = 1;
  this.a = [1, 2, 3, 4, 5];
}
Animals.prototype.say = function () {
  console.log("会叫");
};

function Cat() {
  Animals.call(this);
}
Cat.prototype = Object.create(Animals.prototypes); // 绕开了 new Super（ ） 的实例
var cat1 = new Cat();
var cat2 = new Cat();
console.log(cat1);
cat1.a.push(6);
cat1.say(); // 会叫
console.log(cat2.a); // 1 2 3 4 5
```

### **ES6 的 extend**

```js
class Animal {
  constructor(name) {
    this.name = name;
  }
  showName() {
    alert(this.name);
  }
}
class Cat extends Animal {
  constructor(name) {
    super(name);
  }
  sayMy() {
    super.showName();
  }
}
```

## 内存泄漏、垃圾回收机制

### **什么是内存泄漏？**

内存泄漏是指**不再用的内存没有被及时释放出来**，导致该段内存无法被使用。

### **为什么会导致内存泄漏？**

内存泄漏是指我们无法再通过 js 访问某个对象，而垃圾回收机制却认为该对象还在被引用，因此垃圾回收机制不会释放该对象，导致该块内存永远无法释放，积少成多，系统会越来越卡以至于奔溃。

### 哪些情况会导致内存泄漏 ？

1. **意外的全局变量**，由于使用未声明的变量，而意外的创建了一个全局变量，而使这个变量一直保存在内存中无法被回收 。 局部变量会在函数使用完后自动回收
2. **被遗忘的定时器或回调函数** ：设置了 setInterval 定时器，而忘记取消它。如果循环函数有对外部变量的引用的话，那么这个变量就会被一直留在内存中。而无法被回收。
3. **脱离 DOM 的引用**： 获取一个 DOM 元素的引用，而后面这个元素被删除，由于一直保留了对这个元素的引用，所以他无法被回收。
4. **闭包**：不合理的使用闭包，从而导致私有变量一直被留在内存中

### **垃圾回收机制都有哪些策略？**

- 标记清除

  垃圾收集器在运行的时候会给存储在内存中的所有变量都加上标记，然后它会去掉环境中的变量以及被环境中的变量引用的变量的标记。而在此之后再被加上标记的变量将被视为准备删除的变量，原因是环境中的变量已经无法访问到这些变量了。最后垃圾收集器完成内存清除工作，销毁那些带标记的值并回收它们所占用的内存空间。

- 引用计数

  跟踪记录每个值被引用的次数，当声明一个变量并给该变量赋值一个引用类型的值的时候，该值的计数+1，当该值赋值给另一个变量的时候，该计数+1，当该值被其他值取代的时候，该计数-，当计数变为的时候，说明无法访问该值了。垃圾回收机制清除该对象。

# JavaScript 的执行机制

### 为什么 JavaScript 是单线程 ？

javascript 语言的一大特点就是 单线程 ，因为作为浏览器脚本语言，**主要用途就是与用户互动，操作 Dom,这决定了它只能是单线程** 。 假定 JavaScript 同时有两个线程，一个线程用来在某个 DOM 节点上 添加内容，一个线程用来删除内容，浏览器该以哪个为准呢 ？

### 任务队列

单线程意味着所有任务都需要排队，前一个任务结束，后一个任务才能继续，如果前一个任务耗时很长，后一个任务就必须等着。显然这样效率也会大大减低，CPU 性能也没有充分利用。

于是，提出了同步任务和异步任务这一说，同步任务指的是，在主线程执行的任务形成一个执行栈，只有前一个任务执行完毕，才能执行后一个任务，异步任务指的是，不进入主线程，而进入`任务队列` 的任务，只有任务队列通知主线程某个任务可以执行了，才会进入主线程执行 。

### EventLoop

主线程从`任务队列`中读取事件，这个过程是循环不断，所以整个的运行机制又称为 EventLoop

![image-20210924110853124](https://techliuimg.oss-cn-beijing.aliyuncs.com/img/202110231658626.png)

主线程运行的时候，会产生`堆(heap)`和 `栈(stack)`，栈中的代码调用各种外部 API，他们在“任务队列”中加入各种事件（click,load,done），只要栈中的代码执行完毕，主线程就会去读取 “任务队列” 依次执行那些事件所对应的回调函数 。

### 什么是 JS 的 eventLoop (事件环)？

js 主线程是单线程，异步任务通过事件环（eventloop）以队列存储，如果主线程同步代码执行完，就拿事件队列的异步任务。异步任务分为 宏任务，微任务 ；

**宏任务 macrotask**有：setTimeout、setInterval 、setImmediate 、MessageChannel

**微任务 microtask**有：promise.then( ) process.nextTick( )

```js
js事件环的执行例子：
setTimeout(() => {
    console.log('setTimeout1');
    Promise.resolve().then(data => {
        console.log('then3');
    });
}, 1000);
Promise.resolve().then(data => {
    console.log('then1');
});
Promise.resolve().then(data => {
    console.log('then2');
    setTimeout(() => {
        console.log('setTimeout2');
    }, 1000);
});
console.log(2);
-----------------------------------------------------
output:
2
then1
then2
setTimeout1
then3
setTimeout2

1.先执行栈中的内容，也就是同步代码，所以2被输出出来；
2.然后清空微任务，所以依次输出的是 then1 then2；
3.因代码是从上到下执行的，所以1s后 setTimeout1 被执行输出；
4.接着再次清空微任务，then3被输出；
5.最后执行输出setTimeout2
```

### 执行上下文

- **什么是执行上下文?**

  简而言之，执行上下文就是**当前 JavaScript 代码被解析和执行所在环境**的抽象概念，JavaScript 中运行任何的代码都是在执行上下文中运行。

- **执行上下文的类型**

  执行上下文共有三种类型：

  - 全局执行上下文

    这是默认的、最基础的执行上下文，**不在任何函数中代码都位于全局执行上下文中**，它做了两件事：1. 创建一个全局对象，**在浏览器中这个全局对象就是 window 对象**。2.将 this 指针指向这个全局对象，一个程序中只能存在一个全局执行上下文。执行 js 的时候就压入栈底，关闭浏览器的时候才弹出。

  - 函数执行上下文

    每次函数调用时，都会新建一个函数执行上下文。

    执行上下文分为创建阶段和执行阶段：**创建阶段**，函数环境会创建变量对象：arguments 对象 函数声明（并赋值）、变量声明（不赋值）、函数表达式声明（不赋值）；会确定 this 指向，会创建作用域链。 **执行阶段**，变量赋值、函数表达式赋值，使变量变成编程活跃对象。

  - eval 执行上下文

    运行在 eval 函数中的代码也获得了自己的执行上下文

### 执行栈

- 首先栈的特点是先进后出
- 当进入一个执行环境，就会创建出它的执行上下文，然后进行压栈，当程序执行完成时，它的执行上下文就会被销毁，进行弹栈。
- 栈底永远是全局环境的执行上下文，栈顶永远是正在执行函数的执行上下文。
- 只有浏览器关闭的时候全局执行上下文才会弹出。

# ES6

### fs 模下的 Stream 流

> node.js 中一般读取或写入时用的是 fs.readFile()或者 fs.writeFile()，但是在处理大块文件时用到 fileStream 流。

**简单的文件读取操作：**

```js
var fs = require("fs");
var readerStream = fs.createReadStream("input.txt");
// 设置编码为 utf8。
readerStream.setEncoding("UTF8");

// 处理流事件 --> data, end, and error
readerStream.on("data", function (chunk) {
  // 当有数据可读时触发。
  data += chunk;
});

readerStream.on("end", function () {
  // 没有更多的数据可读时触发。
  console.log(data);
});
// error - 在接收和写入过程中发生错误时触发。
// finish - 所有数据已被写入到底层系统时触发。
```

简单的写入 操作 ：

```js
var fs = require("fs");
var data = "菜鸟教程官网地址：www.runoob.com";

// 创建一个可以写入的流，写入到文件 output.txt 中
var writerStream = fs.createWriteStream("output.txt");

// 使用 utf8 编码写入数据
writerStream.write(data, "UTF8");

// 标记文件末尾
writerStream.end();

// 处理流事件 --> finish、error
writerStream.on("finish", function () {
  console.log("写入完成。");
});

writerStream.on("error", function (err) {
  console.log(err.stack);
});

console.log("程序执行完毕");
```

管道流： 顾名思义，从一个文件流到另一个文件

```js
var fs = require("fs");

// 创建一个可读流
var readerStream = fs.createReadStream("input.txt");

// 创建一个可写流
var writerStream = fs.createWriteStream("output.txt");

// 管道读写操作
// 读取 input.txt 文件内容，并将内容写入到 output.txt 文件中
readerStream.pipe(writerStream);

console.log("程序执行完毕");
```

### **简单请求**须满足以下条件：

- 请求方式为 HEAD、GET、POST

- http 头信息不超出以下字段：Accept、Accept-Language 、 Content-Language、 Last-Event-ID、 Content-Type(限于三个值：application/x-www-form-urlencoded、multipart/form-data、text/plain)

### **为什么分简单请求和负责请求？ 因为 服务器对这两种请求的处理方式不一样 。**

**复杂请求**

- 非简单请求是那种对服务器有特殊要求的请求，比如请求方法是 PUT / DELETE,或者 Content-Type 字段的类型是 application/json。
- 非简单请求的 CORS 请求，会在正式通信之前，增加一次 HTTP 查询请求，称为"预检"请求
- 浏览器先询问服务器，当前网页所在的域名是否在服务器的许可名单之中，以及可以使用哪些 HTTP 动词和头信息字段。只有得到肯定答复，浏览器才会发出正式的`XMLHttpRequest`请求，否则就报错。

"预检"请求用的请求方法是`OPTIONS`，表示这个请求是用来询问的。头信息里面，关键字段是`Origin`，表示请求来自哪个源。除了`Origin`字段，"预检"请求的头信息包括两个特殊字段。

- **Access-Control-Request-Method ** 用来列出浏览器的 CORS 请求会用到哪些 HTTP 方法， 如 put
- **Access-Control-Request-Headers** 该字段是一个逗号分隔的字符串，指定浏览器 CORS 请求会额外发送的头信息字段，上例是`X-Custom-Header`。

**预检请求的回应**

服务器收到"预检"请求以后，检查了`Origin`、`Access-Control-Request-Method`和`Access-Control-Request-Headers`字段以后，确认允许跨源请求，就可以做出回应。
