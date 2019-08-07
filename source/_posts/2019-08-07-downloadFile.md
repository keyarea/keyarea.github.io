---
title: 浏览器直接下载文件而不是尝试打开文件(Nodejs示例)
date: 2019-08-07 15:40:42
tags: 
- JavaScript
- Node.js
- HTTP
category:
- HTTP
---

> 在web开发啊过程之中遇到了这么一个文件，想用浏览器下载一个mp4文件，当初思想简单，没想那么深，直接把MP4文件放在了静态资源文件夹中，直接启动服务器，将url指向了该文件，然后浏览器没能如我所愿，去下载这个文件，而是在浏览器中打开了该文件。我也是很无奈，看来事情没那么简单，多方打听，终于明白了这个原理。
>
> 原来服务端向客户端游览器发送文件时，如果是浏览器支持的文件类型，一般会默认使用浏览器打开，比如mp4、txt、jpg等，会直接在浏览器中显示，而不是下载该文件。

## 如何处理

> 如果想要下载文件而不是打开文件，则需要在服务器端的响应头加上相应的头部信息。

```js
// 首部字段ContentType说明了实体主体内对象的媒体类型，这是一个二进制文件类型
'Content-Type': 'application/octet-stream'

//  Content-Disposition 消息头指示回复的内容该以何种形式展示，是以内联的形式（即网页或者页面的一部分），还是以附件的形式下载并保存到本地。
'Content-Disposition': 'attachment; filename=1.mp4'

'Content-Length': 1024
```

### Content-Type

> Content-Type 实体头部用于指示资源的MIME类型 media type 。
>
>在响应中，Content-Type标头告诉客户端实际返回的内容的内容类型。

### Content-DisPosition

> 在HTTP场景中，第一个参数或者是inline（默认值，表示回复中的消息体会以页面的一部分或者整个页面的形式展示），或者是attachment（意味着消息体应该被下载到本地；大多数浏览器会呈现一个“保存为”的对话框，将filename的值预填为下载后的文件名，假如它存在的话）。

## nodejs express 实例

```js
var express = require('express');
var router = express.Router();
const fs = require("fs");
const path = require("path");
const PUBLIC_PATH = path.resolve(__dirname, '../public');

/* GET home page. */
router.get('/', function(req, res, next) {
  res.render('index', { title: 'Express' });
});

router.get('/download/:path', function(req, res, next) {
  // 获取参数
  let filePath = req.params.path;
  // 拼接文件的绝对路径
  let downloadPath = path.resolve(PUBLIC_PATH, filePath);

  // 得到文件名
  let {base: fileName} = path.parse(downloadPath);

  // 文件的信息
  let stat;
  try{
    // 通过fs模块获取文件的信息，报错就代表着不存在该文件
    stat = fs.statSync(downloadPath);
  }catch(error) {
    // 不存在该文件
    return res.status(404).end();
  }

  // 获取文件的大小
  let {size: total} = stat;
  // 设置响应头信息，指示内容应该被下载
  res.setHeader("Content-Disposition", "attachment; filename="+fileName);

  // 说明了实体主体内对象的媒体类型，这是一个二进制文件类型
  res.setHeader("Content-Type", "application/octet-stream");

  // 响应状态为200
  res.status(200);

  // 通过fs模块创建二进制流，并将其二进制流推送到响应主体中
  fs.createReadStream(downloadPath).pipe(res);
});

module.exports = router;

```

## 下载文件为中文文件名时

> 探究 Content-Disposition：解决下载中文文件名乱码
>
> 直接在Content-Disposition中设置中文会导致乱码。
>
> 首先要注意的是disposition-type。由上面给出的是Content-Disposition header 字段我们可以知道，Content-Disposition有两种type，即inline和attachment。根据文档中的介绍可知，inline类型会自动显示附件内容，比如显示一个图片；而attachment不会自动显示，在邮件中可能会显示为一个带图标的附件，在浏览器中可能会提示下载。
>
>其次就是disposition-parm。主要作用就是提供一个建议的文件名（filename-parm），客户端（浏览器、邮件系统）在尽可能的情况下会以该文件名去保存文件。尽可能的意思是存在不一样的情况，比如文件名非法、存在同名文件，这些情况下客户端会采取一些措施，比如修改文件名。
>
>最后看看filename-parm的value。这个value就是文件名（本文的目的是给value设置一个中文，主要的坑就是这里）。
>
> 通过上面的介绍，要给前端发送一个文件，并且定义文件的名字为中文，可以在发送文件之前返回这样一个HTTP Response Header ：

```js
Content-Disposition: attachment; filename=文件.txt
```

但是，如果你这样做了，服务器就会报错。

> 原因：需要注意的是，RFC 822（ Standard for ARPA Internet Text Messages）规定了文本消息只能为ASCII，因此这个Content-Disposition是非法的。
>
> 在nodejs中，需要将中文文件名给处理一下，如下代码

```js
// 设置响应头信息，指示内容应该被下载
  res.setHeader("Content-Disposition", "attachment; filename="+ encodeURIComponent(fileName));
```

完整代码：

```js
var express = require('express');
var router = express.Router();
const fs = require("fs");
const path = require("path");
const PUBLIC_PATH = path.resolve(__dirname, '../public');

/* GET home page. */
router.get('/', function(req, res, next) {
  res.render('index', { title: 'Express' });
});

router.get('/download/:path', function(req, res, next) {
  // 获取参数
  let filePath = req.params.path;
  // 拼接文件的绝对路径
  let downloadPath = path.resolve(PUBLIC_PATH, filePath);

  // 得到文件名
  let {base: fileName} = path.parse(downloadPath);

  // 文件的信息
  let stat;
  try{
    // 通过fs模块获取文件的信息，报错就代表着不存在该文件
    stat = fs.statSync(downloadPath);
  }catch(error) {
    // 不存在该文件
    return res.status(404).end();
  }

  // 获取文件的大小
  let {size: total} = stat;
  // 设置响应头信息，指示内容应该被下载
  res.setHeader("Content-Disposition", "attachment; filename="+ encodeURIComponent(fileName));

  // 说明了实体主体内对象的媒体类型，这是一个二进制文件类型
  res.setHeader("Content-Type", "application/octet-stream");

  // 响应状态为200
  res.status(200);

  // 通过fs模块创建二进制流，并将其二进制流推送到响应主体中
  fs.createReadStream(downloadPath).pipe(res);
});

module.exports = router;
```


