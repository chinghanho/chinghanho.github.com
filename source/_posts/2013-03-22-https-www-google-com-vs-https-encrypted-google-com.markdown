---
layout: post
title: "Google 搜尋加密有兩種"
date: 2013-03-22 13:53
comments: true
categories: Tech
keywords: "https, encrypted, not provided, google"
---
剛剛看 [Business Insider][1] 提到了 Google 的 [encrypted.google.com](https://encrypted.google.com/) 這個網址，記得在 Google 還沒有對已登入用戶全面預設用 [HTTPS][2] 連線以前，*encrypted.google.com* 是專門用來使用 HTTPS 的網址，也就是當連線到 *https://www.google.com* 時會被自動跳轉到 *https://encrypted.google.com*。

既然現在 Google 對於已登入的用戶已經預設用 HTTPS 連線，那這個東西還留著幹嘛？所以[問一下 Google 找答案](http://security.stackexchange.com/questions/32367/what-is-the-difference-between-https-google-com-and-https-encrypted-google-c)，原來 *encrypted.google.com* 另有其他特異功能，兩者之間的差異，主要在於點擊廣告與搜尋結果時，處理 [HTTP 參照位址][3]的方式不同。

<!-- more -->

### 點擊一個廣告

* *https://www.google.com*：Google 將會把你帶到一個 HTTP 的重新導向頁面，在那裡他們會把你的搜尋字串塞進參照資訊裡去。

* *https://encrypted.google.com*：如果這個廣告主用的是 HTTP，Google 不會讓他們知道你的查詢字串是什麼。而如果廣告主用的是 HTTPS，他們將會如常地收集到你的參照資訊（包括查詢字串）。

### 點擊一般的搜尋結果

* *https://google.com*：除果網站使用 HTTP 連線，Google 將會把你帶到 HTTP 的重新導向頁面，但是不會把你的搜尋字串給塞進參照資訊裡面。他們只會告訴網站你是從 Google 來的。如果你的網站使用 HTTPS 連線，便可以正常地搜集到參照資訊。

* *https://encrypted.google.com*：如果你點擊的網站使用 HTTP 連線，Google 既不會告訴它查詢字串是什麼，也不會告訴它你是哪裡來的。如果該網站使用 HTTPS 連線，它則會正常地收到參照資訊。

所以我的 blog 從搜尋引擎來的訪客中，用關鍵字去查有 74% 都是「not provided」大概就是這個原因。如果這篇答案的結果是正確的，採用 HTTPS 連線加密的網站便可以正確地搜集到帶有查詢字串的參照資料，那網站採用 HTTPS 將會變成最基本的條件。

[1]: http://www.businessinsider.com/google-products-youve-never-heard-of-2013-3#encryptedgooglecom-is-a-more-secure-way-to-search-for-things-encrypted-uses-secure-sockets-layer-ssl-which-is-the-same-security-that-banks-use-online-11
[2]: http://en.wikipedia.org/wiki/HTTP_Secure
[3]: https://zh.wikipedia.org/wiki/HTTP%E5%8F%82%E7%85%A7%E4%BD%8D%E5%9D%80