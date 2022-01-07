# sourceMap

> 一个存储源代码与编译代码(通常经过压缩混淆和代码转换)对应位置映射的信息文件。它能够将经过压缩、混淆、合并的代码还原回未打包状态，帮助开发者在生产环境中精确定位问题发生的行列位置。

在前端的工作中主要是用来解决以下三个方面出现的 debug 问题：

​	a. 代码压缩混淆后
​	b. 利用 sass 、typeScript 等其他语言编译成 css 或 JS 后
​	c. 利用 webpack 等打包工具进行多文件合并后

当我们进行调试代码的时候，经过压缩和处理的代码很难找到bug地方，所以我们需要借助sourceMap 来提供给我们 出错的位置信息。

## 使用

### 环境设置

google浏览器 打开 设置 - source 进行勾选 

<img src="https://gitee.com/youngstory/images/raw/master/img/202201061107899.png" style="zoom:67%;" />

### .map文件生成

![image-20220106112539152](https://gitee.com/youngstory/images/raw/master/img/202201061125242.png)