---
layout: post
title: "SCSS 物件導向模組設計 (1)"
date: 2016-10-02 19:47
comments: true
categories: Programing
---

![](https://lh3.googleusercontent.com/mZCl_NgujdrKc-XhTqKsXA30gUb7dGQaQhe4Jjx8Blj4_97XWHoy5Zbfq6bk2GCu59jt0Gdu1Li8wCI3iCr9ejKu_wFJxzI4u6IrFl--RQ3KER-J0s4WXZ7f5VuQkH9Kc1TijUI6tHbGMdCt-r_fYO9wUI9ImbRlIhBTW4dgkg2YRDwIFbJjeMGWgQHBtjUb1dmsmjmKtcqWVPH5yfdnXxItyvLUCuA2zuN1dW90gljU6fdj5p1ivtoNQERUrJpXUhF0F_sDQcYsc2m9YzzOKvVnmvYDo0tUtmw1YzUts1PwrybI-G9c-7gKrc12Z_8PZl9dmF4GqOmi4c8ODPFFTxnZZGW5OYTGYah3LtiEQR8OzDY5HuOEKm2LUGPOLdBHRZVT-Xs6Ljy1cx6DLIAVPb1Q1SukfmDigXDyaLimn9-oEazRlugDF9Q2qStJaRzrsi2Ht6tnFZ8LK1XsTjgFpMh3mRnGQCKBewIjd7FiVnYGtT0f3XMhkSjv24UYW3gJAZuPLBjl4P8YQBW20ZLWQi33oCDxQKLr6MOLp-cx7YjOGSqfoYkAvcy3F08TyA2XAhV2PL_4g5gAqnETmUZOPmep9kHwy1gFL1eYJhTdLAcH982D-w=w1101-h755-no)

三年前曾經寫過「[劣以為的 OOCSS 和 SMACSS 以及其他 CSS 規範](http://blog.chh.tw/posts/oocss-smacss-and-css-guidelines/)」探討 OOCSS 模組化的方法論，時至今日對於模組化設計又有新的認識跟理解，大原則跟三年前的方法沒有出入，只是多做解釋跟補充。

OOCSS 物件導向是解釋設計原則，但是不限於語法的設計。

如果這個議題有興趣，歡迎繼續往下閱讀，如果有任何想法也歡迎留言與我討論！

<!-- more -->

這篇文章會解釋的內容，包括四個設計原則：

- [單一責任原則](#srp)
- [極少化原則](#lkp)
- [介面設計原則](#isp)
- [開放封閉原則](#ocp)

SCSS 模組化設計模式：


- [元件模式](#cp)
- [語義化模式](#sp)

----

<a name="srp"></a>

## 單一責任原則

一個 class 只負責一件事情。這個原則不是指 [universal.css](https://github.com/marmelab/universal.css) 這種設計方式，事實上這個專案是個反諷。單一責任原則是指職責獨立、目的明確的 class。

``` scss
.clearfix:before,
.clearfix:after {
  display: table;
  content: "";
}

.clearfix:after {
  clear: both;
}
```

<a name="lkp"></a>

## 極少化原則

對於層級的依賴應該盡可能減少。

多半的人剛開始寫 SCSS 都為它的巢狀語法感到著迷：

``` scss
.page {
  .content {
    .header {
      .inner {
        .group-list {
          .group-item {
            a {
              ...
            }
          }
        }
      }
    }
  }
}
```

這個寫法有改善的空間，因為：

- 不容易閱讀：當層級非常多的時候，哪段屬性屬於哪層 class 會看不清楚。
- 太倚賴 DOM 結構：一旦改變 DOM 層級結構，SCSS 也可能會需要做層級調整，層級越多耦合性越高，意味維護成本越高。

改善的方式便是在相同的命名空間下，試著將層級攤平。適當使用巢狀語法的範例：

``` scss
.nav {
  list-style-type: none;
  margin: 0;

  li {
    display: inline-block;
  }

  a {
    display: block;
    text-decoration: none;
    color: blue;
  }
}
```

即便 `<a>` 的 DOM 結構是屬於 `<li>` 的子元素，仍然不影響它們的作用：

``` html
<ul class="nav">
  <li><a href="...">...</a></li>
  <li><a href="...">...</a></li>
  <li><a href="...">...</a></li>
</ul>
```

有時候與 DOM 元素本身有絕對關聯的 class，可以在使用區塊註解加以說明：

``` scss
/* ul */.nav {
  ...
}
```

<a name="isp"></a>

## 介面設計原則

針對介面設計，而不是針對實踐方式設計。這個原則可以減少重複的程式碼，增加可維護性。

例如兩個有著相似樣式的區塊，往往會針對區塊實踐它們各別的樣式：

``` scss
.card-default {
  border: 1px solid hsla(0, 0%, 33%, 0.85);
  padding: 15px;
}

.card-primary {
  border: 1px solid hsla(0, 0%, 66%, 1);
  padding: 20px;
}
```

``` html
<div class="card-default">
  ...
</div>

<div class="card-primary">
  ...
</div>
```

應該先設計這個模組的介面（interface），再依照介面實踐它不同的樣式特性：

``` scss
// 先設計介面
.card {
  border: 1px solid transparent;
  padding: 10px;
}

// 再實踐不同的介面樣式
.card-default {
  border-color: hsla(0, 0%, 33%, 0.85);
  padding: 15px;
}

.card-primary {
  border-color: hsla(0, 0%, 66%, 1);
  padding: 20px;
}
```

``` html
<div class="card card-default">
  ...
</div>

<div class="card card-primary">
  ...
</div>
```

一旦先有了介面，後續也才能夠知道該怎麼對此模組進行擴充。

<a name="ocp"></a>

## 開放封閉原則

對修改封閉、對擴充開放。除非是根本性上地調整模組的樣式，才會需要去改動到介面層級的樣式。這裡引用上一則原則的範例：

``` scss
.card {
  border: 1px solid transparent;
  padding: 10px;
}

.card-default {
  border-color: hsla(0, 0%, 33%, 0.85);
  padding: 15px;
}
```

``` html
<div class="card card-default">
  ...
</div>
```

當某個頁面需要對此介面做特殊調整，可以使用開放封閉原則，對此介面進行擴充，而不會影響到其他頁面樣式：


``` scss
.card-users {
  border-color: hsla(0, 0%, 100%, 1);
}
```

``` html
<div class="card card-default card-users">
  ...
</div>
```

----

<a name="cp"></a>

## 元件模式

元件模式的哲學在於 class 本身與語義化無關，應該關注它們是否合理，不要刻意強調 class 名稱要符合語義，而要注重使用的合理性與未來性。例如：子元素區塊使用 `.list-item ` 上層元素使用 `.list-group` 作為子元素的集合，這與元素本身是否是個序列清單（`<ol>` ）無關。

在前面瞭解過四大設計原則之後，現在已經可以很輕易地設計出模組。先設計介面（interface），再實踐不同的模組樣式：

``` scss
.card {
  background-color: transparent;
}

.card-default {
  background-color: white;
}

.card-primary {
  background-color: red;
}
```

在 HTML 上的使用方式：

``` html
<div class="card card-default">...</div>
<div class="card card-primary">...</div>
```

元件模式好處在於開發前端版型，只需要專注在刻寫 HTML 上，快速調用各種模組 class 名稱、減少花時間替 class 命名的 context switch 成本。缺點亦即很容易過度膨脹 HTML 體積，例如使用熱門框架 [Bootstrap](http://getbootstrap.com/) 的人們應該對此都不陌生：


``` html
<div class="col-lg-9 col-md-9 col-sm-9 col-xs-12 col-lg-offset-3 col-md-offset-3 col-sm-offset-3">
  ...
</div>
```

<a name="sp"></a>

## 語義化模式

語義化強調元素本身所代表的意義，但並非漫無目的地設計 class；結合四種模組設計原則，加上 SCSS 語法的便利性，可以很容易地設計出容易維護及擴充的模組：

``` scss
card {
  @at-root {
    %#{&} {
      background-color: transparent;
    }

    %#{&}-default {
     background-color: white;
    }

    %#{&}-primary {
      background-color: red;
    }
  }
}

.content {
  @extend %card;
  @extend %card-default;
}

.sidebar {
  @extend %card;
  @extend %card-primary;
}
```

這會產生出以下的結果：

``` scss
.content,
.sidebar {
  background-color: transparent;
}

.content {
  background-color: white;
}

.sidebar {
  background-color: red;
}
```

因此可以清晰、明確地定義出區塊所代表的意義：

``` html
<div class="sidebar">...</div>
<div class="content">...</div>
```

語義化模式的優點在於減少 HTML 冗長的 class 名稱串聯，透過 SCSS 語法優勢，對不同的區塊套入相同的模組樣式。缺點在於切出每個區塊時沒有那麼容易替其命名，增加了中間「做決定」的思考成本。

兩個模式並非相互衝突，可以在適當的時機交互結合應用。
