---
layout: post
title: "CSS Font (1) - 字型基本"
date: 2016-12-17 12:59
comments: true
categories: Programing
---

![2048](https://lh3.googleusercontent.com/_vIG5mKzyW0lcp8INazZP3wb3d5tdN2fkEBLHzy-DJMsqzoski-1WcsUA4T-JlFYHgWqRce8Zk0ErcVQn2gR7UvaeN-MJAlFlWGh2n_sJC71dQZ_bFmXpLsOc1an7dH9u6vt44ES-Ljs6x-RbXMz_ObYyZ9vRgH18eTfVFipTJxjxPPgeynvgkWIzz6W2J6aPE6irkS0I8QXUcNMM0WNGW-b5utOd5AQl0qrp0Ti7YHiHArmd-WciTrzdi-M3AngJVw2EPt7WoBPl4DQE-NbIZHEXjp5CEoq-D6fyeefjCK6eWn-wRyqfAbVHID_R_xtajVEUz2ApIi4RxF-urECPudmJFmvy6mJTH1o5exK5-EY9sofFXxiApntimCfoRc1V8J7iyhIAO3er_JrSOAPe76DsQeC2LcHVUIE9zpos4SMgOXj4RsMjsd0U-jOJHeVmOFiMJJKV-BkHGEdQp9EV5L5nFJOoxX-Hd40mgPRwLJa1GflN055qBd0Fh44MxPTkncgBi8mhqaUNpJobO6qWRbXCga1gDRoJDUQL-Df9i5-agXegiwDpzMdW4tmu3QjFQ-e-DPLoeFmT4pYrHsYiBGCuS8pTD0rx5fG6HVaMFMy7W4dKQ=w483-h346-no)

（圖片取自「[Digitizing Letterform Designs](https://developer.apple.com/fonts/TrueType-Reference-Manual/RM01/Chap1.html)」，版權為原網站所有。）

瞭解電腦字型如何被創造，能夠進一步理解文字如何在螢幕上顯示。本系列文章目的在於研究電腦字型在螢幕上顯示的原理，以便理解 CSS 有關字型的各項屬性背後的設計目的，以及要解決的問題。本文適合對象為從事網頁設計的專業人員，不論是網頁設計師、前端工程師，或是全端工程師。

<!-- more -->

## EM square

傳統金屬活版印刷把每個字母刻在一個金屬方塊上，方塊不一定是正方形，寬度可能不同但是高度相同，這樣才能整齊地排列字母。由於英文中「M」這個字母比例接近正方形幾乎是最大的一個，因此金屬塊的寬度被稱為「em（M）」。

在數位時代一個字母的空間容器從實體金屬塊轉移到虛擬的電腦程式上，電腦程式會建構一個虛構的空間容器被稱為 [EM square](http://designwithfontforge.com/zh-CN/The_EM_Square.html)、EM size 或 UPM（Unit per EM），術語源自活版印刷技術的歷史。

字型（font）指的是文字造型，又稱為字體，常見的字型例如標楷體、新細明體、微軟正黑體、蘋方體、Times New Roman、Helvetica 等。字形（Glyph）指的是字元的形狀，由程式演算法或是文字工程師、設計師手動調整的形狀本身；一個字型的所有字形都有相同的 EM square 尺寸，差別在於字形的大小不同。

在技術上 EM square 可以看作是個二維空間座標系統（x,y），有個預設的寬高尺寸是 2048x2048，單位以 FUnit 表示。字形基本上都被設計在這個尺寸的範圍內，但也能超出這個範圍。EM square 事實上可以設定更大的值，[OpenType 官方規格建議大小是 16 到 16384 之間](https://www.microsoft.com/typography/otspec/head.htm)，但如果主流軟體無法支援便失去了意義，例如 [InDesign、Illustrator 特定版本無法正確處理 EM square 超過 5000 的字型](http://typedrawers.com/discussion/comment/863/#Comment_863)。

所以 EM square 的尺寸需要取決於軟體的相容性，目前比較常見的尺寸通常是 1000 或是 2048，例如 [Apple 主要字型都是以 2048 FUnit 去設計](https://developer.apple.com/fonts/TrueType-Reference-Manual/RM01/Chap1.html#master)的。

## 字形解構

將千萬個字形拆解可以找出一些相同的模式，瞭解分解部分的異同之處，可以幫助網頁設計師理解不同字型的差異之處，進而配對選出適合的字型組合到網頁上。

技術上比較重要的幾個字形解構部分是：

* Baseline
* Descender
* Ascender
