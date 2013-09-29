---
layout: post
title: "MongoDB 的 ODM：mongoose 簡單介紹"
date: 2013-09-06 11:11
comments: true
categories: Programing
---
![mongoose](https://lh3.googleusercontent.com/-Tdo_5cAPDnk/UilIbkRmleI/AAAAAAAAGqQ/vh6dM2gZvV4/w690-h450-no/%25E8%259E%25A2%25E5%25B9%2595%25E5%25BF%25AB%25E7%2585%25A7+2013-09-06+%25E4%25B8%258A%25E5%258D%258811.12.48.png)

[mongoose](http://mongoosejs.com/) 是一套給 Node.js 用的 MongoDB ODM，跟常聽到的 ORM 不同的地方只是一些[技術名詞定義上的把戲](http://stackoverflow.com/questions/12261866/what-is-the-difference-between-an-orm-and-an-odm)，其實是差不多的意思。

透過 mongoose 可以用包裝過的、更高階的、更直覺的 API 語法，以及模擬 SQL 資料庫 schema-based 的方式，來操作 MongoDB 資料庫。以下是個官方文件給的簡單範例，先建立了一個叫做 Cat 的 Model，`model` 的第二個參數就是建立 schema 的所在：

``` js
var Cat = mongoose.model('Cat',
  { name: String }
);

var kitty = new Cat({ name: 'Zildjian' });
kitty.save(function (err) {
  if (err) // ...
  console.log('meow');
});
```

<!-- more -->

## 與 MongoDB 建立連線

這裡繼續沿用上一篇文章「[Node.js 的 Http vs Connect vs Express](/posts/node-http-vs-connect-vs-express/)」的例子，先把上一篇文章的範例程式碼複製過來，檔案名稱命名為 _server.js_，下指令執行 `node server.js` 這樣可以得到一台簡單的伺服器：

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

打開瀏覽器訪問 [http://localhost:4000](http://localhost:4000) 應該可以看到「hello」的文字，從 console 應該也能看到有關 cookies 的資訊（有可能只是一個空的物件）。

再來就是把 mongoose 給 requrie 進來，然後讓它跟 MongoDB 嘗試建立連線，連線的 URL 協議一定要用 `mongodb://` 這個 prefix：

``` js
var mongoose = require('mongoose')
mongoose.connect('mongodb://localhost/test')
```

## mongoose 的兩個概念：Schema 與 Model

MongoDB 是以 documents 為基礎，在 SQL 資料庫稱為 table 的東西，在 NoSQL 裡稱為 collection。當然，這又是一種名詞定義上的把戲，實質上大同小異。

### Schema

mongoose 的 Schema 概念就是用 schema-based 的方式，定義一個 collection 的組成結構，用程式碼描述會這樣子寫：

``` js
var Schema = mongoose.Schema
var UserSchema = new Schema(
  {
    name:      { type: String },
    login:     { type: String, unique: true },
    email:     { type: String, unique: true },
    create_at: { type: Date, default: Date.now },
    update_at: { type: Date, default: Date.now }
  }
)
```

因為 MongoDB 是 schema-less 相當有彈性，所以如果上面這個 schema 某些「欄位」沒有賦值，那麼在 MongoDB 裡就不會有那個「欄位」。說「欄位」是 SQL 的思維，可是我覺得這樣講會比較好理解。

### Model

而 mongoose 的 Model 概念，則是對一個 collection 結構定義與操作方法的集合，也就是用 Schema 定義了一個 collection 的結構，加上其他對這個 collection 的驗證設定、操作方法等等，便構成了一個 Model。

結合剛剛的 schema 範例，可以再加上一些驗證跟操作的方法：

``` js
UserSchema.pre('save', function(next) {
  // do something...
})

UserSchema.statics = {

  getUserByLogin: function(login, callback) {
    this.findOne({ login: login })
      .exec(callback)
  }

}
```

最後將這個 Schema 定義到一個叫做 User 的 model：

    mongoose.model('User', UserSchema)

當要使用這個 model 只要用 `mongoose.model()` 將 model 讀出來，便可以對他進行操作了：

``` js
var User = mongoose.model('User')

User.getUserByLogin(login, function(err, user) {
  // here we have a user...
})
```

## 為什麼 Schema-less 的資料庫需要 Schema-based？

不過這就很奇怪了，NoSQL 的 MongoDB 本身就是 schema-less 的資料庫，結果用 mongoose 還要去刻意模擬成 schema-based，這樣的思路是什麼？

其實這是對 NoSQL 的 schema-less 的誤解，schema-less 並不代表 no-schema。在應用當中還是需要一個 schema 來代表 model，而 schema-less 只是代表一種彈性的模式。

否則的話，會需要在很多地方寫髒亂的判斷條件，像是這樣的東西：

``` js
if (cat.name) {
  console.log(cat.name)  // I got a name
}
```
