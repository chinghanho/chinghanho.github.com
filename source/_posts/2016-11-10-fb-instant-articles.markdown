---
layout: post
title: "Facebook Instant Articles「即時文章」是什麼？"
date: 2016-11-10 00:53
comments: true
categories: Tech
---

![速度](https://lh3.googleusercontent.com/QFqpxXZV3qUjUcR3LLrV3R5rcEPxvm-4DIP7Can_KyABedGdX9MEBFKYEasCx3VwVzoAZN9PqHmigIkyAkzMPswzcg1-BNiQxxPsKC3BhwWSG_ZboFlvygNePknmJAUlo-sCxAZMOwl8F1ZujQp-i_YxnvXEMJkSDqSYHNQESg7mCISjGUymV7KcQe-8hFh8jVhd7lHZkYWoeXwgJXjNPVYzzGcUkA26mnpSVo4yaQyf0aesuY-5-KIRxzb1WgCPUuhmsO5dTgTsTxU0zXBAX_XfldjuJ8AUx0SqOOPkzE9XKu6-Kw_rbTuoW07AeUAro_9Ly4sJgYf1yU3AF77W27uOtPWCQGf2eZypNES5NvyH8pgmvdZislP_hLzJ3vms8ffc3xqe1woCxDK69vfGyOQvQkgWP1o8C1a19sRaUncjhHVr_5AT17eqsRC8UHA_wB6o9XxXf26OOHlMQoQVJtAXMZNTvQY_ZSQvOqOwdETRCndDsWTy6Bm9dFp2ZWA19DdtVQkrwFMIQmvee58CVoH5d8cHe7JGOq-k3tm1P8U96PE9-XnqeJWPd_XyPQ_Z7oqQrDoloMXlHOovYlOwSvgcu44d8NaBhCMo0UopW5mUcEYWYA=w1280)

行動裝置時代瓶頸：

* 手機頻寬小；
* 手機速度慢；

但是人們希望：

* 速度快；
* 速度要快；
* 速度更快；

快有什麼好處？遠古的網路傳說：

* Amazon 網頁平均載入時間多 1 秒，公司年度營收少 16 億美元；
* Google 網頁平均載入時間多 0.4 秒，每天搜尋次數減少 800 萬；
* 使用者開啟網頁 4 秒內沒有出現任何內容便會放棄；

所以 Facebook 推出「[即時文章](https://developers.facebook.com/docs/instant-articles)」，讓文章內容可以在毫秒之間顯示。

<!-- more -->

### 技術

即時文章原理：

* 簡單說，他們預先把內容載入到 FB app 裡，讓使用者可以「不離開 FB app」直接讀取內容，省去網路訊號來往半個地球的時間；
* 僅支援 iOS and Android，需要 iPhone iOS 7.0+ & FB app 30.0+，以及 Android Jelly Bean+ & FB app 57+。

我想要我的網站內容可以有即時文章，有什麼條件？

* 得要先有個網站；
* 遵守 FB [文章政策](https://developers.facebook.com/docs/instant-articles/policy)；
* 遵守 FB [設計規範](https://developers.facebook.com/docs/instant-articles/design/overview#design-guidelines)；
* 網站內容頁面程式結構要為「即時文章」做準備；
* 提交申請，至少準備 10 篇文章做「[文章審查](https://developers.facebook.com/docs/instant-articles/get-started/article-review)」。

「即時文章」要怎麼做？有以下幾種方式：

* RSS：網站要另外製作 RSS feed 的端口方便給 FB 抓資料，文件參考「[透過 RSS 摘要發佈](https://developers.facebook.com/docs/instant-articles/publishing/setup-rss-feed)」。
* API：網站內容頁面的結構需要特別設計過，符合 HTML5 語義話結構；然後網站的內容在 CRUD（建立、讀取、更新、刪除）每個動作，都要與 FB 的程式溝通一次。文件參考「[即時文章 API](https://developers.facebook.com/docs/instant-articles/api)」。
* 另外還有 SDK 跟 WordPress 套件，前者要用 PHP 不討論，後者要用 WordPress 架站。

實作了以上功能之後，要先提交 10 篇文章給 FB 進行「文章審查」，在那之前無法發佈即時文章。

一旦完成設定以及審查過程後，FB app 上有 Instant Articles 的內容會出現一個閃點符號，點了[幾乎可以毫秒級出現內容](https://twitter.com/mattjroper/status/598489543478751232)。

### 商業

內容都跑到 FB 上去顯示了，我的網站流量豈不是沒了？

* 對，因為讀者「不離開 FB app」；
* 可以用 [comScore](http://www.comscore.com/)、[Google Analytics](https://www.google.com.tw/intl/zh-TW/analytics/)、[Omniture](https://my.omniture.com/login/) 或 Facebook 自己的工具追蹤「即時文章」的流量；
* 因為網站本身流量減少，所以原本網站上的廣告收益減少；
* 但是可以在即時文章放廣告，詳細見「[即時文章的獲利方式](https://developers.facebook.com/docs/instant-articles/monetization)」。

我的內容在 FB app 上出現得好快好爽，都沒有副作用？FB 轉型做慈善事業？

簡單說，這是 FB 繼吃掉通訊平台、電商平台、支付平台的夢之後，另一個吃掉出版平台的夢。

首先，FB 為內容產出者提供非常好的平台，為許多媒體帶來非常大的流量、讀者和廣告收益等，但這有點像嗑藥，FB 是個一但染上很難戒除的癮，因為：

* 開始需要繳交「演算法維護費」跟「保護費」來維持流量；
* Facebook 網站如果掛了就無法導流；
* Facebook 有自己的一套「內容審查」決定權在他們，例如粉絲團被關閉的案例；

尤其當「即時文章」是要媒體把內容「直接顯示在 FB 上」，讀者連媒體自家網站都不用過去了，對於媒體來說，對自家網站的控制權變得更弱了。

所以，話說回來，我的網站適合「即時文章」嗎？ 以下是個思路：

* 問問為什麼當初選擇花錢自架網站？為什麼不去用免費的 BSP？
* 如果答案是說自己架網站可以對網站樣式、內容有更多的客製化跟掌控權……
* ……
* ……
* ……
* ……
* 那用「即時文章」等於放棄以上這些，那怎麼不去用 BSP？

報告完畢！👾
