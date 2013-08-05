---
layout: post
title: "劣以為的 OOCSS 和 SMACSS 以及其他 CSS 規範"
date: 2013-08-05 10:40
comments: true
categories: Programing
---
真心覺得寫出 CSS 並不難，但是要寫出可被維護的 CSS 比其他程式語言都還難。所幸已經有許多大師級的人物，提出許多設計模式和思維，藉由站在巨人的肩膀上可以讓事情事半功倍。這篇文章就來說說 OOCSS、SMACSS 和撰寫 CSS 時應該注意的規範。

（本文的例子用的是 SCSS 語法）

<!-- more -->

## OOCSS

OOCSS 不是什麼新技術，只是一種撰寫 CSS 的設計模式，或者可以說是一種「道德規範」，大致上我覺得重點只有兩個：

* [減少對 HTML 結構的依賴](#html-dependency)
* [增加 CSS class 重複性的使用](#reuse-classes)

<a name="html-dependency"></a>
### 減少對 HTML 結構的依賴

``` html
<nav class="nav--main">
  <ul>
    <li><a>.........</a></li>
    <li><a>.........</a></li>
    <li><a>.........</a></li>
  </ul>
</nav>
```

一般的導航欄寫法，結構應該會像上面的 HTML 範例一樣，如果要對那些 `<a>` 標籤定義樣式，CSS 的寫法可能寫成 `.nav--main ul li a {}`，這種寫法先不管效能上的問題，可以看出來過度地依賴元素標籤的結構，有可能之後 HTML 結構改變，這個 CSS 就必須跟著重構，造成維護上多餘的成本。

若從這個例子來考量，原則上 `<a>` 都一定會接在 `<li>` 標籤的後面，一個 `<li>` 只會有一個 `<a>`，通常不會獨立存在，那就可以寫成 `.nav--main a {}`，會是比較好的寫法，甚至是直接給 `<a>` 加上 class `nav--main_item`。後者是 OOCSS 所提倡的用法。

這樣的寫法，一來效能理論上比較好（我沒辦法驗證），二來層次比較單純。

<a name="reuse-classes"></a>
### 增加 CSS class 的重複使用

在 OOCSS 的觀念中，強調重複使用 class，而應該避免使用 id 作為 CSS 的選擇器。這種想法就是像 [OOP](https://en.wikipedia.org/wiki/Object-oriented_programming) 盡量抽離重複的程式碼，例如以下這個例子，這是兩種按鈕的 CSS 樣式屬性：

``` scss
.button {
  display: inline-block;
  padding: 6px 12px;
  color: hsla(0, 100%, 100%, 1);
  &.button-default { background: hsla(180, 1%, 28%, 1); }
  &.button-primary { background: hsla(208, 56%, 53%, 1); }
}
```

上面的 CSS 將兩種不同樣式的 button，抽離出重複的部份，並且定義在同個 class 上。因此，要使用這樣的樣式，HTML 的寫法可能長這個樣子：

``` html
<a class="button button-default">
<a class="button button-primary">
```

先用 `button` 宣告此為一個按鈕的樣式，再用 `button-default` 或 `button-primary` 作為按鈕底色的區別。這麼做可以維護成本變得比較低，例如：想要改網站上所有按鈕的大小，就只要修改 `.button` 的 `padding` 而已。

## SMACSS

我對 SMACSS 的理解還不是很深入，或許把 [Scalable and Modular Architecture for CSS](http://smacss.com/) 看完後會有更深一曾的理解。目前對 SMACSS 的概念僅限於它對 CSS 不同的業務邏輯所做的劃分方式：

但我認為原本的設計不是很妥當，因此我自己做了一些改良：

* [Base](#base)
* [Layout](#layout)
* [Module](#module)
* [Partial](#partial)
* [State](#state)
* [Theme](#theme)


<a name="base"></a>
### Base

Base 就是設定標籤元素的預設值，例如瀏覽器的 reset 可以寫在這裡，如果用的是 Compass，只要 `@include global-reset` 即可。這裡只會對標籤元素本身做設定，不會出現任何 class 或 id，但是可以有屬性選擇器或是偽類：

``` scss
html {}
input[type=text] {}
a:hover {}
```

<a name="layout"></a>
### Layout

Layout 是指整個網站的「大架構」的外觀，而非 `.button` 這種小元件的 class。網站通常會有一些主要的大區塊，可能是 header 或 footer，Layout 就是用來定義這些「大架構」的 CSS。如果有做 Responsive Web Design 或是用 Grid System，也是把規則寫在 Layout 這裡。

以下這是個範例：

``` scss
#header { margin: 30px 0; }
#articles-wrapper { ......; }
.sidebar {
  &.sidebar--right { ......; }
  &.sidebar-left { ......; }
}
```

通常只有一個選擇器，一個 id、或一個 class。

<a name="module"></a>
### Module

原本的 SMACSS 對 Module 的設計我覺得不是很好，所以我硬是將 Module 拆分出一個 [Partial](#partial)。

這裡的 Module 顧名思義，就是可以在其他地方被重複使用，如果要找更明確的例子，我想就像 Twitter Bootstrap 的 [Components](http://getbootstrap.com/2.3.2/components.html) 一樣，或者像前面 OOCSS 所舉例的 `.button` 這種會被重複使用的元件模組。

模組不需要用任何的 prefix，因為 Module 就是設計來可以重複應用在不同的 page 上。

### Partial
<a name="partial"></a>

Partial 跟 Latout 不同，也跟 Module 不同，他比 Layout 的範圍小，可能是 header 底下的某個子元素。他不像 Module，他是特定單一領域下特別的設定。

``` scss
.nav--main {
  a { ......; }
}
```

通常會將 Partial 的名稱加在子 class 作為 prefix，例如 `.nav--main` 底下的 `.nav--main_item`。至於為什麼要用這麼奇怪的命名方式？這等等在 [CSS 規範](#bem)部分會說明介紹。

<a name="state"></a>
### State

State 負責定義元素不同的狀態下，所呈現的樣式。但是並非指一個元素的 `:hover` 或 `:active` 下的狀態。舉例來說，一個導航欄分頁，目前所在頁面的分頁需要加上 `.active` 的屬性表示目前位置是在這個分頁，HTML 會長這樣：

``` html
<nav class="nav--main">
  <ul>
    <li><a>.........</a></li>
    <li class="active"><a>.........</a></li>
    <li><a>.........</a></li>
  </ul>
</nav>
```

因此可以替 `.nav--main` 增加 `.active` 這樣的子 class：

``` scss
.nav--main {
  // others…;
  .active {
    background: darken($background-color, 16%);
  }
}
```

有時候為了讓閱讀更貼近語義，會用比較友善的命名方式，以此段的範例來說，`.is-active` 就比 `.active` 來得好讀。

<a name="theme"></a>
### Theme

Theme 是畫面上所有「主視覺」的定義，例如 `border-color`、`background-image` 或是 `font-family` 等相關的 Typography 設定。為什麼說是「主視覺」？因為有些元件模組仍然是留在 Module 去定義，Theme 就像 Layout 一樣負責「大架構」上的視覺樣式。

## CSS 規範

這裡整理的是我覺得一定要知道的，其他還有很多規範可以轉到文末的參考資源連結，那篇文章有介紹更多的細節。

<a name="bem"></a>
### BEM

BEM 即 Block、Element、Modifier 的縮寫，這是一種 class 的命名技巧。如果整個 project 只有自己一個人做，那當然是不太可能出現 class 重複的問題，但是如果同時多個 F2E 一起寫同個部分的 CSS，就很容易出現共用 class 的問題，因此有了 BEM 這樣的命名技巧。

將 Block 區塊作為起始開頭，像前面 SMACSS 介紹的 Partial 就可以拿來作為 Block 的 prefix 名稱；Element 則是 Block 下的元素；Modifier 則是這個元素的屬性。

不同 Block 和 Element 用 `__` 兩個底線區隔開來，不同的 Modifier 則用 `--` 兩個 dash 區隔。至於 `-` 一個 dash 則表示這個 class 不依賴任何 Block 或 Element，是個獨立的存在，例如：`.page-container` 或 `.article-wrapper`。

這裡有個範例：

``` scss
.sidebar {
  .sidebar--left__section {
    .sidebar--left__section--header {}
    .sidebar--left__section--footer {}
  }
}
```

### Javascript Hook

透過 CSS class 來作為 Javascript 選取 DOM 節點的方式，就是 Javascript Hook。用 jQuery 可以常常看到這樣的寫法：`$('.nav--main a')`，可是當 CSS 跟 Javascript 攪在一起反而造成兩邊維護上的不便，當改了 CSS 時 Javascript 也要跟著改。

所以用 HTML 的屬性去選取 DOM 節點會更好，如果非要用 CSS 的 class 那也可以多寫一個 `js-` 的 prefix，以表示這個節點有被 Javascript 使用，例如：

``` html
<li class="nav--main__item  js-nav--main__item"><a>.........</a></li>
<li class="nav--main__item  js-nav--main__item"><a>.........</a></li>
<li class="nav--main__item  js-nav--main__item"><a>.........</a></li>
```

PS. HTML 裡兩個 class 之間用兩個空格，會比一個空格看起來好閱讀。

### 合理的選擇器

> class 無所謂是否語意化的問題；你應該關注它們是否合理，不要刻意強調 class 名稱要符合語意，而要注重使用的合理性與未來性。

有時候為了表示更明確，在使用 CSS 的選擇器時，要表示某的 class 是搭配某個標籤元素使用，會寫成這樣：


``` css
ol.breadcrumb{}
p.intro{}
ul.image-thumbs{}
```

但是上面這個寫法效能不是很好，同樣的目的但可以減少多餘的修飾，試試改用下面這種寫法，將標籤名稱用註解標示起來，維護上有相同的效果，但是瀏覽器處理的速度會比較快：

``` css
/*ol*/.breadcrumb{}
/*p*/.intro{}
/*ul*/.image-thumbs{}
```

## 參考資源

* [撰寫可管理、可維護的 CSS 高階技巧](https://github.com/doggy8088/CSS-Guidelines)