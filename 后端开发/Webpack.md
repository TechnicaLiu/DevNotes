# Webpack 基础

## 1.安装

npm init -y  //创建一个空项目
npm install --save-dev webpack
npm install --save-dev webpack-cli

## 2.目录结构

- 按照我们之前了解 vue 项目的创建目录

  ```js
  project - src - main.js - package.json;
  ```

- index.js 文件内容

  ```js
  const add = function () {
    return 1 + 2;
  };
  ```

````js
console.log(add())
```
````

- 在根目录创建 webpack.config.js 文件

  ```js
  project - src - package.json - webpack.config.js;
```

- webpack 的基础配置

  - 使用 commonjs 语法,将 webpack 配置文件内容暴露出去

    ```js
    module.exports = {};
    ```

  - entry

    js 的入口文件

    ```js
    module.exports = {
      entry: "./src/main.js",
    };
    ```

  - output

    js 打包之后的信息内容

    ```js
    module.exports = {
      output: {
        filename: "js/app.js",
        //resolve是nodejs标准库的path库的方法，用来处理路径的。__dirname指的是当前的路径，
        path: resolve(__dirname, "build"),
        //publicPath会自动在输出的静态资源的路径前加上我们写的值，
        //而加上publicPath:'./'的原因就是让我们html中img标签的路径正确，
        //因为输出的图片是在build文件夹中，但不会创建新的文件夹
        publicPath: "./",
      },
    };
    ```

  - 打包

    - 直接使用 webpack 命令

      ```js
      npx webpack
      ```

    - 通过修改 package.json 文件进行打包

      - 修改 package.json 文件

        ```js
         "scripts": {
            "build": "npx webpack",
        },
        ```

      - 打包

        ```js
        npm run build
        ```

  - 测试打包后的结果

    ```js
    node build/js/mod.js
    ```

## 3.把 html 文件打包

- 增加 index.html 文件

  ```js
  project - src - index.htmls - package.json - webpack.config.js;
  ```

- index.html 文件内容

  ```js
  <html>
      <head>
          <meta charset="utf-8">
          <mata name="viewport" content="width=device-width,initial-scale=1.0">
          <title>webpack-demo</title>
      </head>
      <body>
          <h1>webpack demo </h1>
      </body>
  </html>
  ```

- HtmlWebpackPlugin 插件

  - 安装该插件

    ```js
    npm i -D html-webpack-plugin
    ```

  - 修改 webpack 配置文件

    ```js
    const HtmlWebpackPlugin = require("html-webpack-plugin");        //把html文件打包
    module.exports = {
        entry:...,
        output:...,
        plugins:[
            new HtmlWebpackPlugin({
                template:"./index.html",
                minify:{
                    collapseWhitespace:true, //移出空格
                    removeComments:true, //移出注释
                }
            }),
        ],
    }

    ```

## 4.css 样式的 loader 配置

````js
-安装```
    //将css文件编程commonjs模块加载到js中，里面内容是样式字符串
    npm i -D css-loader
    //创建<style>标签，将js中样式插入进行，添加到head中
    npm i -D style-loader
    ``` -
  修改配置文件```
    //loader配置
    module:{
        rules:[
            {
                test:/\.css$/,
                //use数组中loader执行顺序，是从下到上依次执行
                use:[
                    "style-loader", //创建<style>标签，将js中样式插入进行，添加到head中
                    "css-loader", //将css文件编程commonjs模块加载到js中，里面内容是样式字符串
                ]
            },
        ]
    },
    ``` -
  创建css文件 -
  src / common / common.css -
  common.css内容```
        body {
            color:blue;
        }
        ``` -
  将common.css导入;

index.js```
    import "./assets/common.css" 
    ``` - 打包运行;
````

## 5.将 css 样式单独打包出独立文件

- 安装

  ```
  npm i -D mini-css-extract-plugin
  ```

- 修改 webpack 配置文件

  ```js
  module:{
          rules:[
              {
                  test:/\.css$/,
                  //use数组中loader执行顺序，是从下到上依次执行
                  use:[
                      {
                          loader:MiniCssExtractPlugin.loader, //取代style-loader，作用:提取js中的css成单独文件
                          options:{
                              publicPath:"../../",
                          }
                      },
                      "css-loader", //将css文件编程commonjs模块加载到js中，里面内容是样式字符串
                  ]
              },
          ]
  },
  plugins:[
      new HtmlWebpackPlugin({
          template:"./index.html",
          minify:{
              collapseWhitespace:true, //移出空格
              removeComments:true, //移出注释
          }
      }),
      new MiniCssExtractPlugin({
          filename:'assets/css/build.[contenthash:10].css',
      }),
  ],
  ```

- 打包运行

## 6.打包图片

- 在 assets 文件夹下创建 imgs 文件夹，并放入测试图片

  ```
  src
   - assets
      - imgs
          - 1.jpg
          - 2.png
          - 3.jpg
  ```

- 修改 css 文件

  ```html
  #img1 { width: 100px; height:100px; background-image: url('../imgs/1.jpg') ;
  background-repeat: no-repeat; background-size: cover; } #img2 { width: 100px;
  height:100px; background-image: url('../imgs/2.png'); background-repeat:
  no-repeat; background-size: cover; } #img3 { width: 100px; height:100px;
  background-image: url('../imgs/3.jpg'); background-repeat: no-repeat;
  background-size: cover; }
  ```

- 修改 index.html

  ```
  <div id="img1"></div>
  <div id="img2"></div>
  <div id="img3"></div>
  ```

- 下载 url-loader file-loader

  ```
  npm i -D url-loader file-loader
  ```

- 修改配置文件

  ```js
  module:{
      rules:[
          {
              test:/\.(jpg|png)$/, //处理图片资源
              //下载url-loader file-loader
              loader:'url-loader',
              //不配置options时 默认会把图片转为base64
              options:{
                  //图片大小小于8kb，就会被base64处理
                  //有点：减少请求数量（减轻服务器压力）
                  //缺点：图片体积会更大（文件请求速度变慢）
                  limit:8 * 1024,
                  //给图片重命名，【hash:10】取图片hash前10位，[ext]取文件原来扩展名
                  name:'[hash:10].[ext]',
                  outputPath:"assets/imgs",
                  // esModule:false,
              }
          },
      ]
  },
  ```

- html 中 img 标签打包图片

  - 修改 index.html

    ```
    <img style="width:100px;height:auto;" src="./src/assets/imgs/3.jpg" />
    ```

  - 安装 html-loader

    ```
    //处理html文件的img图片(负责引入img 从而被url-loader进行处理)
    npm i -D html-loader
    ```

  - 修改 webpack 配置文件

    ```js
    //loader配置
    module:{
        rules:[
            {
                test:/\.html$/,
                //处理html文件的img图片(负责引入img 从而被url-loader进行处理)
                loader:"html-loader"
            },
        ]
    },
    ```

## 7.开发服务器 devServer:用来自动化编译，打开浏览器，自动刷新浏览器

- 安装

  ```
  npm i -D webpack-dev-server
  ```

- 修改 webpack 配置

  ```js
  //开发服务器devServer:用来自动化编译，打开浏览器，自动刷新浏览器
  //特点:只会在内存中编译打包，不会有任何输出
  //注意!!!：如果你使用的webpack-cli的版本小于v4 启动devServer指令为:webpack-dev-server
  //注意!!!：如果你使用的webpack-cli的版本是v4 那么启动devServer的命令是：webpack serve
  //webpack-cli v4 comes with out of box support of @webpack-cli/serve which means you can use webpack serve to invoke the webpack-dev-server.
  devServer:{
      contentBase:resolve(__dirname,'build'),
      compress:true,  //启动gzip压缩
      port:3000,
      // open:true, //自动开发默认浏览器
      hot:true //开启hmr服务
  },
  ```

- 修改 package.json 中 脚本命令信息

  ```
  "scripts": {
      "start": "npx webpack serve",
      "build": "npx webpack"
  },
  ```

- 运行测试

  ```
  npm start
  ```

## 8.模式

- 开发环境

  ```
  mode:"development",
  ```

- 生产环境

  ```
  mode:"production",
  ```

## 9.clean-webpack-plugin

- 每次先清除上一次部署文件再编译

- 安装

  ```
  npm i -D clean-webpack-plugin
  ```

- 修改配置文件

  ```
  const { CleanWebpackPlugin } = require("clean-webpack-plugin");  //每次先清除上一次部署文件再编译
  plugins:[
      new CleanWebpackPlugin(),  //删除上一次build之后的文件
  ],
  ```

## 10.source-map

    ```
    devtool:'source-map'
    ```

## 11.alias 别名

    ```
    resolve:{
        alias:{
            "@":resolve(__dirname,"./src")
        }
    },
    ```

## 12.js 文件分割打包

- 修改配置文件

  ```
  optimization:{
      //split 打包
      splitChunks:{
          chunks:'all',
      }
  },
  ```

- 添加 test.js 文件

  ```
  export const fn = () => console.log(1000)
  ```

- 在 index.js 文件引用

  ```
  import(/* webpackChunkName:'P' */"./test")
      .then(({fn})=>fn())
      .catch(e=>console.log(e))
  ```

- 运行测试

# sourceMap

> 一个存储源代码与编译代码(通常经过压缩混淆和代码转换)对应位置映射的信息文件。它能够将经过压缩、混淆、合并的代码还原回未打包状态，帮助开发者在生产环境中精确定位问题发生的行列位置。

在前端的工作中主要是用来解决以下三个方面出现的 debug 问题：

 a. 代码压缩混淆后
​ b. 利用 sass 、typeScript 等其他语言编译成 css 或 JS 后
​ c. 利用 webpack 等打包工具进行多文件合并后

当我们进行调试代码的时候，经过压缩和处理的代码很难找到 bug 地方，所以我们需要借助 sourceMap 来提供给我们 出错的位置信息。

## 使用

### 环境设置

google 浏览器 打开 设置 - source 进行勾选

<img src="https://techliuimg.oss-cn-beijing.aliyuncs.com/img/202201061107899.png" style="zoom:67%;" />

### .map 文件生成

![image-20220106112539152](https://techliuimg.oss-cn-beijing.aliyuncs.com/img/202201061125242.png)



配置代码

```js
const path = require('path')

