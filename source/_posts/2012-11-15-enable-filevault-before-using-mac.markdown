---
layout: post
title: "使用 Mac 前務必先開啟 FileVault 2 功能"
date: 2012-11-15 07:16
comments: true
categories: Tools
---
硬碟是相當精密的硬體，運作原理說起來簡單，其實非常複雜，如果要科普解釋，我通常會把硬碟比喻成「油畫」，如果畫錯了或是要修改，並不是用橡皮擦擦掉，而是用顏料直接覆蓋。

直接覆蓋就會有個問題，油畫可以用紅外線掃描來得知底層是什麼樣子，所以才會有人發現[蒙娜麗莎原來是有眉毛的][mona-lisa]。XD

可是硬碟如果有重要資料，這就變成很大的問題了，尤其是 [SSD 更麻煩][ssd]，所以這也是為什麼業界硬碟如果損壞了，都是採用[物理銷燬][google-destroy-hdd]這種方式，例如 Google 的資料中心：

![google-destroy-hdd](http://lh3.googleusercontent.com/-G0aVvazyu3I/UKQuYHiWVkI/AAAAAAAAFV4/0hR-0glZG-g/s690/%25E8%259E%25A2%25E5%25B9%2595%25E5%25BF%25AB%25E7%2585%25A7%25202012-11-15%2520%25E4%25B8%258A%25E5%258D%25887.49.52.png)

對於個人使用者而言，有時候用電腦不一定是「用到死」，也許中間會二手賣出，那也不大可能把硬碟砸爛再給對方，所以有一種方式就是啟用 FileVault 2 這個功能（Windows 上我記得好像也有類似的功能），替你的資料先行加密，這樣別人就算從廢棄電腦裡面讀出資料，也不會直接得到原始資料。

![filevault-2](http://lh3.googleusercontent.com/-1FanB4n87e8/UKQwaYadieI/AAAAAAAAFWA/8bI5u6ok61c/s690/%25E8%259E%25A2%25E5%25B9%2595%25E5%25BF%25AB%25E7%2585%25A7%25202012-11-15%2520%25E4%25B8%258A%25E5%258D%25887.59.13.png)

這必須在剛拿到電腦存入任何個人資料之前，就先設定好。

[mona-lisa]: http://big5.xinhuanet.com/gate/big5/news.xinhuanet.com/tech/2007-10/24/content_6928914.htm
[ssd]: http://www.ithome.com.tw/itadm/article.php?c=66722&s=1
[google-destroy-hdd]: http://youtu.be/avP5d16wEp0?t=1m15s