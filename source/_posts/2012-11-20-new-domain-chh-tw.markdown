---
layout: post
title: "註冊了新域名 CHH.tw"
date: 2012-11-20 16:36
comments: true
categories: Life
---
原本的 whiteball.tw 這個月底到期，趁著最近有個活動促銷，趕快買了一個 .tw 結尾的 CHH.tw，沒意外的話以後應該是不會再改了。今天下午花了一點時間，把 CloudFlare 上的 record 設定全部搬家，順便刪除一些已經沒用的網址。

為什麼網址要取 CHH？當然因為這是個人網站，我的名字 Ching-Han Ho 縮寫就是 CHH，另一方面因為我的偶像 David Heinemeier Hansson{% fn_ref 1 %} 縮寫是 DHH 的關係，兩個字很像啊。XD

2012.11.21 更新：今天 DHH 在 [Twitter][dhh-twitter] 上報喜說他兒子出生了，叫做 Colt Heinemeier Hansson！XD

反正本站一直以來都是「微流量」，就算不做 [HTTP 301 導向][301-and-302]其實也沒什麼差啦，所以不想多花時間寫腳本，只是到 Google 和 Bing 的站長工具去上傳了一下新網址的網站地圖，還有把一些平常在追流量的分析工具改一下設定。

然後又演練一次把新網址跟 Google App 結合，這次花的時間只有十分鐘超快，果然熟能生巧。以後就可以到處用 @chh.tw 的信箱，多麼帥氣啊。:p

在這邊分享一個 Gmail 的小技巧，有時候在外面用信箱註冊怕信箱被該網站賣給 spammer，這個雖然沒辦法預防，但是可以知道信箱是被誰傳出去的，因為一般的網站是不會對信箱進行字串處理，所以可以在信箱加上 `+` 符號，後面接著註冊的網站名稱，例如：

example+fb@chh.tw

這樣郵件還是會寄到 example@chh.tw 這個信箱，但我們可以知道這是從 fb 這個網站寄來的，如果你收到垃圾信件，你就知道你的信箱是誰給你洩漏出去的了。現在新的[個資法][person-law]這麼兇，用這招去告死對方吧！XD

再見了，Whiteball！:)

[dhh-twitter]: https://twitter.com/dhh/status/271209790674464768
[301-and-302]: http://blog.longwin.com.tw/2009/10/http-status-redirect-301-302-diff-2009/
[person-law]: http://www.techbang.com/posts/10878-law-of-personal-data-protection-of-the-interests-of-you-and-me-on-several-networks-of-common-funding-legal-issues-pchome-201-science-and-technology

{% footnotes %}
{% fn David Heinemeier Hansson 是 Ruby on Rails 框架的發明者，是我景仰的 hacker 之一。 %}
{% endfootnotes%}