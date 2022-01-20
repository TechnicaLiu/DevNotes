Gulp入门

## 定义

gulp是grunt功能类似的**前端项目构建工具**，也是基于Node.js的自动任务运行器。

能自动化的完成 javascript/coffee/sass/less/html/image/css等文件的合并，压缩，检查，监听文件变化，浏览器自动刷新，测试等任务。

gulp更高效（异步多任务）更易于使用，插件高质量。

官网  [快速入门 · gulp.js 中文文档 (gulpjs.com.cn)](https://www.gulpjs.com.cn/docs/getting-started/quick-start/)

## 基本使用

1. 全局安装gulp  (全局和项目都要安装gulp)

   ```
   npm i -g gulp
   ```

2. 生成一个package.json·

   ```
   npm init 
   //记得要把入口文件（entry point)设置为gulpfile.js 
   ```

3. 安装gulp作为项目依赖 ·

   ```
   npm install --save-dev gulp@3.9.1
   ```

   **gulp 3.x和4版本有很大差异，语法不同**，可能和node版本不兼容，具体查看官方文档语法

4. 在项目根目录下新建一个 gulpfile.js  的文件，用来写gulp配置代码

## gulp相关api

1. gulp用**task**来创建任务`gulp.task(任务名，[先执行的任务],执行任务的函数)`

2. gulp默认会执行名为`default`的任务

   ```js
   gulp.task('default',['html','css','js']，function(){
       console.log('default默认执行')
   })
   在执行default之前，他会先去执行数组中的任务
   即先执行html任务，css任务，js任务，在执行default任务
   ```

3. gulp用**src**来读取文件

   `gulp.src("src/css/*")`gulp会去读取src/css目录下的所有文件

4. gulp用**dest**来写入文件

   `gulp.dest('dist/css/')`gulp会将读取的文件写入到dist/css文件下，写入的文件名跟读取时的文件名一致

5. gulp用**pipe**来当做管道，所谓管道即从水管的一头可以流向另一头

   ```js
   gulp.src('src/css/*')
   	.pipe(gulp.dest('dist/css/'))
   // gulp.src读取文件,gulp.dest写入文件，
   // pipe则将读取的文件传送给gulp.dest写入
   ```

6. gulp 用 watch 来监听文件变化

   ```js
   gulp.watch('src/css/*',['css'])   
   // watch会去监听src/css目录下的所有文件，一旦文件发生更改，则取执行css任务
   ```

7. 演示代码

   注意： gulp4.0版本使用task时，回调函数使用匿名函数带来的问题，gulpgulp不再支持同步任务 写一个回调函数来执行函数完成  

   ```js
   // gulpfile.js
   var gulp = require("gulp");
   var folder = {
       src:'src/',
       dist:'dist/'
   }
   gulp.task('css',function(){
       gulp.src(folder.src+"css/*")
       	.pipe(gulp.dest(folder.dist+"css/"))
   })
   
   gulp.task('html',function(){
       gulp.src(folder.src+"html/*")
       	.pipe(gulp.dest(folder.dist+"html/"))
   })
   
   gulp.task('js',function(){
       gulp.src(folder.src+"js/*")
       	.pipe(gulp.dest(folder.dist+"js/"))
   })
   
   gulp.task('default',['html','css','js'])
   
   //当前目录下，命令行 gulp  启动gulp
   
   ```

## 相关插件

插件用来压缩你的src代码的，文件类型不同对应着不同的插件 

1. 压缩html--->`gulp-htmlclean`

```js
var htmlClean = require('gulp-htmlclean');
gulp.task('html',function(){
    gulp.src(folder.src+"html/*")
    	.pipe(htmlClean) //先压缩再写入
    	.pipe(gulp.dest(folder.dist+"html/"))
})
复制代码
```

2. 压缩图片--->`gulp-imagemin`

```js
var imageMin = require(gulp-imagemin);
gulp.task('img',function(){
    gulp.src(folder.src+"img/*")
    	.pipe(imageMin())
    	.pipe(gulp.dest(folder.dist+'img/'))
})
复制代码
```

3. 压缩js--->`gulp-uglify`

```js
var uglify = require('gulp-uglify');
gulp.task('js',function(){
    gulp.src(folder.src+'js/*')
    	.pipe(uglify())
    	.pipe(gulp.dest(folder.dist+'js/'))
})
复制代码
```

4. 给css加前缀 （为兼容各版本浏览器）

   gulp-postcss

   autoprefixer

```js
var postcss = require('gulp-postcss');
var autoprefixer = require('autoprefixer');
gulp.task('css',function(){
   gulp.src(folder.src+"css/*")
    	.pipe(postcss([autoprefixer()])) //重点，注意看怎么写
    	.pipe(gulp.dest(folder.dist+"css/"))
})
```

5. 别的常用的插件
   * gulp-strip-debug 去除js中的所有调试语句、debug语句
   * gulp-sass 将sass转化为css
   * gulp-clean-css 压缩css

6. 开启服务器  gulp-connect 

```js
var connect = require('gulp-connect');
gulp.task('server',function(){
    connect.server({
        port:8888.//端口号
        livereload:true,//开启实时刷新
    })
})
//
gulp.task('css',function(){
   gulp.src(folder.src+"css/*")
    	.pipe(connect.reload())//加了connect.reload()后才能在浏览器实时看到最新变化
    	.pipe(postcss([autoprefixer()])) //重点，注意看怎么写
    	.pipe(gulp.dest(folder.dist+"css/"))
})

```

## 优化

在生产环境下压缩代码，开发环境下不压缩

思路：在代码压缩前进行判断 process.env.NODE_ENV 为 生产环境。

```js
var devMod = process.env.NODE_ENV =='development'
gulp.task('css',function(){
    var page = gulp.src(folder.src+'css/*')
    if(！devMod){
        page.pipe(cleancss())
    }
    page.pipe((gulp.dest(folder.dist+'css/')))
})

```

## gulp4.0

### 组合任务

Gulp 提供了两个强大的组合方法： `series()` 和 `parallel()`，允许将多个独立的任务组合为一个更大的操作。这两个方法都可以接受任意数目的任务（task）函数或已经组合的操作。`series()` 和 `parallel()` 可以互相嵌套至任意深度。

如果需要让任务（task）按顺序执行，请使用 `series()` 方法。

对于希望以最大并发来运行的任务（tasks），可以使用 `parallel()` 方法将它们组合起来。

```
gulp.task('default', gulp.parallel('css', 'img', 'html'));
```

