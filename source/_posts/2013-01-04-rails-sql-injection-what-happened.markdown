---
layout: post
title: "Rails 重大 SQL injection 安全漏洞其實沒這麼可怕"
date: 2013-01-04 09:15
comments: true
categories: Programing
---
![xkcd](http://lh3.googleusercontent.com/-Ptn8b9s_OAM/UOYvBgxC29I/AAAAAAAAFj8/qaLFQuKm_L0/s666/exploits_of_a_mom.png)

2013.01.10 更新：前兩天 [Rails 又爆出新的漏洞][rubyonrails-security]（有沒有這麼歡樂啊？XD），比本篇文章所講的嚴重一百萬倍，據說這漏洞是非常容易被搞，所以「一定」要更新到最新版本（目前的版本是 3.2.11、3.1.10、3.0.19 和 2.3.15 ）！不過目前我沒辦法重現這個 bug，也不清楚這次到底是不是「真有這麼可怕？」。:p

[rubyonrails-security]: https://groups.google.com/forum/?fromgroups=#!topic/rubyonrails-security/61bkgvnSGTQ

[xkcd][exploits-of-a-mom] 這張圖幽默地說明 SQL injection 的可怕，一位媽媽用 SQL 語法替自己小孩取名字，結果校方電腦沒有消毒資料就寫道資料庫去，結果整個資料庫就爆掉了，原來這小孩的名字其實是刪掉整個表格的語法。XD

[exploits-of-a-mom]: http://xkcd.com/327/

Rails 前兩天的[重大安全更新][rails-updated]，所有 3.x 的版本都有問題，引起鄉民激動的討論，我看了一下[大家討論的內容][hacker-news-rails-bug]看不出什麼所以然來，也不曉得這次的 SQL 注射是怎麼辦到的，可能很多人跟我一樣都搞不清楚狀況，所以 [Phusion 部落格][rails-sql-injection-vulnerability-hold-your-horses-here-are-the-facts]發表了一篇文章來說明整件事情的原委，寫得相當清楚。

[rails-updated]: http://weblog.rubyonrails.org/2013/1/2/Rails-3-2-10--3-1-9--and-3-0-18-have-been-released/
[hacker-news-rails-bug]: http://news.ycombinator.com/item?id=4999406
[rails-sql-injection-vulnerability-hold-your-horses-here-are-the-facts]: http://blog.phusion.nl/2013/01/03/rails-sql-injection-vulnerability-hold-your-horses-here-are-the-facts/

現在我終於搞清楚這個 bug 是這麼回事……

## 動態 finder

Rails 透過 ActiveRecord 查詢資料庫，通常會用到像是 `User.find(params[:id])` 這樣的方法，依照參數給的值查詢 model 的主鍵（primary key），非常方便。此外，Rails 還有非常方便的動態 finder，可以依照資料庫欄位來尋找給予的參數，例如：

``` ruby
find_by_foo(params[:foo])
```

而這次 Rails 3.2.10（包括 3.1.9 和 3.0.18）所修補的這個 bug 就是發生在這個動態 finder。`find_by_*` 用來查詢指定的任何欄位，例如 User model 中有 name、email 這兩個欄位，那就可以這樣寫：

``` ruby
User.find_by_login(params[:login])
User.find_by_email(params[:email])
```

`params[:name]` 和 `params[:email]` 是使用者 input 的參數，因此 cracker 就可能會用 [SQL 填字遊戲][sql_injection]注射髒東西進來。這麼簡單的原理 Rails 當然有保護措施，他們用[單引號][ruby-single-quotes-vs-double-quotes-comparision-and-performance] `' '` 不轉義字元，例如以下這例子：

[sql_injection]: http://en.wikipedia.org/wiki/SQL_injection
[ruby-single-quotes-vs-double-quotes-comparision-and-performance]: /posts/ruby-single-quotes-vs-double-quotes-comparision-and-performance/

``` ruby
User.find_by_login("chh")
# => SELECT "users".* FROM "users" WHERE "users"."login" = 'chh' LIMIT 1

User.find_by_login("chh'; DROP TABLE USERS; --")
# => SELECT "users".* FROM "users" WHERE "users"."name" = 'chh''; DROP TABLE USERS; --' LIMIT 1
```

cracker 想要把整個 User 表格都摘掉，不轉義的結果最後只會找不到東西，返回 nil，因為我們 login 欄位裡並沒有一個叫做「chh'; DROP TABLE USERS; --」的使用者。看起來好像沒事了，可是 ActiveRecord 的動態 finder 不只可以接受字串，如果有需要的話還可以自己定義 SQL 查詢條件，例如：

``` ruby
User.find_by_login("chh", :select => "id, login")
# => User Load (0.3ms)  SELECT id, login FROM "users" WHERE "users"."login" = 'chh' LIMIT 1
# => #<User id: 1, login: "chh">
```

只調出 id 跟 login 兩個欄位，也可以直接用 `:select`，儘管傳回來的會是 nil：

``` ruby
User.find_by_login(:select => "1; DROP TABLE users; --")
# => SELECT 1; DROP TABLE users; -- FROM "users" WHERE "users"."name" IS NULL LIMIT 1
```

可是這就有點問題了，由於 `User.find_by_login(params[:login])` 的 `params[:login]` 參數請求是會被 Rails 轉換成 hash 的，也就是說如果 cracker 用傳來的不是 string 而是有髒東西的 hash，那就會變成資安漏洞！例如以下的 URL：

    /example-url?login[select]=whatever&login[limit]=23

這個時候的 `params[:login]` 得到的就是一個 hash：`{ "select" => "whatever", "limit" => 23 }`。BUT！在 Ruby 當中 string 和 hash 是兩種不同的資料類型，雖然 Rails 中常常某些情況這兩種會混用，因為這是 ActiveSupport::HashWithIndifferentAccess 會自動轉換 hash 為 string ，以避免不必要的臭蟲滋生；`params[:login]` 也是如此。

而如果 cracker 想要用這個方法搞我們的 Rails app，他就必須要讓 string `"select"` 變成 hash `:select`，這樣才有辦法注射，但這是不可能的，Rails 會把它轉成 string。

## 問題點

可是問題就發生在 [Authlogic][authlogic] 這個 gem，他們透過很粗糙的方式向資料庫查詢 token：

[authlogic]: https://github.com/binarylogic/authlogic

``` ruby
User.find_by_persistence_token(the_token)
```

這個 `the_token` 並沒有將 hash 轉為 string，因此暴露了漏洞讓 cracker 有機可趁，可是一定要透過 session data 這種認證方式，否則其他方法 `the_token` 都會變成 string。

要破解這招要先準備這個 Rails 網站的 secret_token，該怎麼拿到呢？可以 [google][secret_token-on-github] 一下找到一堆 open source 專案的 secret_token，很多人佈署網站的時候都沒有重新 `rake secret`，而且就直接暴露到 Github 上去了。XD

[secret_token-on-github]: https://www.google.com/search?q=inurl:secret_token+filetype%3Arb+site%3Agithub.com

所以說這次的安全漏洞其實不會「瞬間」讓所有的 Rails app 都陷入危險狀態，甚至可以說，就算不更新好像也不會怎樣（？），除非網站用的是 open source ，也用到像 Authlogic 這種 gem，而且剛好又沒有重新產生新的 secret_token，才會有立即的危險。話說回來，這也不像是 Rails 本身的 bug，只是開發者會很容易不小心疏忽這個部份，所以 Rails 為了預防這種情況發生，才會補上這次的安全更新。

## 防禦方法

目前要補上這個漏洞有三種方法：

* 更新 Rails 新版本，目前官方已經發出 patch，更新完以後什麼事情都不用做；:)
* 在所有的 `find_by_*` 參數都加上過濾機制，例如 `find_by_login(params[:login].to_s)`，強制轉換成字串；
* 如果使用 open source 的網站，記得執行 `rake secret` 重新產生 token；