const name = process.env['VUE_APP_TITLE ']

// 配置路径  
function resolve(dir) {
  return path.join(__dirname, dir)
}

module.exports = {
  // 保证静态资源路径正确
  publicPath: '/',
  // 在开发模式下 启用 eslint验证
  lintOnSave: process.env.NODE_ENV === 'development',
  // 生成源代码和打包后的代码 对应映射位置的 信息文件
  productionSourceMap: false,
  // 自动化编译 
  devServer: {
    disableHostCheck: true,
    open: true,
    port: 8888,
    // 开发环境默认开启反向代理，如果不需要请自行注释
    proxy: {
      '/tduck-api': {
        target: 'http://192.162.130.172:8001/',
        // target: 'http://220.178.67.242:8082',
        // target: 'http://122.9.33.45:9101',
        // target: 'http://localhost:8899',
        changeOrigin: true
      }
    }
  },
  // 对 webpack做简单配置  
  configureWebpack: {
    // provide the app's title in webpack's name field, so that
    // it can be accessed in index.html to inject the correct title.
    name: name,
    resolve: {
      alias: {
        '@': resolve('src')
      }
    }
  },
  // 对 webpack 做高级配置 ，包括对 loader的添加修改 以及 插件的使用
  chainWebpack(config) {
    // it can improve the speed of the first screen, it is recommended to turn on preload
    // prefetch:用来告诉浏览器在页面加载完成后，利用空闲时间提前获取用户未来可能会访问的内容
    // preload: 用来指定页面加载后很快会被用到的资源
    config.plugin('preload').tap(() => [
      {
        rel: 'preload',
        // to ignore runtime.js
        // https://github.com/vuejs/vue-cli/blob/dev/packages/@vue/cli-service/lib/config/app.js#L171
        fileBlacklist: [/\.map$/, /hot-update\.js$/, /runtime\..*\.js$/],
        include: 'initial'
      }
    ])

    // when there are many pages, it will cause too many meaningless requests
    // prefetch 会加载未来资源 消耗带宽 所以要关掉
    config.plugins.delete('prefetch')

    // set svg-sprite-loader  目的是为了将svg图片转换为svg标签插入html 
    config.module
      .rule('svg')
      .exclude.add(resolve('src/assets/icons'))
      .end()
    
    config.module
      .rule('icons')
      .test(/\.svg$/)
      .include.add(resolve('src/assets/icons'))
      .end()
      .use('svg-sprite-loader')
      .loader('svg-sprite-loader')
      .options({
        symbolId: 'icon-[name]'
      })
      .end()
    config.plugin('html')
      .tap(args => {
        args[0].title = process.env.VUE_APP_TITLE
        args[0].debugTool = process.env.VUE_APP_DEBUG_TOOL
        return args
      })
      .end()

    config
      .when(process.env.NODE_ENV !== 'development',
        config => {
          config
            .plugin('ScriptExtHtmlWebpackPlugin')
            .after('html')
            .use('script-ext-html-webpack-plugin', [{
              // `runtime` must same as runtimeChunk name. default is `runtime`
              inline: /runtime\..*\.js$/
            }])
            .end()
      // splitChunks提取或者分离代码的插件，主要作用提取公共代码，防止代码被重复打包 ，拆分过大的js文件，合并零散的js文件 
          config
            .optimization.splitChunks({
            //chunks 决定要提取那些模块。
            // initital: 提取同步和异步加载模块，如果一个模块既同步也异步，则打包两次
            // async: 只提取异步加载的模块出来打包到一个文件中
              chunks: 'all',  // 不管异步加载还是同步加载的模块都提取出来，打包到一个文件中。
            
              // 配置提取模块的方案 。 里面每一项代表一个提取模块的方案
           	 cacheGroups: { 
                libs: {
                  // 打包生成js文件的名称 
                  name: 'chunk-libs',
                  // 用来匹配要提取的模块的资源路径或名称 
                  test: /[\\/]node_modules[\\/]/,
                  // 方案的优先级
                  priority: 10, 
                  chunks: 'initial' // only package third parties that are initially dependent
                },
                elementUI: {
                  name: 'chunk-elementUI', // split elementUI into a single package
                  priority: 20, // the weight needs to be larger than libs and app or it will be packaged into libs or app
                  test: /[\\/]node_modules[\\/]_?element-ui(.*)/ // in order to adapt to cnpm
                },
                commons: {
                  name: 'chunk-commons',
                  test: resolve('src/components'), // can customize your rules
                  minChunks: 3, //  minimum common number
                  priority: 5,
                  reuseExistingChunk: true
                }
              }
            })
          // https:// webpack.js.org/configuration/optimization/#optimizationruntimechunk
          config.optimization.runtimeChunk('single')
        }
      )
  }
}

```

