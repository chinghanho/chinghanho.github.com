---
layout: post
title: "CSS Font (1) - EM square"
date: 2016-12-17 12:59
comments: true
categories: Programing
---

![2048](https://lh3.googleusercontent.com/_vIG5mKzyW0lcp8INazZP3wb3d5tdN2fkEBLHzy-DJMsqzoski-1WcsUA4T-JlFYHgWqRce8Zk0ErcVQn2gR7UvaeN-MJAlFlWGh2n_sJC71dQZ_bFmXpLsOc1an7dH9u6vt44ES-Ljs6x-RbXMz_ObYyZ9vRgH18eTfVFipTJxjxPPgeynvgkWIzz6W2J6aPE6irkS0I8QXUcNMM0WNGW-b5utOd5AQl0qrp0Ti7YHiHArmd-WciTrzdi-M3AngJVw2EPt7WoBPl4DQE-NbIZHEXjp5CEoq-D6fyeefjCK6eWn-wRyqfAbVHID_R_xtajVEUz2ApIi4RxF-urECPudmJFmvy6mJTH1o5exK5-EY9sofFXxiApntimCfoRc1V8J7iyhIAO3er_JrSOAPe76DsQeC2LcHVUIE9zpos4SMgOXj4RsMjsd0U-jOJHeVmOFiMJJKV-BkHGEdQp9EV5L5nFJOoxX-Hd40mgPRwLJa1GflN055qBd0Fh44MxPTkncgBi8mhqaUNpJobO6qWRbXCga1gDRoJDUQL-Df9i5-agXegiwDpzMdW4tmu3QjFQ-e-DPLoeFmT4pYrHsYiBGCuS8pTD0rx5fG6HVaMFMy7W4dKQ=w483-h346-no)

（圖片取自「[Digitizing Letterform Designs](https://developer.apple.com/fonts/TrueType-Reference-Manual/RM01/Chap1.html)」，版權為原網站所有。）

傳統金屬活版印刷把每個字母刻在一個金屬方塊上，方塊不一定是正方形，寬度可能不同但是高度相同，這樣才能整齊地排列字母。由於英文中「M」這個字母比例接近正方形幾乎是最大的一個，因此金屬塊的寬度被稱為「em（M）」。

在數位時代一個字母的空間容器從實體金屬塊轉移到虛構的網格，這個網格空間被稱為 [EM square](http://designwithfontforge.com/zh-CN/The_EM_Square.html)、EM size 或 UPM（Unit per EM），術語源自活版印刷技術的歷史。

這個空間容器可以看作是個正方形網格，「標準化」的網格寬高尺寸為 2048x2048，每個網格格點表示為一個 FUnit；不過在技術上 EM square 應該說是個二維空間座標系統（x,y）而不是網格。

<!-- more -->

字型（font）指的是文字造型，又稱為字體，常見的字型例如標楷體、新細明體、微軟正黑體、蘋方體、Times New Roman、Helvetica 等。字形（Glyph）指的是字元的形狀，由程式演算法或是文字工程師、設計師手動調整的形狀本身；一個字型的所有字形都有相同的 EM square 尺寸，差別只在於字形的大小不同。

之所以說「標準化」定義 EM square 尺寸是 2048x2048，是因為這取決於軟體的相容性，事實上可以設定更大的值，[OpenType 官方規格建議大小是 16 到 16384 之間](https://www.microsoft.com/typography/otspec/head.htm)，但如果主流軟體無法支援便失去了意義，例如 [InDesign、Illustrator 特定版本無法正確處理 EM square 超過 5000 的字型](http://typedrawers.com/discussion/comment/863/#Comment_863)。目前比較常見的尺寸通常是 1000 或是 2048，例如 [Apple 主要字型都是以 2048 FUnit 去設計](https://developer.apple.com/fonts/TrueType-Reference-Manual/RM01/Chap1.html#master)的。
