---
layout: post
title: "使用 Zepto + Velocity 無縫取代 jQuery 更輕巧"
date: 2015-10-01 00:26
comments: true
categories: Programing
---

![zeptojs](https://lh3.googleusercontent.com/fKcLfP4rJK5syiKv-cfmDYlbdC0_dV8sAdyQfYM3DxZW76knVkgxFsMC4sWsBpaN_27emoOHF504CNpmSDn0rnlDG2Eu1vw6RNowi18owUxdnBYaotT3hz2BIWnrQioS-tfQfOU4iDNAss3pzDKuNOPmSLWjNVP-zQrZJhZOK1Xf78TU9GdCSKnIApESyUZ777gjz7LC5PKw9Yk_NcNr8GCrKRsd7cCQciMf_sSmmZ1sqWKRcIAVWdt-B7_5fV9nW4vNfBYkFvJCI-BaIC_KmR-KPiVMUHi7y4hMzW1DmT4uzAJwfKIk3tvjWW6pEe345hE3R0nxqq-y0apP64DcndcDHFq16S1KqW465nsZRwvg6412LAjzp1cVM_XEimA9NekxAlTC1u89PNLyALrNtaaNRsAsj02D43lSFKu0gEKGMdAl23BhXkBtL-khwLYgoKWCbRkvZ7eznqnONx7zyGxxbNNU48Rn5NLDhw74Bxr0Xy72I1ROoGjm7E22V5-5BSl3CbsBAgczJ-fxCgxHzYWsmJpdqAIA6DVFmD5A8Vk=w690)

Zepto 是個跟 jQuery API 相容的套件，但是非常輕巧。

jQuery 1.11.2 不壓縮 278KB，壓縮後是 94.8KB。儘管 jQuery 2.0 比 jQuery 1.11 減少了 12% 的體積大小，仍然是個 200KB 級的套件。而 Zepto 不壓縮 56KB，壓縮後大約是 24.5KB，比 jQuery 平均小了 74%！

Zepto 極其輕巧的秘訣在於它預設只有 jQuery 最關鍵的幾個核心功能，並且用友善的模組化設計，保持每個功能能夠輕易地被擴充。

<!-- more -->

## 擴充 animate 模組

jQuery 的 `animate` 效能不是很好，上次在製作公司官網作品牆，讓圖片載完進行 `fadeIn` 的效果，造成 FPS 低到明顯感受到 lag。

通常需要用到大量的動態視覺效果的首選是 GreenSock 所提供的 TweenMax（344KB）。如果只是需要用來取代 jQuery 的 `animate` 操作 DOM 的動態效果，那麼能夠跟 jQuery 無縫整合的 Velocity（215KB）有更好的條件。Velocity 優點是效能高，體積比較小，也能夠相容 jQuery API。

Zepto 因為模組化設計的關係，預設是不包括 `animate` 這個方法的，因此便可以改用 Velocity 來擴充。

外部網站參考：

* [Zepto 的專案網站](http://zeptojs.com/)
* [Velocity 的專案網站](http://julian.com/research/velocity/)
* [TweenMax 的專案網站](https://greensock.com/tweenmax)


## 瀏覽器相容性

jQuery 2.0 開始就直接放棄 IE 9 以下的瀏覽器，Zepto 更狠直接跳到 IE 10+；而 Velocity 則是不支援 IE 8 以下，所以能不能使用 Zepto + Velocity 這個組合要先觀察一下產品的目標使用群。

如果打算支援 IE 8，還是去用 jQuery 1.11.3 吧！
