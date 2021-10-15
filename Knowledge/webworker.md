# WebWorker

>web worker 是运行在后台的 JavaScript，独立于其他脚本，不会影响页面的性能。您可以继续做任何愿意做的事情：点击、选取内容等等，而此时 web worker 在后台运行。

## 检测浏览器是否支持web worker

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

## 创建 web worker 文件

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

## HTML中使用

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

## 注意事项

由于 web worker 位于外部文件中，它们无法访问下列 JavaScript 对象：

- window 对象
- document 对象
- parent 对象