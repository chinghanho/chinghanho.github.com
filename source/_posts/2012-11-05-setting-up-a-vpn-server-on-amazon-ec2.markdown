---
layout: post
title: "三秒鐘在 Amazon EC2 上架好 VPN-server"
date: 2012-11-05 08:52
comments: true
categories: Programing
---
原本老爸用[自由門][free]還可以翻出來到 Facebook 上貼些吃吃喝喝的照片，可是最近 GFW 又在搞怪，自由門也不太有效，所以老爸就問我能不能幫他架個 VPN 網路，剛好最近在練習 [Rails deploy][rails-deploy] 玩到 Amazon AWS 雲端服務，所以想說乾脆開個 instance 來架看看。

上網查了一下資料發現已經有人把整個流程寫成執行檔，只要先用 `wget` 把檔案抓到本機裡執行就搞定了，步驟超級簡單。首先有幾個前置作業：

* 開一台 Amazon AMI 32 位元的 instance
* Security Groups 防火牆設定要打開 port 1723 給 VPN 連線用（當然別忘了 SSH 用的 port 22）

然後用 `wget` 把執行檔抓到本機執行：

``` bash
wget https://raw.github.com/gist/999866/4fa28f39890fa4244ebeb82ea4e097e75756b582/pptpd.sh
sudo sh pptpd.sh
```

安裝完成後，會輸出 VPN 帳號跟密碼在最後一行，把它複製起來收好，或是抄起來，接下來就可以到電腦上設定 VPN 了。如果要增加用户，就编辑 /etc/ppp/chap-secrets 這個檔案，自己添加用戶名稱和密碼即可。

請注意 Free Tier 上下傳每個月限制是 15GB 免費，超過就要付錢了。15 GB 很多嗎？像我每天上網的流量可能都是超過 7、8 GB，當然是因為我會連到一些多媒體網站上去，如果用不到 VPN 來連記得把它關掉，有沒有回到撥接時代的感覺？XD

參考資料：

* [Amazon EC2上一键安装配置PPTP VPN服务](http://passtest.net/2010/11/amazon-aws-ec2-vpn-vps-2/)
* [Mac技巧之苹果电脑Mac OS X系统下设置VPN翻墙上真正互联网的教程](http://www.mac52ipod.cn/post/set-vpn-on-apple-mac-os-x.php)

[free]: http://us.dongtaiwang.com/?lang=cn
[rails-deploy]: http://blog.whiteball.tw/posts/deply-rails-app-on-ec2/