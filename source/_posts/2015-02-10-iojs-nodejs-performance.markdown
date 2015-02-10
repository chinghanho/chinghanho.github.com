---
layout: post
title: "io.js 1.1 VS node.js 0.12.0 VS node.js 0.10.36"
date: 2015-02-10 23:37
comments: true
categories: Programing
---

這篇文章不是要做深入的效能評比，只是最近 node.js v0.12.0 正式版釋出，又剛好人品很好地用 io.js 1.1 把專案建起來了，乾脆就在本機上做一下 node.js 新舊版和現在很熱的 io.js 效能紀錄。圖有點小，要嘛自己想辦法放大，要嘛就直接看結論吧！XD

### Page 1

![node.js v0.12.0 page1](https://lh3.googleusercontent.com/-AWI4LN6fELs/VNomvCU_iMI/AAAAAAAAIKs/Im1igPcG3EI/w1118-h331-no/Screen%2BShot%2B2015-02-10%2Bat%2B11.28.59%2BPM.png)

node.js v0.12.0 TTFB 1.36s。

![io.js v1.1.0 page1](https://lh6.googleusercontent.com/-tl-PFTsvs-0/VNomy_t_IkI/AAAAAAAAILU/YRiVxSVD5Dc/w1118-h331-no/Screen%2BShot%2B2015-02-10%2Bat%2B11.34.28%2BPM.png)

io.js v1.1.0 TTFB 1.21s。

![node.js v0.10.36 page1](https://lh4.googleusercontent.com/-pJ4XM3ov8v4/VNomx2fPf-I/AAAAAAAAILE/RxH-DOoQygg/w1118-h331-no/Screen%2BShot%2B2015-02-10%2Bat%2B11.30.32%2BPM.png)

node.js v0.10.36 TTFB 603ms。

<!-- more -->

### Page 2

![node.js v0.12.0 page2](https://lh4.googleusercontent.com/-5LGXJ2DInfM/VNomvEBYaQI/AAAAAAAAIK0/UyWZldWsB-4/w1118-h331-no/Screen%2BShot%2B2015-02-10%2Bat%2B11.29.22%2BPM.png)

node.js v0.12.0 TTFB 910ms。

![io.js v1.1.0 page2](https://lh3.googleusercontent.com/-CePlsRGHndI/VNomzP3ZMzI/AAAAAAAAILc/YlkhDkjRHAY/w1118-h331-no/Screen%2BShot%2B2015-02-10%2Bat%2B11.34.49%2BPM.png)

io.js v1.1.0 TTFB 776ms。

![node.js v0.10.36 page2](https://lh5.googleusercontent.com/-AemhYmtokBw/VNomx8aIBMI/AAAAAAAAILg/3GBjTwsTs1w/w1118-h331-no/Screen%2BShot%2B2015-02-10%2Bat%2B11.30.43%2BPM.png)

node.js v0.10.36 TTFB 604ms。

### Page 3

![node.js v0.12.0 page3](https://lh3.googleusercontent.com/-sbjgcsYBKEI/VNomvAQ99yI/AAAAAAAAIKw/1kKSgNjzysw/w1118-h331-no/Screen%2BShot%2B2015-02-10%2Bat%2B11.29.37%2BPM.png)

node.js v0.12.0 TTFB 221ms。

![node.js v0.10.36 page3](https://lh4.googleusercontent.com/-D1D5stJUzPg/VNomxwPjWOI/AAAAAAAAILI/Zj_XEEIcOxs/w1118-h331-no/Screen%2BShot%2B2015-02-10%2Bat%2B11.30.51%2BPM.png)

io.js v1.1.0 TTFB 217ms。

![io.js v1.1.0 page3](https://lh3.googleusercontent.com/-ZnrOiohW9VU/VNomzm0vZCI/AAAAAAAAILk/cT8R99I1tJc/w1118-h331-no/Screen%2BShot%2B2015-02-10%2Bat%2B11.35.08%2BPM.png)

node.js v0.10.36 TTFB 150ms。

### 結論

每個頁面我都土法重複 reload 過好幾次，大概抓的數字是看到的平均值，想說這樣的測試太土法了，沒參考性，但是三個頁面的測試結果彼此比較後都很相像，所以應該可以說明什麼了吧！

* node.js v0.12.0 TTFB 1.36s +0%
* io.js v1.1.0 TTFB 1.21s +12%
* node.js v0.10.36 TTFB 603ms +126%

頁面一舊版 node.js v0.10.36 比其他兩頁都快得多！跟 io.js v1.1.0 比起來也快了至少 100%。

* node.js v0.12.0 TTFB 910ms +0%
* io.js v1.1.0 TTFB 776ms +17%
* node.js v0.10.36 TTFB 604ms +51%

頁面二也是 node.js v0.10.36 最快。

* node.js v0.12.0 TTFB 221ms +0%
* io.js v1.1.0 TTFB 217ms +1.8%
* node.js v0.10.36 TTFB 150ms +47%

頁面三其實都相差不遠，但 node.js v0.10.36 仍然反應最快。

結論就是……等網站上到遠端 server 之後再來測一次真實網路環境下的 TTFB 吧！以目前在本機測試的結果，還沒有很想更新到 node.js v0.12.0，而且以[現在這局勢](http://blog.chh.tw/posts/what-is-iojs-nodejs-forking/)看來 io.js 勢頭很高，再過一段時間來評估直接把專案跳去改用 io.js 算了。:p
