---
layout: post
title: "幾個加強 Shell 生產力的用法"
date: 2012-10-18 09:42
comments: true
categories: Programing
---
PS. 以下環境為 OS X，我不確定其他平台是否可用相同的方式。

## 從指令的歷史紀錄自動補全

剛看到「[9 Enhancements to Shell and Vim Productivity][enhancements-to-shell-and-vim-productivity]」這篇文章，學到一個新招就是 `⌃ + r` 可以用搜尋的方式，自動找出歷史記錄裡的指令，例如我蠻虛榮的喜歡用 `awk` 統計自己輸入過的指令次數{% fn_ref 1 %}，以前都是用複製貼上的方式，現在透過 `⌃ + r` 只要輸入 `awk` 馬上就找到了。

[enhancements-to-shell-and-vim-productivity]: http://danielmiessler.com/blog/enhancements-to-shell-and-vim-productivity

## 跳字移動

另外，最近終於找出如何在 [iTerm 2][iterm-2] 底下跳字移動了，所謂跳字移動就是在單字之間移動 cursor，而不是一個字母慢慢走。在 OS X 下，一般文字編輯器都是壓著 `⌥ + ←` 和 `⌥ + →`，可是 iTerm 2 下會觸發到其他的快捷鍵，所以要來修改一下。

方法就是打開偏好設定面板，在 Profile > Key 標籤頁下修改 `⌥ + ←` 和 `⌥ + →` 這兩個快捷鍵設定，分別改成 Action 為 Send Escape Sequence、Esc + b 以及 Action 為 Send Escape Sequence、Esc + f，這樣一來就跟平常慣用的方式一樣了。

[iterm-2]: http://www.iterm2.com/

~~目前我還在找尋該怎麼在 iTerm 2 底下用 `⌥ + ⌫` 來跳字刪除單字，如果找到方法會再補上來。~~

2012.10.19 更新：`⌥ + ⌫` 的方法跟上面差不多，快捷鍵設好後 Action 為 Send Escape Sequence、Esc + 007f，這裡的 007f 要用 Unicode 16 進位輸入。

## 善用 alias 縮寫

這有優點也有缺點，優點是可以少打很多字，缺點是換了別台電腦會突然發現自己不會下指令了。XD

用法是在主機根目錄下的 .bash_profile（原本如果沒有這個檔案就自己手動建立一個）裡加上 `alias` 指令，例如要改變目錄位置回到上一層，一般用法是這樣：

    cd ..
    cd ../..
    
在 .bash_profile 裡加上：

    alias ..="cd .."
    alias ...="cd ../.."
    
之後只要用 `..` 就可回到上一層目錄，用 `…` 回到上上一層目錄，可以參考看看我備份在 [Github][github] 上的設定。不過 alias 不要用得太兇，其實只要設定幾個常用到的就好，弄太多到時候一方面可能記不住，有時還可能跟其他指令衝突到，需要自己衡量一下。

[github]: https://github.com/chinghanho/.dotfiles/blob/master/.aliases

{% footnotes %}
{% fn 這條指令我放在 <a href="https://gist.github.com/3280141" alt="gist" target="_blank">Gist</a> 上，有興趣可以玩玩看，別忘了把結果回覆在這篇文章的留言上哦。XD %}
{% endfootnotes %}