secret_token.rb 檔案的處理可以像 [database.yml][rails-database-yml-management] 一樣，製作成一個樣板，命名成 secret_token.rb.example，然後原本的檔案不列入版本控制，最後佈署時寫個腳本用 symlink 覆蓋過去；也可以參考[這個作法][snpr-secret_token]，在 Rails app 根目錄底下新建 secret_token 檔案專門拿來放 token，再引入到 secret_token.rb 去；或者也可以設在環境變數 `ENV` 裡。

[rails-database-yml-management]: /posts/rails-database-yml-management/
[snpr-secret_token]: https://github.com/gedankenstuecke/snpr/blob/master/config/initializers/secret_token.rb

## One More Thing... About Rails 4 Dynamic Methods...

Rails 4 的動態 finder [改變了一些寫法][active-record-deprecations]，除了 `find_by_*` 和 `find_by_*!` 這兩個之外，其他的全部都不能再用了：

[active-record-deprecations]: http://edgeguides.rubyonrails.org/4_0_release_notes.html#active-record-deprecations

* `User.find_all_by_*` 要改寫成 `User.where(*)`；
* `User.find_last_by_*` 改寫成 `User.where(*).last`；
* `User.scoped_by_*` 改寫成 `User.where(*)`；
* `User.find_or_initialize_by_*` 改寫為 `User.where(*).first_or_initialize`；
* `User.find_or_create_by_*` 改寫成 `User.find_or_create_by(*)` 或者 `User.where(*).first_or_create`；
* `User.find_or_create_by_*!` 則要改寫成 `User.find_or_create_by!(*)` 或是 `User.where(*).first_or_create!`；

看起來有點不太習慣，感覺沒有原本的直覺，為什麼要改這樣我也不曉得。