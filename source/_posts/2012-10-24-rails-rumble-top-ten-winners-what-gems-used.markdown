---
layout: post
title: "Rails Rumble 贏家們都用哪些 Gems"
date: 2012-10-24 08:27
comments: true
categories: Programing
---
![](https://lh5.googleusercontent.com/-qvZPDwZ2xTQ/UIdKILEJtOI/AAAAAAAAEWI/Jzwy2lzNIKc/s720/rails-rumble-logo-large.png)

Rails Rumble 是國外相當有名的 Hackathon 大賽，是一場考驗 hacker 們的~~爆肝力~~體力和智力的「遊戲」，必須 48 小時之內用 Ruby on Rails 或是其他 Rack-based 的 Ruby 框架寫出一個「最小可行性產品{% fn_ref 1 %}」。

48 小時內能完成什麼網站？[Rails Rumble][rails-rumble] 網站上列出了今年 2012 贏得大賽的前十名作品，例如第一名的 [findthin.gs][findthin-gs] 可以找出電視影集和電影的線上資源，讓你選擇網路上直接收看或是線下購買回家。

[rails-rumble]: http://railsrumble.com/entries/winners
[findthin-gs]: http://grumpy-cat.r12.railsrumble.com/

Rails 開發網站果然很快很強大，也仰賴 Ruby 語言的「[膠水][glue]」特性，利用「工具箱哲學」可以用很多簡單的小工具，快速拼裝起來一個成品。剛剛看到 [Dwellable][Rails-Rumble-Winners-Gem-Teardown] 部落格寫了一篇文章，訪問 top 10 的參賽者，挖出他們的 Gemfile 窺探他們到底用了哪些 Gems 來「組裝」網站。感覺挺有趣的，所以我就順手翻譯了一下我看得懂的部分。

[glue]: http://en.wikipedia.org/wiki/Glue_language
[Rails-Rumble-Winners-Gem-Teardown]: http://www.dwellable.com/blog/Rails-Rumble-Winners-Gem-Teardown

## 前端技術

![](http://lh4.googleusercontent.com/-Y7Dm1PZ_Yjc/UIdDEnDyirI/AAAAAAAAEVQ/s5gfmI5TIZw/s642/%25E8%259E%25A2%25E5%25B9%2595%25E5%25BF%25AB%25E7%2585%25A7%25202012-10-24%2520%25E4%25B8%258A%25E5%258D%25889.21.33.png)

* [jQuery][jquery]：每個團隊都用到了 jQuery 這套輕量的 JavaScript 函式庫；
* [CoffeeScript][coffescript]：9 個，顯然大家都不喜歡單純的 JavaScript；
* [Bootstrap][bootstrap]：6 個團隊採用這套由 Twitter 所維護的開源框架，對於不擅長前端的後端開發者而言，這真的非常好用；
* [Sass][sass]：9 個，Sass / SCSS 讓 CSS 寫起來更友善，而且更容易維護，尤其是當上了 Compass 後威力更強大；
* [HAML][haml] / [Slim][slim]：有 5 個團隊採用 HAML，有 3 個團隊採用 Slim，比起 HAML 來說 Slim 感覺是更輕量的引擎{% fn_ref 2 %}；

[jquery]: http://jquery.com/
[coffescript]: http://coffeescript.org/
[bootstrap]: http://twitter.github.com/bootstrap/
[sass]: http://sass-lang.com/
[haml]: http://haml.info/
[slim]: http://slim-lang.com/

## 後端技術

### 資料庫

![](http://lh4.googleusercontent.com/-Ak4RSlmv_04/UIdDEk1el2I/AAAAAAAAEVI/Zu78v5jhAH0/s635/%25E8%259E%25A2%25E5%25B9%2595%25E5%25BF%25AB%25E7%2585%25A7%25202012-10-24%2520%25E4%25B8%258A%25E5%258D%25889.21.54.png)

近年來 MongoDB{% fn_ref 3 %} 很火紅，還是只有在 Rails 圈內（？）。不過常被詬病的大數據處理的性能，我沒用過所以我也不曉得，但是在小數據的處理上我想大概沒什麼太大爭議，在這種 Hackathon 場合上用起來應該也是很適合的。

在 Top 10 團隊當中，有 5 組採用 MySQL 資料庫，其他有 3 組採用 MongoDB，另外兩組用的是 Postgres。而其中有 5 組團隊採用了 Redis 這套開源的緩存資料庫系統。

### 測試

![](http://lh3.googleusercontent.com/-5Yx1TxaXPCI/UIdDE3KMwOI/AAAAAAAAEVM/oQgMay8odFE/s630/%25E8%259E%25A2%25E5%25B9%2595%25E5%25BF%25AB%25E7%2585%25A7%25202012-10-24%2520%25E4%25B8%258A%25E5%258D%25889.22.05.png)

其中有 6 個團隊使用 RSpec 來寫測試，雖然他們也有用到其他測試相關的 gems，不過這裡提到的是最主要的。讓我感到訝異的是在 Hackathon 這麼短的時間內，他們竟然還有時間來寫測試？不曉得覆蓋率多少就是了。當然，也有團隊是完全不寫測試的。:p

## 佈署策略

[Capistrano][capistrano] 看來還是最多人用的，有 7 個團隊用它來佈署應用程式，其中有個團隊使用的是 [Mina][mina]。在記憶體快取方面，有 2 個團隊用了 [Dalli][dalli] 這套軟體，其他的團隊或許沒有用或者是用 Redis 來替代。

[capistrano]: https://github.com/capistrano/capistrano
[mina]: https://github.com/nadarei/mina
[dalli]: https://github.com/mperham/dalli

### 第三方服務

大量應用第三方網站的服務來「組裝」是一種趨勢，尤其對於網站的初期來說{% fn_ref 4 %}。比方說寄信這檔事好了，寄給一個人或是寄給十個人很簡單，但是如果今天要一次寄出幾千封 eDM 就是一門專業學問了，專業的還是讓給專業的去搞吧。而 Rails Rumble 比賽中就有 2 個團隊採用 [MailChimp][mailchimp] 來處理這方面的問題。

[mailchimp]: http://mailchimp.com/

此外，有 3 個團隊使用了 [Pusher][pusher] 這個服務來 host 即時訊息系統。

[pusher]: http://pusher.com/

### 認證系統

![](http://lh3.googleusercontent.com/-Gc6_Zwxm2Jw/UIdDFzvUbeI/AAAAAAAAEVw/4Nw_BVpJ2gc/s607/%25E8%259E%25A2%25E5%25B9%2595%25E5%25BF%25AB%25E7%2585%25A7%25202012-10-24%2520%25E4%25B8%258A%25E5%258D%25889.22.39.png)

大多數的團隊使用的是富具盛名的 [Devise][devise]，也有少數幾個團隊用的是「純」[OmniAuth][omniauth]。大多數的開發者都會把 OmniAuth 直接整合進 Devise。

[devise]: https://github.com/plataformatec/devise
[omniauth]: https://github.com/intridea/omniauth

## 該來用看看的 Gems

* [Pry][pry]：irb 的替代品，當然不是單單「替代」這麼簡單，能替代當然是因為它非常強大的 debug 功能，Rails Rumble top 10 團隊中就有 6 個團隊使用它；
* [Redcarpet][redcarpet]：[Markdown][markdown] 語法的解析器，也是我常用的 gem 之一；
* [UserAgent][useragent]：用來解析 HTTP user agent，可以判斷你是用什麼裝置來上網的；
* [Whenever][whenever]：用 Ruby 產生例行性的工作，我之前寫推文演算法用的是 [rufus-scheduler][rufus-scheduler] 這套；
* [Stamp][stamp]：用友善的時間表示法來表示日期，替代原本 `%m` 這種不曉得是 month 還是 minute 的格式。

[pry]: https://github.com/pry/pry
[redcarpet]: https://github.com/vmg/redcarpet
[markdown]: http://markdown.tw/
[useragent]: https://github.com/josh/useragent
[whenever]: https://github.com/javan/whenever
[rufus-scheduler]: https://github.com/jmettraux/rufus-scheduler
[stamp]: https://github.com/jeremyw/stamp

## 後記

Hackthon 好像很好玩，不過以我現在的實力去參加，恐怕是被碾假的吧。前幾天才剛結束 Yahoo! Hack Day，最後獲勝者是 [Polydice][polydice] 報名的兩個隊伍，結果兩個隊伍全得獎了。XD

[polydice]: http://tw.polydice.com/2012/10/22/yahoo-open-hack-day-2012-get-your-ideas-into-action-appvengers/

而前幾個禮拜 Facebook 也來台灣舉辦 Facebook World Hack，台灣相當強的 Rails 高手 XDite 贏得最後的大獎，也在她的部落格分享[獲勝心得][how-to-win-hackthon]，其中我覺得相當有參考價值的是：

[how-to-win-hackthon]: http://blog.xdite.net/posts/2012/10/20/how-to-win-hackthon/

> 評審希望得獎的題目是 valuable 的。搞笑、諷刺、主題模糊的題目，一定不會得獎。（我在 2009 的比賽跟一群大神等級的朋友合作做了一個精美的搞笑網站「[我是專家][i-am-expert]」，但是……我們沒有得獎）

[i-am-expert]: http://wp.xdite.net/?p=1472

看到這裡再去瞄一下 [Rails Rumble 前十名][rails-rumble]，都是非常有實用性的網站。所以下次如果要去參加 Hackathon 比賽，我想選好題目是非常重要的。

{% footnotes %}
{% fn <a href="http://www.books.com.tw/exep/prod/booksfile.php?item=0010547374" alt="精實創業">精實創業</a>所提倡的一種概念，產品或服務不需要等到「完美」才推出，只要「堪用」能夠滿足使用者需求就夠了。 %}
{% fn <a href="https://github.com/klaustopher/hamlerbslim" alt="hamlerbslim">Github</a> 上有人用 <a href="http://httpd.apache.org/docs/2.2/programs/ab.html">Apache Benchmark</a> 做了一個實驗，測試 ERb、HAML、Slim 這三套樣板處理引擎的性能，有興趣可以參考看看。 %}
{% fn 值得提一下的是最近 <a href="https://education.10gen.com/" alt="10gen">10Gen</a> 在線上開課，可以上免費的 MongoDB 專業課程。 %}
{% fn 例如國內的 <a href="http://www.inside.com.tw/2012/01/30/icook-cloud-service" alt="icook-cloud-service">iCook</a> 就用了非常多的網路服務來「組裝」。 %}
{% endfootnotes %}