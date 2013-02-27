---
layout: post
title: "Opera 產品全面改用 WebKit 引擎"
date: 2013-02-14 08:40
comments: true
categories: Tech
---
![installed-browsers](http://lh3.googleusercontent.com/-81Na2LPt3oY/URwrpztPvcI/AAAAAAAAF0I/nW6cpHKy0xQ/s690/installed-browsers.png)

大約在一個月前就有看到 [Opera 要改用 WebKit rendering 引擎][opera-ice-new-webkit-browser]的消息，所以昨天看到 [Opera 開發團隊發佈的新聞][300-million-users-and-move-to-webkit]其實沒有太驚訝，反而是看到[鄉民們激動的表現][opera-moves-to-webkit-on-hacker-news]讓我有點意外。

要說有什麼感想其實沒有，因為 Opera 放棄 Presto 跟採用 WebKit 引擎說實在跟我沒有太大的關係，對於網站開發有沒有影響我也不知道，因為在我練功的路上目前都沒有碰到太多跟瀏覽器前端有關的部份，要說直接的影響大概就是寫 CSS 可以少個 prefix 吧！可是我寫 CSS 都是用 [Compass][compass] 這個武裝神器，本來就沒有在寫 prefix 啊。XD

我最喜歡的瀏覽器是 Google Chrome，可是有時會因為不同的需求或是開發需要，我電腦裡同時安裝了各家瀏覽器，包括 Safari、Firefox 和 Opera 等等。裝 Firefox 是因為要用到某些特別的 add-ons；而 Opera 則是 UI 用得不是很習慣所以不喜歡，裝起來只是純粹開發測試用。現在 Opera 改用 WebKit 後突然發現好像已經可以移除它了齁！:p

跟我一樣的人應該也不少，Opera 改用 WebKit 應該多多少少會影響到市佔率，可是我想其實應該不會減少太多，因為「一般使用者」本來就沒有在乎 Opera 用的是哪一套 rendering 引擎，重點還是在 UI 的設計跟整體效能讓使用者滿不滿意。我對 Opera 沒有什麼偏見，真的只是純粹用不慣而已。

Opera 放棄 Presto 後市場上就只剩下 WebKit、Gecko 和 Trident 三個主要的 rendering 引擎，也許以後會常見到「最佳瀏覽效果請使用 WebKit-base 瀏覽器」這樣的標語出現在網頁的尾部，也可能讓 WebKit 推向成為下一個「IE 世代」，誰知道呢？即便到真的有那麼一天，我相信以現在 web 的發展程度，一定會出現一個替代品讓堅持開放信仰的人們能夠有個地方去。

[opera-ice-new-webkit-browser]: http://www.pocket-lint.com/news/49375/opera-ice-new-webkit-browser
[300-million-users-and-move-to-webkit]: http://my.opera.com/ODIN/blog/300-million-users-and-move-to-webkit
[opera-moves-to-webkit-on-hacker-news]: http://news.ycombinator.com/item?id=5211953
[compass]: https://github.com/chriseppstein/compass