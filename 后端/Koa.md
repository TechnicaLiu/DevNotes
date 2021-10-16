# KOA

## koa项目 

post  传送模块   bodyparse 

<img src="E:/Typora/img/image-20211014092015250.png" alt="image-20211014092015250"  />

ejs 模板引擎  安装 koa-views 和  ejs  

![image-20211014092100837](E:\Typora\img\image-20211014092100837.png)

传输日志 logger  

![image-20211014092241463](E:\Typora\img\image-20211014092241463.png)

前后端分离  必须是json接口 

get  ? 普通传值   用ctx.query对象来接收

![image-20211014093411419](E:\Typora\img\image-20211014093411419.png?lastModify=1634278073)

get  : 动态传值    用 ctx.params 来接收    前后端分离resetful 常用动态路由 ![image-20211014093354213](https://gitee.com/youngstory/images/raw/master/img/image-20211014093354213.png)

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





 