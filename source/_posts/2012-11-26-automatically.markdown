---
layout: post
title: "電腦自動化的可靠性"
date: 2012-11-26 21:09
comments: true
categories: Tech
---
![](http://lh4.googleusercontent.com/-o6yYk2tMzN8/ULNgfhVSOqI/AAAAAAAAFYw/7dDPvi695dk/s690/%25E8%259E%25A2%25E5%25B9%2595%25E5%25BF%25AB%25E7%2585%25A7%25202012-11-26%2520%25E4%25B8%258B%25E5%258D%25886.28.53.png)

電腦自動化是這麼回事，沒出事沒感覺，出點事哪管可能只是千萬之一，就好像不可容忍的災難。誰叫他是電腦，天生就沒有容錯空間，如果人們能夠把對待電腦嚴苛的要求，拿到政治和法律上，我想那才是真正的救贖吧？（笑）

儘管電腦有時候會有些 bug{% fn_ref 1 %}，不過它的可靠性和效率超過人類已經是顯而易見的，因為我們的生活中早就已經充滿了 [PLC][plc] 的東西，尤其是重要的基礎設施，包括工廠、食品加工、製藥、核能發電廠、捷運系統、金融系統、航空系統、國防系統等等，映入眼簾的一切都是電腦控制，全部都是仰賴電腦。

[plc]: http://en.wikipedia.org/wiki/Programmable_logic_controller

前幾個禮拜有親戚在台北往基隆路上被拍到超速，好心跟親朋好友們提醒該路段要小心，老媽就說 TOYOTA 的車有定速系統，一開始就把速度設定好就不怕會超速了，不過另外有位親戚對於這種定速系統倒是相當不以為然，認為永遠[不該相信這種電子設備][escape]。

[escape]: http://www.mobile01.com/topicdetail.php?f=260&t=2481108&last=32457665

這其實也沒有錯，如果哪天電腦不行了，當機、中毒、短路、掛掉、爆炸，總是必須要準備好退路，讓這些電腦停擺的同時，讓整個社會能夠繼續運作，不致於因為發生 [BSoD][bsod] 就讓全世界陷入黑暗，或者是因為出現……新接龍？

![](http://lh6.googleusercontent.com/-hi6uzTZrI1g/ULNbJiNE97I/AAAAAAAAFYg/hkzPj1BXQ58/s690/579179_4751139094694_1381059163_n.jpg)

[bsod]: http://en.wikipedia.org/wiki/Blue_Screen_of_Death

事實上很多時候根本搞不清楚真相，常常會陷入一種[專家說的都是對的][keiserens-nye-klaeder]的迷思，就像要等到日本核電廠爆掉了才能喚醒人們對於高科技的幻覺，甚至於加入積極反核的行列，可是在那之前核能專家們總是對大眾諄諄教誨核能是多麼地安全。

[keiserens-nye-klaeder]: http://blog.chh.tw/posts/keiserens-nye-klaeder/

扯開一下話題，前陣子被討論得沸沸湯湯的社會議題：廢死。到底應該寧殺一百也不肯錯放一人？還是寧放一百也不肯錯殺一人？前者可能變成的是政府濫權的武器，後者伸張人權後可能反而造成更多的傷害。我的看法很簡單，就是：

> 有沒有承擔後果的能力？

過年玩鞭炮要很小心，弄得不好可能炸傷自己也會炸傷旁邊的人，我可能基於自信和自負會去玩一下，可是如果這串鞭炮一不小心就會轟爛半顆地球，那我得坦承自己完全沒有承擔這種後果的能力，那我就不會去玩鞭炮{% fn_ref 2 %}。

電腦自動化很方便，很多時候它其實比人更可靠{% fn_ref 3 %}，而我們應該要重新去思考那些真正重要的事情，或許那並不包括電腦自動化這件事。

照片來源：[The Matrix](http://www.youtube.com/watch?v=IojqOMWTgv8)、[Facebook](https://www.facebook.com/photo.php?fbid=4751139094694&set=a.1249077985355.39557.1181623619)

{% footnotes %}
{% fn 其實大多數的 bug 也都是人為造成的。:p %}
{% fn 事實上我根本不敢玩鞭炮。XD %}
{% fn 以 Ruby on Rails Deployment 為例，這就是為什麼要用 <a href="/posts/ruby-on-rails-server-options/">Capistrano</a> 自動化佈署的原因。:) %}
{% endfootnotes %}