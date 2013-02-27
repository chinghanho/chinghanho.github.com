---
layout: post
title: "Dropbot: Octopress Theme Inspired By Tweetbot For Mac"
date: 2012-07-19 00:32
comments: true
categories: Tech
---
{% img center https://lh6.ggpht.com/-xdTubdUGPac/UAbCwMr7OSI/AAAAAAAADoM/4Lv8EpDvqBQ/s700/tweetbot-head.JPG "Tweetbot head" "tweetbot-head" %}

自從安裝官方 Twitter.app 之後，因為超容易操作、而且超好看，所以我開始使用 Twitter 這個社群服務。我也嘗試過其他的 Twitter 用戶端軟體，例如：YoruFukurou、Twitterrific 以及比較功能多元的 HootSuite、TweetDeck 等等，可是用起來都不是很滿意。對我而言介面設計不夠吸引人，滿分就只剩下最多 70 分了，再加上操作體驗相當不友善的話，很容易就被我直接淘汰。

直到上個禮拜 iOS 有名的 Twitter 用戶端軟體 Tweetbot 終於推出 Mac 測試版本，我裝起來後一用就愛上她了，獨特的介面風格{% fn_ref 1 %}、良好的操作體驗、支持觸控版手勢，還可以記錄已經閱讀過的推文，同步在不同裝置上（雖然我現在沒有 iOS 裝置啦 XD）。所以我決定把我的網站主題模擬成 Tweetbot 的介面風格，取名做「Dropbot」！以下就著墨一下比較特別的特色。

## Construction Page

{% img center https://lh3.ggpht.com/-zWhkfW11NS0/UAbCvt7inSI/AAAAAAAADnc/juluiHRy2Zg/s700/tweetbot-for-mac-alpha-download-page.JPG "Tweetbot for Mac alpha download page" "tweetbot-for-mac-alpha-download-page" %}

從[下載頁面][10]開始我就對這個 app 就留下深刻的印象，會跳動的 Tweetbot egg 意味著仍然處在 alpha 階段，Twitter bird 還沒孵化出來，實在是很細膩的巧思。:p

{% img center https://lh3.ggpht.com/-X3A3dd3MPVk/UAf-VbOywrI/AAAAAAAADo0/kMFYuKdn66o/s700/blog-under-construction-page.JPG "Blog under construction page" "blog-under-construction-page" %}

首先借用了 Tweetbot 下載首頁那個工程條紋的圖片，依樣畫葫蘆做了一個[施工頁面][12]，原本想要 PS 一下那顆蛋放上來，可是手邊這台電腦沒軟體，所以暫時作罷。話說回來，其實我也不曉得做這個施工頁面幹嘛？XD 或許我應該把它挪到 [404 錯誤頁面][13]去才對。

## Navigation

{% img center https://lh6.ggpht.com/-5rob_QybFAI/UAbFqoD5hmI/AAAAAAAADoA/7pIxRXjZeOk/s1046/tweetbot-nav.JPG "Tweetbot nav" "tweetbot-nav" %}

Tweetbot 這個選單按鈕螢光發亮的設計迷死人{% fn_ref 2 %}，丟掉 Twitter 官方那個 app 的原因之一，就是我已經對玻璃反光感到審美疲勞了，Apple 超愛這個的。Tweetbot 選單列可以用快捷鍵 `command + 1~9` 快速切換還蠻方便的，或許改天可以來寫個 JavaScript，實作類似的功能在我的網站上。XD

{% img center https://lh6.ggpht.com/-3GxV6LImoVA/UAf-Cpdwf-I/AAAAAAAADo0/rFXS9BF5DOc/s700/blog-nav.JPG "Blog nav" "blog-nav" %}

原本我想要完全仿 Tweetbot 選單的樣式，可是那必須動到非 custom 目錄下的 HTML 檔案，所以我單純修改樣式表就好。還有啊，我把原本使用的 [Font Awesome][15] icon font 換成 [IcoMoon][20] 了，因為 Font Awesome 推特鳥 Logo 一直不更新，天哪！我這個 Twitter 用戶端軟體為基礎的主題，怎麼可以用舊的 Twitter Logo{% fn_ref 3 %}？XD

## Footer

{% img center https://lh6.ggpht.com/-aog57nhnOm8/UAbClXfQfTI/AAAAAAAADnU/YYkuKwPgdwE/s700/tweetbot-footer-version.JPG "Tweetbot footer version" "tweetbot-footer-version" %}

左下角這個小圖案也是相當有趣，黑黃色條紋代表測試版開發階段，正式版應該就不會出現了。我很喜歡這個小細節，不需要特別寫上 beta 或 alpha 字樣，透過簡單的小圖案就可以充分理解。

{% img center https://lh4.ggpht.com/-9Peb6oTf7mY/UAf-BFTUYkI/AAAAAAAADo0/kAXIYCXHuC0/s700/blog-footer-construction.JPG "Blog footer construction" "blog-footer-construction" %}

之所以叫做 Dropbot 是因為上一套佈景我原本取名叫做 Droplet，現在以 Tweetbot 介面設計為基礎，就把兩個名字合體了。由於 Dropbot 也仍然處在開發階段，還有很多細節跟功能還沒實現，所以我也在網站右下角加上了這個小圖案，不過我的這個是用 CSS 畫的喔{% fn_ref 4 %}！:)

{% footnotes %}
	{% fn 其實只要不是直接套用 Mac 系統那種視窗風格的介面，我都覺得很好看。 %}
	{% fn 我發現我對藍色情有獨鍾，例如 <a href="http://www.iawriter.com/">iA Writer</a> 那個水藍色的游標。XD %}
	{% fn 真正的原因是我覺得 IcoMoon 比較好看啦！:p %}
	{% fn 在解析度不是非常高的情況下，向量繪圖我認為還是有很大的問題，因為反鋸齒的關係，會把線條拉扯得很難看，所以現在呼聲很高的向量網頁設計可能沒這麼簡單，在很重要的地方還是直接套圖吧。 %}
{% endfootnotes %}

[10]: http://tapbots.com/tweetbot_mac/
[12]: ../../under-construction
[13]: ../../404
[15]: http://fortawesome.github.com/Font-Awesome/
[20]: http://keyamoon.com/icomoon/
