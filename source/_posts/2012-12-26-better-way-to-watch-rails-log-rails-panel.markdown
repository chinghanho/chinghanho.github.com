---
layout: post
title: "查看 Rails log 更好用的工具：rails_panel"
date: 2012-12-26 18:59
comments: true
categories: Programing
---
今天早上 [Xavier Noria][fxn] 轉推 [RubyFlow][rubyflow] 一則 [tweet][rubyflow-tweet]，看到這個有趣的工具，能用更友善的介面來顯示 `rails server` 下的 log 資訊，這東西就是 [rails_panel][rails-panel]。

[fxn]: https://twitter.com/fxn
[rubyflow]: http://www.rubyflow.com/
[rubyflow-tweet]: https://twitter.com/rubyflow/status/283689153747644417
[rails-panel]: https://github.com/dejan/rails_panel

安裝方式很容易，先在 Google Chrome Store 上下載 [rails_panel][rails-panel-extension] 這個擴充套件，再把 [meta_request][meta-request] 這個 gem 加進 Gemfile，`bundle install` 後重啟伺服器，然後開 Chrome 的開發者面板就可以看到新的標籤頁。

[rails-panel-extension]: https://chrome.google.com/webstore/detail/railspanel/gjpfobpafnhjhbajcjgccbbdofdckggg
[meta-request]: http://rubygems.org/gems/meta_request

使用前：

![rails-server-log](http://lh6.googleusercontent.com/-sQP75CibCDg/UNrWZy9ID9I/AAAAAAAAFf8/w0WgsGmo2RQ/s690/%25E8%259E%25A2%25E5%25B9%2595%25E5%25BF%25AB%25E7%2585%25A7%25202012-12-26%2520%25E4%25B8%258B%25E5%258D%25886.19.47.png)

使用後：

![rails-panel](http://lh4.googleusercontent.com/-JUS4VoFaWwE/UNrNeRUEqkI/AAAAAAAAFfo/k69Phmixw8Q/s690/%25E8%259E%25A2%25E5%25B9%2595%25E5%25BF%25AB%25E7%2585%25A7%25202012-12-26%2520%25E4%25B8%258B%25E5%258D%25885.59.32.png)

超級清楚！我不曉得其他人都用什麼方式檢視 log，不過這目前是我用過最好的工具，對於查看 view render、資料庫拉資料的時間以及傳遞的參數內容都非常方便，推薦！