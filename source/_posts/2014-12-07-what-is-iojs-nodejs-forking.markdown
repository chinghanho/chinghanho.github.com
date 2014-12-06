---
layout: post
title: "io.js 是什麼？Node.js 社群分裂事件觀察"
date: 2014-12-07 04:29
comments: true
categories: Tech
---

![](https://lh3.googleusercontent.com/-mDHp26oTCxg/VINpfJZV2ZI/AAAAAAAAIJk/FlmyYl4aRBY/w770-h333-no/Screen%2BShot%2B2014-12-07%2Bat%2B4.38.35%2BAM.png)

這個事件已經燒了好幾天，剛開始注意到是 Hacker News 上出現「[IO.js – Evented I/O for V8 javascript](https://news.ycombinator.com/item?id=8669557)」這篇推文，我甚至還不曉得到底該寫 IO.js 還是 io.js 比較正確。

整件事情發展詭譎得像 [Prometheus](http://www.imdb.com/title/tt1446714/) 的 Elisabeth Shaw 剛發現黑色黏液一樣，不知道到底是希望還是崩壞的開始。最近公司有要用 Node.js 所以我就追了一下新聞，大概統整出來的概況寫了這篇觀察紀錄，或許有些可能有錯麻煩在幫我指正，感謝喔！

<!-- more -->

### 故事的序章

希臘傳說中的巨人族自殺喝下奇怪的黑色黏液來開創人類世界，Node.js 的創建者 [ry](https://github.com/ry)（Ryan Dahl） 確實也有點像上帝創世後神龍見首不見尾地消失無蹤，怎麼有種 [_why](http://www.smashingmagazine.com/2010/05/15/why-a-tale-of-a-post-modern-genius/) 風格的感覺……都回到真實世界去了嗎（[The Matrix](http://www.imdb.com/title/tt0133093/) 梗）？

在 ry [消失](https://groups.google.com/forum/#!topic/nodejs/hfajgpvGTLY)之前他把 Node.js 版權賣給了 [Joyent](https://www.joyent.com/) 還替他們工作了一陣子，但稍微有點信仰的 developer 都不會喜歡參與的 Open Source 專案跟商業公司扯上關係，這次 io.js 事件主導人之一的 [mikeal](https://github.com/mikeal)（Mikeal Rogers）就寫了一篇「[On Corporate Ownership of Open Source](https://medium.com/@mikeal/on-corporate-ownership-of-open-source-786ebd15847e)」說出他的想法。

Node.js 有另外兩個非常重要的 core commiter 分別是 [piscisaureus](https://github.com/piscisaureus)（Bert Belder）和 [bnoordhuis](https://github.com/bnoordhuis)（Ben Noordhuis），他們成立的公司 [StrongLoop](http://strongloop.com/) 想要搶奪 Node.js 社群統治權的野心非常強，是 Joyent 最大的兢爭對手，但無奈商標權就是 Joyent 的，StrongLoop 也不能拿他們怎麼辦。

StrongLoop 你就直說吧！你就是想要取代 Joyent 來主導 Node.js 的發展！XD

### 分裂的跡象

Open Source 這回事嘛……就是授權範圍允許，當然可以 fork 一份出去改成自己想要的版本，只是看整個社群願不願意支持而已。我不是很了解 Node.js 核心的開發上，Joyent 是用什麼樣的風格來管理，但顯然有很多 core commiter 不太滿意，從很久之前就出現了分裂的聲音。

Joyent 做為 Node.js 所有者、掌門人也聞到了這個味道，當然不會坐視這種情況不管，因此成立了所謂的 [Advisory Board](http://nodejs.org/about/advisory-board/)，目的就是在於廣納所有 Node.js developer 的意見跟想法。

呃，傾聽人民的聲音？但似乎此舉在某些人眼裡看起來就是獨裁者的作秀，實質上沒什麼幫助，所以熱血革命鬥士開始組織 [Node Forward](http://nodeforward.org/) 來討論該怎麼「推翻」政府，最後有了今日的 [io.js](https://github.com/iojs/io.js/)。

說說看在這之間難道 StrongLoop 的人難道不會煽風點火嗎？:p

### io.js 是什麼？

io.js 目前在 Github 上的專案位置：[https://github.com/iojs/io.js](https://github.com/iojs/io.js)

它的前身是 [node-forward](https://github.com/node-forward/)/node，也就是 Node Forward 用來抵抗 Joyent 的專案，另一個 Node.js fork 版本，在正式 public release 前醞釀了大概 4 個禮拜左右。

io.js 目的是希望可以讓 Node.js 更加擁抱社群、更自由、更公開、更透明……之類的，而不是被 Joyent「跋扈的專制統治」，其實我也不曉得真實狀況是怎樣。總之，目前的核心成員有原本 Node.js 的重要貢獻者：

* Fedor Indutny（@indutny）
* Trevor Norris（@trevnorris）
* Ben Noordhuis（@bnoordhuis）
* Isaac Z. Schlueter（@isaacs）
* Nathan Rajlich（@TooTallNate）
* Bert Belder（@piscisaureus）

也有邀請 Advisory Board 大家長 TJ Fontaine（@tjfontaine）但是被婉拒了，劇情超像 [Jimmy's Hall](http://www.imdb.com/title/tt3110960/) 的神父啊！lol

### 賤人就是矯情

StrongLoop 在 io.js 公諸於世之後發表了「[StrongLoop’s Position on io.js](http://strongloop.com/strongblog/position-on-io-js/)」文章，來表示「其實我們還是愛著 Node.js 啦！但如果……」，彷彿是在向 Joyent 下達最後通牒，不好好處理我們就分手吧！

分裂其實對於整個 Node.js 生態系來說絕對是有害無益，但 StrongLoop 的態度就是師馬昭之心。看吧！文末還不忘歌功頌德一下他們自己。

最近常在他們公司網站上看到自我宣傳的文宣，尤其是這次 v0.12 版本當中有很大比例的程式碼是由 StrongLoop 的人 commit 的，那是因為 v0.12 可以視為 v1.0 版本的候選版本，這也意味著在告訴大家 Node.js v1.0 正式版最強力的推動者是我們 StrongLoop 啊！

其實這也沒什麼不好，而且 io.js 給的方向確實就是現在 Node.js 需要處理以及解決的問題，純粹是一種給人的觀感差異罷了。:p

### 小結

「這是個政治問題，需要政治的智慧來解決」，已經無關乎技術。

整個事件對於 Node.js 還是有點影響的，那就是 Joyent 終於[發佈消息](http://blog.nodejs.org/2014/12/03/advisory-board-update/)說 v0.12 即將到來，可能也意味著 v1.0 正式版本要隨之釋出，不過……我想這已經無法阻止 io.js 那群人繼續 forking，接下來會如何繼續拭目以待吧！
