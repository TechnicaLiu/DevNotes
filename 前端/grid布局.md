# Grid布局

## 基本概述

>它将网页划分成一个个网格，可以任意组合不同的网格，做出各种各样的布局。

他与flex布局的区别在哪里？

1. Flex 布局是轴线布局，只能指定"项目"针对轴线的位置，可以看作是**一维布局**。
2. Grid 布局则是将容器划分成"行"和"列"，产生单元格，然后指定"项目所在"的单元格，可以看作是**二维布局**。

参考阮一峰笔记： [CSS Grid 网格布局教程 - 阮一峰的网络日志 (ruanyifeng.com)](http://www.ruanyifeng.com/blog/2019/03/grid-layout-tutorial.html)

## 基本概念

### 容器和项目

采用网格布局的区域，称为"容器"（container）。容器内部采用网格定位的子元素，称为"项目"（item）。

```html

<div>
  <div><p>1</p></div>
  <div><p>2</p></div>
  <div><p>3</p></div>
</div>
```

上面的代码中 最外层的div 就是 容器，里层的三个div子元素就是 项目， 而p元素不属于项目。

### 行和列

<img src="https://gitee.com/youngstory/images/raw/master/img/202110231116662.png" alt="image-20211023111654563" style="zoom:50%;" />

   容器里面的水平区域称为"行"（row），垂直区域称为"列"（column）。

### 单元格

行和列的交叉区域，称为单元格。

正常情况下，n行和n列会产生 n*m个单元格。

### 容器属性

grid布局属性一般划分为 容器属性和项目属性； 分别对应 容器和 项目 ；

1. display属性

   ```js
   // 默认情况下，容器元素都是块级元素 
   div {
     display: grid;
   }
   // 也可以转为 行内元素 
   div {
     display: inline-grid;
   }
   ```

2. grid-template-columns/ grid-template-rows 属性

   ```js
   //这两个属性指定了  行和列宽度 
   .container {
     display: grid;
     grid-template-columns: 100px 100px 100px;
     grid-template-rows: 100px 100px 100px;
   }
   ```

   同时也支持 百分比    **repeat()**   **auto 关键字** **uto-fill 关键字**  **fr 关键字** minmax()  minmax()**

   具体参数设置  查看文档 [CSS Grid 网格布局教程 - 阮一峰的网络日志 (ruanyifeng.com)](http://www.ruanyifeng.com/blog/2019/03/grid-layout-tutorial.html)

3. grid-row-gap 属性， grid-column-gap 属性， grid-gap 属性

   ```js
   // grid-row-gap 指定了行间距   //  grid-column-gap  指定了列间距  
   
   .container {
     grid-row-gap: 20px;
     grid-column-gap: 20px;
   }
   ```

4. grid-auto-flow  属性

   ```js
   // 默认 容器中的子元素 是 先行后列 的顺序进行排列的，这是由 grid-auto-flow 决定的
   //  默认值 row， column是 先列后行 
   
   grid-auto-flow: row/column/row dense/column dense 
   
   
   // 设为row dense，表示"先行后列"，并且尽可能紧密填满，尽量不出现空格。 
   ```

5. justify-items 属性， align-items 属性， place-items 属性 

   **属性可选值有： start、end  、center、stretch  拉伸** 

   ```js
   // justify-items  规定了 单元格内容的水平位置
   .container {
     justify-items: start;
   }
   
   // align-items   规定了 单元格内容的垂直位置
   .container {
     align-items: start;
   }
   // place-items属性是align-items属性和justify-items属性的合并简写形式。
   place-items: <align-items> <justify-items>;
   
   ```

6. justify-content 属性， align-content 属性， place-content 属性

   **属性值有: start | end | center | stretch | space-around | space-between | space-evenly;**

   ```js
   // justify-content  整个内容区域在容器里面的水平位置（左中右）
   // align-content 整个内容区域在容器里面的垂直位置 （上中下）
   .container {
     justify-content: start | end | center | stretch | space-around | space-between | space-evenly;
     align-content: start | end | center | stretch | space-around | space-between | space-evenly;  
   }
   // place-content 是 align-content 属性和justify-content属性的合并简写形式。
   ```

7. grid-auto-columns 属性， grid-auto-rows 属性

   `grid-auto-columns`属性和`grid-auto-rows`属性用来设置，浏览器自动创建的多余网格的列宽和行高。

   ```js
   .container {
     display: grid;
     grid-template-columns: 100px 100px 100px;
     grid-template-rows: 100px 100px 100px;
     grid-auto-rows: 50px; 
   } 
   // 上面代码指定新增的行高统一为50px（原始的行高为100px）。
   
   ```

### 项目属性

1. grid-column-start 属性， grid-column-end 属性， grid-row-start 属性， grid-row-end 属性

   项目的位置是可以指定的，具体操作就是指定四个边框 分别定位在哪根网格线

   ```js
   grid-column-start属性：左边框所在的垂直网格线
   grid-column-end属性：右边框所在的垂直网格线
   grid-row-start属性：上边框所在的水平网格线
   grid-row-end属性：下边框所在的水平网格线
   
   .item-1 {
     grid-column-start: 2;
     grid-column-end: 4;
   }
   
   // 上述代码展示了 item-1的盒子  左边框在第二跟网格线上，右边框在第四根垂直网格线 
   
   
   ```

2. grid-area属性

   指定了项目放在哪一个区域 

   ```css
   .item-1 {
     grid-area: e;
   }
   ```

3. justify-self 属性， align-self 属性， place-self 属性

   作用和 跟`justify-items`属性/ `align-items` 属性的用法完全一致，但只作用于单个项目。

   `place-self`属性是`align-self`属性和`justify-self`属性的合并简写形式。

# rem布局

- em单位 相对于父元素字体大小来说的 

- rem是相对于html元素的字体大小

- 可以通过html里面的文字大小来改变页面中的元素的大小可以整体控制

# 媒体查询

> @ media可以针对不同的屏幕尺寸设置不同的样式

1. 语法规范 

<img src="https://gitee.com/youngstory/images/raw/master/img/202110231642787.jpeg" alt="img"  />

2. 在html > head 中 ![img](https://gitee.com/youngstory/images/raw/master/img/202110231643467.jpeg)

3. 在CSS中

    ![img](https://gitee.com/youngstory/images/raw/master/img/202110231643606.jpeg)

4. mediatype查询类型![img](https://gitee.com/youngstory/images/raw/master/img/202110231643893.jpeg)

# flex布局

幕布笔记：https://www.mubucm.com/doc/tgEh2_Uato