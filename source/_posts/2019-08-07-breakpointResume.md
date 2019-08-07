---
title: 断点续传（Nodejs示例）
date: 2019-08-07 15:42:05
tags:
- Node.js
- JavaScript
category:
- HTTP
---

> 什么是断点续传（这里说的是下载断点续传）？怎么实现的断点续传？
> 断点续传就是下载了一半断网或者暂停了，然后可以接着下载。不用从头开始下载。
>
> 当一个服务器支持Range时，客户端可以将需要发送的内容分成很多份发送给服务端，服务端可以每次接收部分内容。有了这样的能力，遇到断网的情况，我们可以在客户端记录下已经传送的文件范围，网络恢复后再将剩余部分发送给服务端，这样就实现了断点续传。
>
> 首先客户端向服务端发送一个请求（下载文件）。然后服务端响应请求，信息包含文件总大小、文件流开始和结束位置、内容大小等。那具体是怎么实现的呢？

HTTP/1.1有个头属性Range。比如你发送请求的时候带上Range:0-199，等于你是请求0到199之间的数据。然后服务器响应请求Content-Range: bytes 0-199/250 ，表示你获取了0到199之间的数据，总大小是250。（也就是告诉你还有数据没有下载完）。

是不是很简单？这么神奇的东西也就是个“约定”而已，也就是所谓的HTTP协议。


express 示例：

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


// 文件下载之断点续传
router.get("/transfer/:path", function(req, res, next) {
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

  // 获取请求头部信息
  let range = req.get("range");

  // 如果包含该头部
  if(range) {
    // 获取范围请求的开始和结束位置
    let [, start, end] = range.match(/(\d*)-(\d*)/);

    // 处理请求头中范围参数不传的问题
    start = start ? parseInt(start) : 0;
    end = end ? parseInt(end) : total - 1;

    res.status(206);

    res.setHeader("Accept-Ranges", "bytes");
    res.setHeader("Content-Range", `bytes ${start}-${end}/${total}`);
    res.setHeader("Content-Type", "application/octet-stream");
    res.setHeader("Content-Disposition", 'attachment; filename=' + encodeURIComponent(fileName));
    res.setHeader("Content-Length", total);

    fs.createReadStream(downloadPath, { start, end }).pipe(res);
  }else {
    res.status(200);

    res.setHeader("Content-Type", "application/octet-stream");
    res.setHeader("Content-Disposition", 'attachment; filename=' + encodeURIComponent(fileName));
    res.setHeader("Content-Length", total);

    fs.createReadStream(downloadPath).pipe(res);
  }


})


module.exports = router;
```