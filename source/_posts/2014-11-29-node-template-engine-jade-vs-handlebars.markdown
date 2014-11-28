---
layout: post
title: "Node 樣版引擎評估：Jade、Handlebars"
date: 2014-11-29 01:38
comments: true
categories: Programing
---

![](https://lh3.googleusercontent.com/-aa-cbFoMPDs/VHi1FfBlM-I/AAAAAAAAIJM/T6wEE0RlEtQ/w640-h201-no/Screen%2BShot%2B2014-11-29%2Bat%2B1.44.09%2BAM.png)

## 短文版

用 jade。

## 長文版

因為這個專案效能不會是評估重點，語法設計、擴充性、使用彈性會是比較在意的部分。

活在樣版引擎這個群雄割據的戰國時代實在太多選擇，從各方的評論和簡略地掃描文件上的語法特性，挑出以下幾個我比較喜歡的：

* [TJ](https://github.com/tj) 早期樣版引擎代表作 [ejs](https://github.com/tj/ejs/)
* 以及 TJ 後來的 [jade](https://github.com/jadejs/jade)
* 還有 [wycats](https://github.com/wycats)（[Yehuda Katz](http://yehudakatz.com/)）的 [handlebars](https://github.com/wycats/handlebars.js)

TJ 當初設計 [Express](http://expressjs.com/) 承襲 Ruby micro-framework [Sinatra](http://www.sinatrarb.com/) 的設計理念，ejs 好像是因應而生（？），從 ejs 語法上面可以看到非常多 Ruby 樣版引擎 ERb 的影子，對我來說是最「友善」的。不過 TJ 另創 jade 樣版引擎加上投奔 Go 世界以後，ejs 的活躍程度越來越低，從今年 5 月之後就再也沒有更新過了。

我向來都會先以「社群活躍度」來評量一個程式專案計畫可靠度，jade 貢獻者在 Github 上成立 [jadejs](https://github.com/jadejs) 組織來維護專案，長久來看蠻可靠的。而 handlebars 在 Github 上也廣受 Javascript 開發者歡迎，所以最後我就拿這兩套樣版引擎來比較，以下是值得提出來講的部分。

（再強調一次好了：語法設計、擴充性、使用彈性是我比較在意的項目）

<!-- more -->

### Layouts

寫過 Rails 後就很喜歡那種把不同 controller actions 的 view，跟 layouts 拆得乾乾淨淨的做法。這點 handlebars 可以做到，也是我最喜歡的一點，jade 必須在每一個 view 裡面寫上重複的 code，來表示外層的 layout：

``` jade
extends layout

block content
  h1= post.title
```

把內容的部分宣告成 block，然後套進 layout 裏去。這也不是大缺點啦，block 寫法可以讓 view 不同區塊使用上更有彈性，但這就很考驗 block 的拆分功力了，不然真的會很容易寫出大量讓人崩潰的縮進層級。

### Partials

版型上共用的部分一定會拆出來到同個檔案去維護，例如 header、footer、sidebar 之類的，handlebars 要做到這點繞得也太大圈：得先告訴 handlebars 的實例（instance）某個目錄放著我要的 partials 檔案，例如有個 `header.hbs` partial 放在 `/views/partials` 目錄下，然後要在 app 主要程式中寫：

``` js
var hbs = require('hbs');
hbs.registerPartial(__dirname, '/views/partials');
```

才能在 view 裡面帶入 header 這個 partial：`{% raw %}{{> header}}{% endraw %}`。這樣讓檔案的組織非常缺乏彈性，如果 partials 不是在同個地方而是多個目錄，這樣光是找到檔案就很直覺。

### 條件判斷

這個是壓死駱駝最後一根稻草，handlebars 沒有辦法用很簡單的條件判斷式，內建的 `#if` helper 根本不是拿來寫「條件判斷」的，當初掃描完文件後一定會很自然地寫下 `{% raw %}{{#if query === 'list'}}{% endraw %}`，這樣是不行的喔！

`#if` 頂多拿來判斷物件裡面有東西還是沒東西而已，如果真要寫個「條件判斷」一樣要拆去寫 helper，不管你的是多簡單的判斷。

### 結論

當初 wycats 寫 handlebars 時主要是為了解決前端樣版引擎的問題，所以可以發現很多 server-side 應用的場景 handlebars 都不適用。jade 是 TJ 寫給 Express 用的，讓我最不適應的地方是像 [HAML](http://haml.info/) 那樣「縮進崇拜主義」簡化的語法我實在不敢恭維，但顯然目前沒有比「不習慣」更好的理由來阻止我用它，因為 jade 其他面向提供的 solution 確實都能解決問題。

結論就是用 jade！
