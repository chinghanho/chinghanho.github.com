---
layout: post
title: "Ruby on Rails 伺服器的選擇"
date: 2012-11-18 07:35
comments: true
categories: Programing
---
2012.11.25 更新：新增一些內容。

Ruby on Rails 佈署方案五花八門，對於新手如我而言，看到一堆 Apache、Nginx、Mongrel、Phusion Passenger 和 Capistrano 這些名詞，可是一直搞不太清楚他們之間有什麼異同。

今天在 [StackOverflow][stackoverflow] 上搜尋，讓我找到這一篇文章「[Ruby on Rails Server Options][ruby-on-rails-server-options]」，終於搞懂他們之間的差別，所以順手翻譯了一下，還有加上一些自己知道的東西。

## Apache vs Nginx

Apache 和 Nginx 都是 web server，就是能吐出靜態檔案，或是吐一些動態內容（搭配適當的模組）給 client 的伺服器。目前最流行的是 Apache，功能相當的多也相當的齊全，相較下 Nginx 是小而美的輕量伺服器，但絕對不要誤會它是小眾玩具。

Apache 和 Nginx 專注在處理 HTTP 的效能上，所以對於靜態檔案的吞吐都有卓越的表現。但是無法開箱即用（out-of-the-box），也就是沒辦法直接執行 Ruby、Python 或是 PHP 等等的其他語言，而要倚賴 FastCGI或是代理方式{% fn_ref 1 %}，讓其他的 application server 來處理。

application server{% fn_ref 2 %} 是種能夠將 HTTP request 送到程式碼（也就是寫好的 Rails app）裡面去處理的 server，既然能處理 HTTP request，所以它當然也可以充當 web server 的角色，可是會這樣用的人不多，因為他並不是真的這麼擅長這件事，原因後面會講{% fn_ref 3 %}。

## Mongrel vs WEBrick

Mongrel 是一個 Ruby 的 application server，具體來說他能夠用來：

1. 在它自己的 process space 讀取 Rails app{% fn_ref 4 %}；
2. 建立 TCP socket 跟外面的世界（網際網路）保持通訊，隨時把收到的 HTTP request 傳送到 Rails app，反過來則是把 Rails app 回應的 HTTP response，透過 socket 丟回到 client；

WEBrick 也是做類似的事情，不同的地方是：

1. 完全用 Ruby 寫的，Mongrel 則是 Ruby 跟 C 的混合體（HTTP 解析器是用 C 寫的，因為效能比較好{% fn_ref 5 %}）；
2. WEBrick 速度很慢，而且不夠強壯，開發的時候用起來很方便（WEBrick 預設包含在 Ruby 裡，Mongrel 則是要另外安裝），可是挪到 production 上一下子就會被打死，而且已知有 memory leak 和 HTTP 解析等問題；

還有其他常見的 Ruby application server，例如 [Unicorn][unicorn] 和 [Thin][thin]，他們跟 Mongrel 很類似，但是都有自己的一些特點，可以在網路上找找別人寫的效能測試，來評估一下要用哪一種方案。

## Mongrel

雖然 Mongrel 可以拿來做 web server 相同的事情，但是沒有人會直接這樣用，因為 Mongrel 本質上是個 Ruby application server，負責把 request 帶到 Ruby code 裡面處理，然後再把處理完的東西用 HTTP 丟出來，並沒有辦法同時處理多個 request，也不擅長處理效能上的問題。

舉例來說，當碰上慢吞吞的用戶，不只發過來的 HTTP requests 很慢，就連接收也超級慢，這樣老實的 HTTP server 就會被這傢伙卡住動彈不得，非得等到把這項工作完成才能繼續處理下一個用戶。更糟的是如果遇到壞人故意把 socket 留在那裡不讀取 HTTP 的 response，這樣就會造成 Ruby 永遠沒辦法處理接下來的任何 requests。

儘管 web app 可以有很多解法，但是專業的還是交給專業的去做，更好的方式就是將這些任務交給 web server，因為這本來就是他們擅長的核心商業邏輯。

換句話說，當拜訪一個 Rails 網站絕對不會直接跟 Mongrel 接觸到，它總是放在 Apache 或是 Nginx 後面扮演 reverse proxy 的角色，這麼做有幾個原因：

* 由於每個 Mongrel-served 的 Rails app 同時只能處理一個 request，如果想要一次多跑幾個，那就需要同時多 run 幾個 Mongrel instance，而每個 instance 都為同一個 Rails app 做牛做馬，這種架構稱為「Mongrel cluster（一堆雜種？XD）」。Apache 或是 Nginx 會自動分配 request 到不同的 instance 之間；
* 儘管 Mongrel 也可以吐靜態檔案，可是它並不是特別擅長這檔事，專業的交給專業的來，Apache 和 Nginx 都可以做得比它更快更好，所以大部分的人都會讓後者來提供靜態檔案；

