---
layout: post
title: "關閉 OS X 雞肋的 Dashboard"
date: 2012-11-17 06:36
comments: true
categories: Tools
---
OS X 不知道在哪一個版本加進 Dashboard 這個功能，對我來說這東西一點用也沒有，如果把 widgets 挪到桌面上可能還方便一點，可是放到 Dashboard 上真的超級不方便。

不過就算把 widgets 弄到桌面上，我還是不會用到這些工具，所以我決定把它整個給移除掉，因為開在背景這東西也是會消耗資源的。關閉的方法很簡單，在終端機中輸入：

    defaults write com.apple.dashboard mcx-disabled -boolean YES
    killall Dock

第一條指令後面的 YES 換成 NO 的話，就是復原這個動作，不過記得，都要下 `killall Dock` 這個指令重新啟動 Dock 才會生效。

![](http://lh5.googleusercontent.com/-sPmI3eSBgYU/UKbA4bQtJOI/AAAAAAAAFXs/LAGQ9ySlNVI/s690/%25E8%259E%25A2%25E5%25B9%2595%25E5%25BF%25AB%25E7%2585%25A7%25202012-11-17%2520%25E4%25B8%258A%25E5%258D%25886.37.49.png)

現在滑到 Mission Control 也不會出現 Dashboard 了，整個心情都變好，頭腦也變聰明，考試都考一百分。XD