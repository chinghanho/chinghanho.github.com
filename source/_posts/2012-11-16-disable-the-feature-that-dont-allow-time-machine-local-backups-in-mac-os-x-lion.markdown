---
layout: post
title: "關閉 Time Machine 本機快照備份節省硬碟空間"
date: 2012-11-16 06:49
comments: true
categories: Tools
---
![os-x-disk-usage](http://lh4.googleusercontent.com/-DdQoYkv3voo/UKVzqUZRQEI/AAAAAAAAFWw/XN5s9URR5OI/s690/%25E8%259E%25A2%25E5%25B9%2595%25E5%25BF%25AB%25E7%2585%25A7%25202012-11-16%2520%25E4%25B8%258A%25E5%258D%25886.58.10.png)

我的 Macbook Air 就一個硬碟一個磁區，所以硬碟快照備份在本機一點意義也沒有，因為一旦硬碟掛了，那連備份也一起跟著掛。而且總容量只有 128GB，備份的檔案就吃掉 7GB 了，所以我打算把這個本地備份的功能給關掉。

此外，備份的意思就是要備份在不同的地方，備份在本機也是多此一舉。我通常會把一些常用但非「極機密」的資料，另外備份在 [Dropbox][dropbox] 上；也會定期（通常是十天做一次）外接硬碟，用 Time Machine 備份。

所以本機的硬碟快照沒有保留的理由，刪掉的方法很容易，從終端機下指令把這個功能關閉，馬上就會多 7GB 的空間可以用。不過也可能會很危險，使用以下指令前先確保資料都已經另外備份到別的硬碟上。

關閉本地快照這個功能：

    sudo tmutil disablelocal

重新啟用本地快照功能：

    sudo tmutil enablelocal

[dropbox]: https://www.dropbox.com/