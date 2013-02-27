---
layout: post
title: "Octopress Theme Customization"
date: 2012-06-16 12:01
comments: true
categories: Tech
---
[Octopress][05] 是基於 [Jekyll][10] 產生靜態頁面的部落格框架，專門為 hackers 設計的，因為他沒有友善的後台，而是用超宅的 Ruby 語言和最潮的版本控制系統 Git 作為操作的主要方法。其實架設 Octopress 並不是很困難，對於想要我這樣的程式入門者，甚至是一個了解 Ruby 語言和練習 Git 的好機會。

[05]: http://octopress.org/
[10]: https://github.com/mojombo/jekyll

這裡有我調整 Octopress 版面的寫筆記，記錄我用什麼方法，以及我為什麼這樣改的原因：

## Readability

我是 Instapaper、Readability 和 Pocket 這類應用的愛用者，很享受那種舒適的排版和文字。這次我把側邊欄整個移除掉，因為我想營造出閱讀最佳化的感受，而側邊欄亂七八糟的文字恰恰會破壞這份體驗。除此之外，為了保持網頁的效能和簡潔，我也決定不要安裝任何的社群按鈕。背景則使用的是 [SubtlePatterns][27] 的 Assault 主題，相當有質感。

## Typography

[Google Font][15] 真的是網頁開發者的好朋友，上頭有非常多的免費字型可以使用，只要透過 API 就可以取得喜歡的字型。找了好幾組配對，最後部落格標題字體和內文標題字體，分別使用我超愛的 [Patua One][30] 和 [Yanone Kaffeesatz][40]，因為超好看的，所以我決定以後每一篇文章標題都要用英文寫。

至於中文怎麼辦？內文我用中文來寫，原則上會依照每個使用者瀏覽器設定的襯線字型來顯示，例如同樣是 Chrome 瀏覽器，Mac 上是儷宋 Pro，Windows 則是新細明體，但這不是我想要的。我想要讓使用者在不同平台上都能夠看到相同的字體，目前我只找到 [justFont][20] 這個解決方案，而且成為付費會員後能夠使用非常棒的[信黑體][23]字型，只是這樣的開銷不小，以我這個沒有流量的個人網站來說，實在很不划算，還是暫且擱著吧。

## Retina…ready?

如果到 [About][24] 頁面你可以看到一些可愛的圖示嵌入在內文當中，那是使用 [Font Awesome][25] 這套圖示字型做到的，因為隨著 Apple 推出 Retina 這類變態螢幕的裝置賣得越來越好，影響到現在的網頁設計也要跟著適應，所以我網站裡面都會盡量避免直接套圖，目前……除了右上角那顆 RSS 按鈕，其他都是用 HTML / CSS 和字型來實現，包括漸層、圓角和陰影等等。

不過也不要被 Retina 過度嚇壞了，因為還是必須看網站的使用者是哪種屬性，例如我們都知道 IE6 很該死，但是還是有人在用，所以我們做網站的時候真的要為了他們，然後殺光開發者的腦細胞去搞 hack 嗎？未必，如果這批人少到可以忽略的時候，那就放他們去吧！別理他們了！Retina 螢幕的裝置目前就只有 iPhone 4S、iPad 第三代和新版 Macbook Pro，如果使用這些裝置的使用者，在訪問你的網站流量中比例低到你認為可以忽略，那就別理他們吧！

[15]: http://www.google.com/webfonts
[20]: http://www.justfont.com/
[23]: http://www.vmtype.com/index_c.html
[24]: ../../about
[25]: http://fortawesome.github.com/Font-Awesome/
[27]: http://subtlepatterns.com
[30]: http://www.google.com/webfonts/specimen/Patua+One
[40]: http://www.google.com/webfonts/specimen/Yanone+Kaffeesatz

****

以上大概就是我這次改佈景的一些記錄，其實沒有改很多，只是至少不會這麼容易第一眼就看出來是 Octopress！XD
