---
layout: post
title: "Node.js 的 http vs Connect vs Express"
date: 2013-09-05 02:17
comments: true
categories: Programing
---
![node-express](https://lh5.googleusercontent.com/-mk_3GdVqmSI/Uid55DfCYHI/AAAAAAAAGp4/3mW_vcrS63Q/w690-h450-no/%25E8%259E%25A2%25E5%25B9%2595%25E5%25BF%25AB%25E7%2585%25A7+2013-09-05+%25E4%25B8%258A%25E5%258D%25882.19.28.png)

這篇應該是我第一篇寫有關 [Node.js](http://nodejs.org/) 的文章，未來應該會開始多寫一些有關 Javascript 的東西。這次主題要來聊聊 Node.js 的 http 模組，跟知名的 Connect 和 Express 之間的關係，如果上網去問，有人會回說就像是 Ruby 的 [Rack](http://rack.github.io/) 一樣，但是具體到底是什麼東西呢？

<!-- more -->

## http 模組

Node.js 自身就帶有一個叫做 http 的 [module](http://nodejs.org/api/http.html)，可以呼叫 `http.createServe` 建立一個相當基本伺服器，具體用法會長得像是以下這個樣子，可以建立一份檔案命名為 _server.js_ 來測試一下：

``` js
var http = require('http')

http.createServer(function(req, res) {
  res.writeHead(200)
  res.end("hello world\n")
}).listen(8000)
```

然後在終端機執行 `node server.js` 指令來啟動伺服器，因為程式碼裡面指定了 8000 port，所以當在瀏覽器裡打開 [http://localhost:8000](http://localhost:8000) 看到「hello world」就代表成功了。

## Connect

然而我需要更多的細節，例如讀取送來的 request 中的 cookies，因此 [Connect](http://www.senchalabs.org/connect/) 這樣的 middleware 框架，便是用來輕鬆安插各種 middleware 來處理 request，透過 `connect.cookieParser()` 讓 `req` 物件多了一個 `cookies` 屬性：

``` js
var http    = require('http')
  , connect = require('connect')

var app = connect()
  .use(connect.cookieParser())
  .use(function(req, res) {
    console.log(req.cookies);
    res.end('hello');
  })

http.createServer(app).listen(8000)
```

但是 Connect 充其量只是提供了安插 middleware 的 API，其他像是 route、view rendering 等工作，就會需要靠 Express 這套框架了。

## Express

[Express](http://expressjs.com/) 其實就是延伸自 Connect 的加強版，所有 Connect 的 API 在 Express 裡都可以使用，並且提供了更多實用的 functions，一次解決所有問題。

將剛剛寫的程式碼修改成以下的範例，其中 `get` 這個 function 就是 HTTP 的動詞，有了這樣的 API 便能很輕鬆地策劃路由了：

``` js
var express = require('express')
  , app     = express()

app.use(express.cookieParser())

app.get('/', function(req, res) {
  console.log(req.cookies)
  res.send('hello')
})

app.listen('4000')
```

一言以蔽之，http 之於 Connect，就如 Connect 之於 Express。

### 參考資料

* [What is Node.js' Connect, Express and “middleware”?](http://stackoverflow.com/questions/5284340/what-is-node-js-connect-express-and-middleware)
