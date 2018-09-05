# hello-express

Node.js + Express + MongoDB 实战 TodoList 基础入门

视频地址

- [https://www.rails365.net](https://www.rails365.net/movies/nodejs-express-mongodb-ji-chu-pian-1-jie-shao)
- [b站](https://www.bilibili.com/video/av20196752?t=62)

常用链接

- [express 官网](http://expressjs.com/)
- [express 官网中文](http://expressjs.com/zh-cn/)
- [express github](https://github.com/expressjs/express)
- [Nodejs学习笔记以及经验总结](https://github.com/chyingp/nodejs-learning-guide)

看视频整理要点笔记:

----

- [hello-express](#hello-express)
    - [1.介绍](#1%E4%BB%8B%E7%BB%8D)
    - [2.请求与响应](#2%E8%AF%B7%E6%B1%82%E4%B8%8E%E5%93%8D%E5%BA%94)
    - [3.路由参数](#3%E8%B7%AF%E7%94%B1%E5%8F%82%E6%95%B0)
    - [4.查询字符串](#4%E6%9F%A5%E8%AF%A2%E5%AD%97%E7%AC%A6%E4%B8%B2)
    - [5.POST请求和postman工具](#5post%E8%AF%B7%E6%B1%82%E5%92%8Cpostman%E5%B7%A5%E5%85%B7)
    - [6.上传文件](#6%E4%B8%8A%E4%BC%A0%E6%96%87%E4%BB%B6)
    - [7.模版引擎介绍](#7%E6%A8%A1%E7%89%88%E5%BC%95%E6%93%8E%E4%BB%8B%E7%BB%8D)
    - [8.使用模版引擎](#8%E4%BD%BF%E7%94%A8%E6%A8%A1%E7%89%88%E5%BC%95%E6%93%8E)

----

## 1.介绍

- express
    - 基于 Node.js 的 web 框架
    - 用于快速搭建网站和应用，如博客、商场、聊天室、为前端提供 API
    - 热门、健全、简单、少走弯路
    - 简单路由系统
    - 集成模版引擎
    - 中间件系统

- 快速开始
    - `npm init -y` 默认模式生成 `package.json`
    - `npm install --save express` 安装框架
    - `npm install -g nodemon` 方便调试，`nodemon xxx` 启动应用

```js
var express = require('express')

var app = express()

app.get('/', function (req, res) {
    res.send('this is homepage')
})

app.listen(3000)
```

## 2.请求与响应

- 学会查看 [官网 API 文档](http://expressjs.com/en/4x/api.html)，最快最全，这个文档太清晰易懂了
- [res.send([body])](http://expressjs.com/en/4x/api.html#res.send)
- [req.ip](http://expressjs.com/en/4x/api.html#req.ip)

```js
res.send(new Buffer('whoop'));
res.send({ some: 'json' });
res.send('<p>some html</p>');
res.status(404).send('Sorry, we cannot find that!');
res.status(500).send({ error: 'something blew up' });

res.json({ user: 'tobi' });
res.status(500).json({ error: 'message' });

req.ip
// => "127.0.0.1"

// GET /search?q=tobi+ferret
req.query.q
// => "tobi ferret"

// example.com/users?sort=desc
req.path
// => "/users"
```

## 3.路由参数

- 路由参数是动态的

```js
// http://127.0.0.1:3000/profile/1/user/able
app.get('/profile/:id/user/:name', function (req, res) {
    console.dir(req.params) // 显示属性  { id: '1', name: 'able' }
    res.send("You requested " + req.params.id + req.params.name)
})
```

- 路由参数支持正则表达式

```js
app.get('/ab?cd', function (req, res) {
    res.send('ab?cd')
})
```

## 4.查询字符串

- 文档 [req.query](http://expressjs.com/en/4x/api.html#app.use)

```js
// GET /shoes?order=desc&shoe[color]=blue&shoe[type]=converse
req.query.order
// => "desc"

req.query.shoe.color
// => "blue"

req.query.shoe.type
// => "converse"
```

```js
// http://127.0.0.1:3000/?find=hot
app.get('/', function (req, res) {
    console.dir(req.query) // => { find: 'hot' }
    res.send('home page: ' + req.query.find)
})
```

## 5.POST请求和postman工具

- 使用 body-parser 包，处理 post 请求
    - [body-parser 文档](https://www.npmjs.com/package/body-parser)
    - `npm install body-parser --save` 安装
    - 查看文档，使用例子

- postman 工具，用来图形化模拟浏览器发送各种请求
- [POST 提交数据方式](https://imququ.com/post/four-ways-to-post-data-in-http.html)
    - HTTP/1.1 协议规定的 HTTP 请求方法有 OPTIONS、GET、HEAD、POST、PUT、DELETE、TRACE、CONNECT 这几种
    - POST 一般用来向服务端提交数据
    - application/x-www-form-urlencoded 普通表单提交
    - multipart/form-data 可以上传文件的表单

```js
var bodyParser = require('body-parser')
// create application/json parser
var jsonParser = bodyParser.json()
// 使用中间件，在请求和响应中间处理 create application/x-www-form-urlencoded parser
var urlencodedParser = bodyParser.urlencoded({ extended: false })

app.post('/', urlencodedParser, function (req, res) {
    console.dir(req.body)
    res.send('ok')
})

app.post('/upload', jsonParser, function (req, res) {
    console.dir(req.body)
    res.send('ok')
})
```

## 6.上传文件

- [Multer 包](https://www.npmjs.com/package/multer) 处理上传文件
> Multer is a node.js middleware for handling multipart/form-data, which is primarily used for uploading files.

- 安装 `npm install --save multer`

- [基于express+multer的文件上传](https://www.cnblogs.com/chyingp/p/express-multer-file-upload.html)
- 上传文件的表单需要指定 `enctype="multipart/form-data"`
- postman 上传文件，post body form-data

```js
// form.html
<form action="/upload" method="post" enctype="multipart/form-data">
    <h2>上传logo图片</h2>
    <input type="file" name="logo">
    <input type="submit" value="提交">
</form>
```

```js
// 创建目录，上传文件
var createFolder = function (folder) {
    try {
        fs.accessSync(folder);
    } catch (e) {
        fs.mkdirSync(folder);
    }
};

var uploadFolder = './upload/';

createFolder(uploadFolder);

var storage = multer.diskStorage({
    destination: function (req, file, cb) {
        cb(null, uploadFolder);
    },
    filename: function (req, file, cb) {
        cb(null, file.originalname);
    }
});
var upload = multer({ storage: storage });

app.get('/form', function (req, res) {
    var form = fs.readFileSync('./form.html', { encoding: "utf8" })
    res.send(form)
})

app.post('/upload', upload.single('logo'), function (req, res) {
    console.dir(req.file); // 列出文件的所有属性
    res.send({ 'ret_code': 0 })
})
```

## 7.模版引擎介绍

- 直接使用`res.sendFile(__dirname + '/form.html')`响应网页

```js
app.get('/form', function (req, res) {
    // var form = fs.readFileSync('./form.html', { encoding: "utf8" })
    // res.send(form)
    res.sendFile(__dirname + '/form.html')
})
```

- [模版引擎 EJS](http://ejs.co)
    - `npm install ejs --save`
    - 模版文件扩展名 `.ejs`
    - ejs 模版的Tags 特殊，非对称的，有前面和后面的，如 `%> Plain ending tag`


```js
app.get('/form/:name', function (req, res) {
    var person = req.params.name
    res.render('form', { person: person })
})

// views/form.ejs
<h1><%= person %></h1>
// http://127.0.0.1:3000/form/able
// 输出 able
```

- [将模板引擎用于 Express](http://expressjs.com/zh-cn/guide/using-template-engines.html)
    - 在 Express 可以呈现模板文件之前，必须设置以下应用程序设置
    - views：模板文件所在目录。例如：app.set('views', './views') 默认
    - view engine：要使用的模板引擎。例如：app.set('view engine', 'ejs')

## 8.使用模版引擎

```js
app.get('/form/:name', function (req, res) {
    // var person = req.params.name
    var person = { age: 29, job: 'CEO', hobbies: ['eating', 'coding', 'finshing']}
    res.render('form', { person: person })
})

app.get('/about', function (req, res) {
    // var person = req.params.name
    res.render('about')
})
```

```js
<%- include('particals/header.ejs') -%>  // 引用模版
    <h1><%= person %></h1>
    <h1><%= person.age %></h1>
    <h2>hobbies</h2>
    <ul>  //遍历数组
        <% person.hobbies.forEach(function(item){ %>
            <li>
                <%= item %>
            </li>
        <% }) %>
    </ul>
```