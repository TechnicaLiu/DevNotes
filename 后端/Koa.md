## Koa基本运行原理

> koa采用的是洋葱圈模型，先执行完next()前面的语句，匹配完路由后，再返回执行next后面的语句

### 特点

- 避免异步嵌套 

- **Koa 异步处理Async...Await 和Promise使用**

- 洋葱模型  中间件  合理拆分业务 

  ![image-20220120214707352](https://gitee.com/youngstory/images/raw/master/img/202201202147449.png)

- 通过app.use  添加一个中间件 ， 一个中间件就是一个函数 

- 内层中间件能否执行取决于外层中间件的 next 函数 是否调用 

- **调用next  函数 返回的是 promise 对象**

### 处理http请求

- ctx 即 request 和 response 的集合 ， 用来处理接收和返回数据  

- ctx.query  获取get 请求得参数集合   gei静态传值 

- ctx.request.path可以获取用户请求的路径 

- ctx.request.body 获取post 请求的参数集合 

- ctx.response.body  设置响应数据 

### 代码示例

```js
var Koa = require('koa');
var router = require('koa-router')();
var app = new Koa();

// 中间件 

app.use(async(ctx,next)=>{
  console.log('1.这是第一个中间件');
  await next();
  console.log('5.匹配路由完成以后又会返回来执行中间件');
})

app.use(async(ctx,next)=>{
  console.log('2. 这是第二个中间件 ');
  await next();
  console.log('4. 匹配路由完成以后又会返回来执行中间件');
})
router.get('/',async(ctx)=>{
  ctx.body='首页'
})

router.get('/new', async(ctx,next)=>{
  console.log('3. 匹配到了news这个路由');
  ctx.body='这是一个新闻'
})
app.use(router.routes()) // 启动路由
app.use(router.allowedMethods());
app.listen(3002)


```

## **jwt的token验证**

> **JWT就是一个加密的字符串，作为验证信息在计算机之间传递，只有可以访问持有对应正确的加密密钥的计算机才能对其进行解密，从而验证携带这个令牌（`Token`）的请求是否合法。**

基于Token的身份验证方法：

```
1.客户端使用用户名跟密码请求登录
2.服务端收到请求，去验证用户名与密码
3.验证成功后，服务端会签发一个 Token，再把这个 Token 发送给客户端
4.客户端收到 Token 以后可以把它存储起来，比如放在 Cookie 里或者 Local Storage 里
5.客户端每次向服务端请求资源的时候需要带着服务端签发的 Token
6.服务端收到请求，然后去验证客户端请求里面带着的 Token，如果验证成功，就向客户端返回请求的数据
```

jwt 由 header，payload（数据）,signature（签名） 组成

### payload（数据）

Payload 里面是 Token 的具体内容，这些内容里面有一些是标准字段

- iss：Issuer，发行者
- sub：Subject，主题
- aud：Audience，观众
- exp：Expiration time，过期时间
- nbf：Not before
- iat：Issued at，发行时间
- jti：JWT ID

### signature

JWT 的最后一部分是 Signature ，这部分内容有三个部分——
  第一部分 ：Base64 编码的header。
  第二部分：Base64 编码的payload。
  第三部分：再用加密算法加密一下，加密的时候要放进去一个 `Secret` ，这个相当于是一个密码，这个密码秘密地存储在服务端，不得泄露。![image-20211025141715996](https://gitee.com/youngstory/images/raw/master/img/202110251417258.png)

## 配置项

**post  传送模块**  

 bodyparse  可支持的文件类型解析  

<img src="https://gitee.com/youngstory/images/raw/master/img/202110231653916.png" alt="image-20211014092015250"  />

**ejs 模板引擎**  

安装 koa-views 和  ejs  

![image-20211014092100837](https://gitee.com/youngstory/images/raw/master/img/202110231653575.png)

**传输日志 logger**  

![image-20211014092241463](https://gitee.com/youngstory/images/raw/master/img/202110231653010.png)

前后端分离  必须是json接口 

get  ? **普通传值   用ctx.query**对象来接收

![image-20211023165431501](https://gitee.com/youngstory/images/raw/master/img/202110231654587.png)

get  : **动态传值    用 ctx.params** 来接收    前后端分离resetful 常用动态路由 ![image-20211014093354213](https://gitee.com/youngstory/images/raw/master/img/202110231654915.png)

接收post传值  ctx.request.body

## 关联表

hasOne  一对一

hasMany 一对多

```js
category.hasMany(article,{foreignKey:{name:'catId',allowNull:false}})   
```

belongsTo  一对多

```js
article.belongsTo(category,{foreignKey:{}}) // 默认 article表中会添加  categoryId字段 
```

BelongsToMany 多对多 

```
收藏表：会员，商品
```

## 使用koa-multer 上传文件 

前端部分  使用 FormData 将数据编译成键值对，以便用`XMLHttpRequest`来发送数据

```html
<body>
    <input type="file" id="file" accept="image/*"/>
    <button id="submit">提交</button>
</body>
<script src="https://cdn.bootcss.com/jquery/3.4.1/jquery.min.js"></script>
<script>
    $(function(){
        $('#submit').click(()=>{
            let file = $('#file')[0].files[0]
            let formData = new FormData()
            formData.set('file',file)
            formData.set('name',file.name)
            formData.set('timestamp',Date.now())
            $.ajax({
                url:'http://localhost:3000/user/file',
                type:'post',
                data: formData,
                cache: false,
                contentType: false,
                processData:false,
                success(res){
                    console.log(res)
                }
            })
        })
    })
</script>
```

node代码

```javascript
const Koa = require('koa')
const Router = require('koa-router')
const route = new Router()
const multer = require('@koa/multer')
const path = require('path')

//上传文件存放路径、及文件命名
const storage = multer.diskStorage({
    destination: 'public/upload/'
    filename: function (req, file, cb) {
        let type = file.originalname.split('.')[1]
        cb(null, `${file.fieldname}-${Date.now().toString(16)}.${type}`) //为了命名不重复，我使用时间戳转为16进制作为文件命名
    }
})
//文件上传限制
const limits = {
    fields: 10,//非文件字段的数量
    fileSize: 500 * 1024,//文件大小 单位 b
    files: 1//文件数量
}
const upload = multer({storage,limits})

route.post('/user/file', upload.single('file'), async (ctx,next)=>{  // 上传多个文件用 fields 
    ctx.body = {
        code: 1,
        data: ctx.file  //在路由中，可通过 ctx.file 获取上传完毕的文件信息，多文件上传可通过 ctx.files 获取
    }
})

app.use(router.routes()).use(router.allowedMethods())

app.listen(3000)
```





 