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

在數位時代一個字母的空間容器從實體金屬塊轉移到虛擬的電腦程式上，電腦程式會建構一個虛構的空間容器被稱為 [EM square](http://designwithfontforge.com/zh-CN/The_EM_Square.html)、EM size 或 UPM（Unit per EM），術語源自活版印刷技術的歷史，而數位字型也將以此作為容器基礎。

字型（font）指的是文字造型，又稱為字體，常見的字型例如標楷體、新細明體、微軟正黑體、蘋方體、Times New Roman、Helvetica 等，基於相似風格字形組成的家族（family）便稱為字型，也是 CSS 的 `font-family` 所設定的名稱。

字形（Glyph）指的是字元的形狀，由程式演算法或是文字工程師、設計師手動調整的形狀本身。一個字型的所有字形都有相同的 EM square 尺寸，差別在於字形的大小不同。

![EM square](https://lh3.googleusercontent.com/g1lcXEFfL9nixF0Y2Y_iGmWdB_0IDR7x8lc1Qyvsmm9HaADlF3NuwPsYRAgYZGIDfiTkE70Bzplb8wTlXMlWomr5oW3pYh82SeoK4Up0JDWVTTfBFb1uGlhuqeOSgxVxY3EQnZv7z70PqQedGmiKk3bslIllLWK_qkm0UAkz_-bH8Tu5UofMGV79RDcMCLsPcTHmz4aIv8kgwSV1v9QOsBnwzHA5VxGkUhYaI43KF7wrI8671AiRWqdoVC50fEnAilnAZfdEOuJYemEPrK08WILJhE4TsU9vT-HyyNjVbyofFkZjAuoTz38Ar8Ui-fhF_2oyqIQ_eT8IyzcAqWJdID_LQpavb4l7t_oFXvtcUMUCmBgbr0vwwBghubTM3hCP1X-L4uDYLA3ZRcZRxUilyfvx77eY5Ql7ShBQbV_WODZxsEGlTvXJVKG_R63uS01mWg0JEXEmD-hWc75yDofawRB_9wXU9iAbqR6PrFlyUUXvrRBGqDj5tqlnz1IXaOO_yviAqlWaWLjOTEP5sayRQmzlM2XsS0IOalze94YySZN31VJ34Yisf5kBjhdDQVUz5XgENmTyBCjK_CKihb-BiuT1UceXbU_sYPDNrcvMKnV1BJOIEFd2b6pWUQZ0ZtKaFc5_RyiBRZH70dyaCw0VL4_Dm8l9hwMIsaXvK89Yxj0=s755-no)

在技術上 EM square 可以看作是個有限的二維空間座標系統（x,y），寬高可能是 2048x2048（單位 FUnit），[OpenType 官方規格建議大小是 16 到 16384 之間](https://www.microsoft.com/typography/otspec/head.htm)，但如果主流軟體無法支援便失去了意義，例如 [InDesign、Illustrator 特定版本無法正確處理 EM square 超過 5000 的字型](http://typedrawers.com/discussion/comment/863/#Comment_863)，所以 EM square 的尺寸需要取決於軟體的相容性。目前比較常見的尺寸通常是 1000 或是 2048，例如 [Apple 主要字型都是以 2048 FUnit 去設計](https://developer.apple.com/fonts/TrueType-Reference-Manual/RM01/Chap1.html#master)的。

字形基本上都會設計在 EM square 的範圍內，但也能超出這個範圍，視字型製造商的偏好及決定。

![Character extending outside of the em square.](https://lh3.googleusercontent.com/87tTVmRKfgB9GM7NZWF6ip9nHW-NKYGy8gNU9f8K3aMZXCkg0f16JGgT4-_0tht9i5WpLWOHJYi3NKflZiRjHg5ve1V9lKnUVTGiubZwDXkMuizGc2jP-hozP4zLsLPQlVM_Jmfs65TrS_exYL-BmWRc_h2g9YwxAZkB08zoX3CmEiTfVsehGl31MPuB0C3JtpObW19usVSG98AMDSrYQIt_SxYu58sBQEpfTKpJprFjND5IP7um5IKWuqJixDHjr3ZwF4k4y9b3PiCr3KV7OrpzF51Gl7_HvNfZPzCKt-8vCEgIRODN26N5NjEv6Gq7wuKDDItU30Ukj1RUOibGYlv6grb39g2EkiPwQsnYx9R0FEBvOHP_LrZMfzPH8-Yhqka99lIAbQT2-29593zDY3CqKTB3pFADrqM9zJSUNz3zTU_qeBZppdVZ3IflBaDdD1uKDBlVSe9tu6HQqoxFgNpXBmQmuuX6uI7s5CNpWDtocaon9VfyhWPxzRsKYlABBP5L8Ag3TC7GOwFvKGjwK1lse5WIuK1t8HgC9MniNHW1iIenF0H33kvPruTispc31EV8lpz3w4uQNJFmFzMWVB-KmKXqOCdlTu5oJ1iqxVhsNB8bV8X1mVDzKu3w00-oQvVexxWi5FUd_ng1hs7oDY2VnOXW6nSyeCmrxOq0YPA=s799-no)

當指定 CSS `font-size` 16px 瀏覽器便會將字型 EM square 的大小映射到螢幕 16px 的大小上，我會在下一篇文章介紹這部分的原理。

## 字形解構

觀察英文字母的書法規則，可以歸納出一些模式，例如每個字母基本上都坐落在同一條水平線上，這條水平線稱為基準線（baseline），相對於其他字型或字形決定如何對齊，不過這是針對英文（正確來說是拉丁文）字型設計，中文字型由於都是方形等寬等高的，因此不需要基準線的設計。

此外，基線到字形最高的頂部稱為升部（Ascent）；基線到字形最底的底部稱為降部（Descent）。在數位字型中，這項規格制定在 OpenType、TrueType 的 [OS/2 表格](https://www.microsoft.com/typography/otspec/os2.htm)中 `sTypoAscender` 及 `sTypoDescender` 的值，或是 [HHEA 表格](https://developer.apple.com/fonts/TrueType-Reference-Manual/RM06/Chap6hhea.html)中的 `Ascent` 及 `Descent`。OS/2 是給 Windows 的規格、HHEA 則是 Apple 的規格。

![](https://lh3.googleusercontent.com/qC070S-SC4Z330u2-mHASXfdcuMaSWZeJLLkMrdDcjZ-k5U5mPDwHJJPOSpT_mCwVtofFUjevRD3BI9mqIXEfJp39UFkk9cCRD-KcciLuOGQalz2xBk8i3skrqOCgv92jiJsqAnK99jXGV_Qjr0vosSo2_tgZmZXdMrGSRemvVDsw9Gn2PYg8VNhEjxnZNAZkoeCUPe9hPg7ecTpWWpJKhWgUY-UnQTRtV3KIGKEIufRnhjwMQrE4rkO2l3v6l5BX3_Gultxc-hNkJ9-0ScKhGMz5gyPHKHBLiykv0MATtp04_292JxcO8079Qoy65mMWIE8XPjvYdcfJR5GhxiKk2seNof0f4gAb-jjhDl3u7SvA8XI8-VAHm2wPS2qgMCP_w8jxM__VhbJDnOcBqBLBpbNz3P_o3AjXPUy6S5fVGbbT7DSfhPDSn2RAEUIi0K68ZnfaeX0VRWqPY3vQ_jq_wDsasqZYw2NedOFpR5VVvgK3MSfKN40jw1qn7_IaXnClwSgZzNZJqZ7UwYthjUO-QxZlTGQyCILBnnye3b5aEAJmIlDXD08GWMj9Hu778uap1AJcX1mseJb4L_fwlv3NSQzutLmJMeC2zFrCXtvO3Om970ucDOllxt3kP2-n08cy9igomlsEgUa1G46u3HyVvITc51D22ZbbkpfH0gLfMw=w914-h738-no)

## 行高

印刷排版中的行高英文是 leading，指的是兩行文字中間的距離或空白，但是 CSS 的行高（line height）略有不同的定義，因為它不是只是單純地加在行與行的中間，而是先計算出 leading：

> leading = `line-height` - (ascent + descent)

然後 CSS 將其中一半的 leading 加到文字的上方，一半加到下方。舉例來說，指定某區塊字級大小 16px，行高設定 24px，則 (24 - 16) = 4，CSS 會在 16px 的文字上面及下面同時加上 4px 的空白，最後整體文字區塊（inline box）才會得到 24px 的高度。

W3 規定 CSS `line-height` [預設的值是 normal](https://www.w3.org/TR/2011/PR-CSS2-20110412/visudet.html#propdef-line-height)，並且建議這個值可以介於 1.0 ~ 1.2 之間。為了測試實際 line-height 到底是多少，我準備了一個測試網頁，在 Chrome 跟 Firefox 兩種不同的 render engine 測試不同英文字跟中文字體，得到以下 4 個測試結果：

* 不同瀏覽器 normal 不同；
* 同個瀏覽器 `font-size` 不同 normal 實際值也不同；
* 同個瀏覽器 `font-size` 相同 font-family 不同 normal 實際值也不同；
* 雖然 W3 建議 normal 應該介於 1.0 ~ 1.2 但實際上隨著 `font-size` 與 `font-family` 變化，可能會大於 1.2；

![line-height-compare](https://lh3.googleusercontent.com/2YoOk7Z7nao8okS7x8r351ZcSxuz9OHWziJEkoYIIEfsZlm6pcN-2vEGGL_Meh001IBPfSdw_JApeQ2ONLzlIdjs-1jkWS9Nz1REdtiY6pbY4SUKZFYEcPTDLk5WuZFoo_UiGfgSVBUJ6F-mm549PqzgxBp279iQM0B5GrrAA62MKUAfHxVJmRLQohvPVh2PKA9I5tvHdQUO0Y0N1J_Up3CCSlPaSB9h4Zvoj4wZ6M51GeJi39OeqZUu_XL-YAPOmQSTXvDAgBwaiMlAXUevxBCkoARGDOKGXB8Z3mW_OyPHwv4Af37-MYgzO4SyMQWoHoocYLBxtPIV4QzRRQTUaU9lroOE2q5blXCwQHSTVWY5tIej_tjB_P4Gy6AV4RSyKJDnUXom5s5vCMGCma2CU851laqZV4nYiLRVQXv61ngtRDBLn5xujjTLM66AG4nwfr7yeCXrbCLpO8aA0__33F5lHlZITIaq5zvbDYfe2wME_j3AWmunQ2zir6pBJ9W8MTDttq0LG4d6MVW9P4_joHIcvO-670lGG6rLMiY14AJtTqrx6aminh9E4u_e2Nve6ayuPHipD5GxFtbRYHEJLFk81kgpNtdtQcGAuT79HxNn0TaHKg=w1071-h799-no)

不同瀏覽器預設 `line-height` 不同，要解決這個問題也容易，只要在 `<html>`、`<body>` 指定固定的值即可，例如 [normalize.css](https://necolas.github.io/normalize.css/) 指定了 `line-height` 為 1.15，[reset.css](http://meyerweb.com/eric/tools/css/reset/) 設定 `line-height` 為 1。