如果因為 Rails app 中某個 bug 讓其中一個 instance 崩潰，那也只需要重新啟動那個 instance 就好。不過 Mongrel 則需要隨時監控，如果出問題要自己去重新啟動。

## Phusion Passenger

[Phusion Passenger][phusion-passenger] 也是一個 Ruby 的 application server，但是運作方式很不一樣，它直接整合到 Apache 或者 Nginx 裡，只需要編輯 web server 配置檔案，指定到 Rails app 的 public 目錄，就可以讓 Rails 動起來。

因為 Phusion Passenger 是基於 Rack 實作的，所以可以與任何 Rack-base 的 Ruby 框架應用程式相容。而 Rails 從 3.0 版本開始已經完整兼容 Rack 介面，只有早期的版本還沒有全部相容，不過 Phusion Passenger 仍然可以支援先前 1.x 和 2.x 的版本。

使用 Phusion Passenger 的好處就是不必生那麼多「雜種（Mongrel）」，而且還有很多優點：

* 當網站突然忙碌起來的時候，Mongrel cluster 能夠處理的 process 數量是固定的，在這方面 Phusion Passenger 完全會自動調適，當網站不忙的時候 process 也會自動關閉節省系統資源的消耗；
* 如果 Rails app 崩潰了，Phusion Passenger 很貼心地、很聰明地會自動啟動；
* 除此之外，Phusion Passenger 是用 C++ 寫的，所以效能非常好，速度 very very 快。:p

Phusion Passenger 原本的名字是 mod_rails，目的跟 mod_php 一樣，為了讓 Apache 或 Nginx 能夠執行 web app。如果遇到系統崩潰，也能噴出有用的錯誤訊息來修復系統。

基於以上這些原因，Phusion Passenger 成為目前最受歡迎的 Ruby application server，之前寫過佈署的過程，可以參考「[如何佈署 Rails 應用程式到 Amazon EC2][deploy-rails-app-on-ec2]」。

Phusion Passenger 其實也可以跟 Mongrel 一樣，可以不需要 Apache 或是 Nginx 而拿來直接當 web server 用；在 Rails app 中下 `passenger start` 指令，它將會啟動一個 Phusion Passenger web server，直接透過 port 80 來使用。當然，Mogrel 在 HTTP 處理上的缺點它也同樣會有。

## Capistrano

[Capistrano][capistrano] 既不是 web server 也不是 application server，它是完全不同的東西。在把 Rails app 佈署到 application server 上之前，同樣會需要一些事前的準備工作，像是：

* 上傳 Rails app 程式碼和檔案到伺服器；
* 安裝 app 倚賴的 libraries；
* 建立或是 migrating 資料庫；
* 啟動或是終止應用程式倚賴的任何 daemon，例如 DalayedJob workers 等等之類的；
* 讓 app 運作起來所需要的其他東西；

Capistrano 是一個工具，負責用來處理上述這些事前的準備工作，可以先告訴 Capistrano 每一次佈署新版本的 app 有哪些指令需要執行，或是伺服器在哪裡，讓整個佈署流程自動化。

這並不是佈署 Rails 過程中必要的工具，如果偏好一切 DIY，也可以全部自己動手來做，如果只是自己一個人那還沒什麼問題，可是如果專案是整個團隊在維護，或是機器很多的時候，難保所有人都能如預期好好地把 app deploy 到機器上去，所以這時候最好就用 Capistrano。

[stackoverflow]: http://stackoverflow.com/
[ruby-on-rails-server-options]: http://stackoverflow.com/questions/4113299/ruby-on-rails-server-options
[phusion-passenger]: https://www.phusionpassenger.com/
[deploy-rails-app-on-ec2]: /posts/deply-rails-app-on-ec2/
[capistrano]: https://github.com/capistrano/capistrano
[thin]: http://code.macournoyer.com/thin/
[unicorn]: http://unicorn.bogomips.org/

## 其他參考資料：

* [Phusion Passenger design & architecture](http://www.modrails.com/documentation/Architectural%20overview.html)
* [Deploying with Capistrano](http://guides.beanstalkapp.com/deployments/deploy-with-capistrano.html)

{% footnotes %}
{% fn 所謂的代理方式就是，Apache 和 Nginx 就是說把收到的 HTTP request 轉送到另一台伺服器去處理，然後再把 HTTP response 轉送回 client。 %}
{% fn application server 透過各種通訊協議，例如 HTTP、FastCGI、SCGI、AJP 等等，來連接到 web server。 %}
{% fn 應該這麼說，web server 和 application server 是一種概念上的區分，並沒有說一定是哪一種 server。 %}
{% fn 這句話是直接照翻，我還不是很懂所謂的 process space 是指什麼。 %}
{% fn C 語言是一種低階語言，少了許多解譯的動作，所以速度會比較快。 %}
{% endfootnotes %}