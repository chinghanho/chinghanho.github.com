---
layout: post
title: "終端機必備的多工良伴：tmux"
date: 2013-04-09 17:11
comments: true
categories: Programing
---
每次把終端機關掉後，先前的狀態都無法記錄，這個問題困擾我好久，最近才知道 tmux 這個工具，終於解決了這個問題。

從官網上的描述，[tmux](http://tmux.sourceforge.net/) 是個 terminal multiplexer，意思就是可以讓終端機同時跑多個程式，不用的時候可以把他們藏到背景去，需要的時候再叫出來，甚至 ssh 登入到遠端主機也不會斷掉，此外還可以在同個視窗下切割區塊，如下圖（[圖片來源](http://www.psteiner.com/2012/05/tmux-for-ruby-on-rails.html)）：

![tmux-demo](http://lh5.googleusercontent.com/-Z0XpVHmJ-ks/UWO-JgwbszI/AAAAAAAAF_M/QdewyOkH1xw/s690/rumble.png)

右邊開 Vim 寫程式，左上角跑 RSpec 測試，同時左下角開 Rails server 看 log，神奇的是這些狀態可以保存起來，不需要的時候可以藏到背景去，可是用 Sublime Text 2 就沒這麼理想，害我有種想改用 Vim 的衝動。

<!-- more -->

## 在 OS X 上使用 Homebrew 安裝 tmux

首先要安裝 [Homebrew](http://brew.sh/index_zh-tw.html)，沒有安裝過的話只要在終端機執行這一行指示即可：

    ruby -e "$(curl -fsSL https://raw.github.com/mxcl/homebrew/go)"

儘管剛剛才安裝 Homebrew 但是還是要先更新一下再安裝程式：

    brew update
    brew install tmux

## tmux 運作方式

從 SuperUser 看到的[答案](http://superuser.com/questions/398735/difference-between-tmux-and-shell-split-options-on-iterm2)，如果關閉終端機掛載在上面的 shells 也會跟著被刪除，結構就像是這樣：

    iterm2
      +---- shell
      +---- shell
      +---- shell

透過 tmux 仍然會保持所有 shells 繼續運作，即便把整個終端機都關閉，因此可以之後重新將他們重新掛載上去，這也是為什麼在 tmux 下用 ssh 登入遠端主機不會斷線的原因：

    iterm2
      +---- tmux
              +---- shell
              +---- shell
              +---- shell

## tmux 基本用法

直接在終端機執行 `tmux` 會開啟第一個 client，可以執行 `tmux ls` 查看有哪些 client，如果沒有任何 client 可以用，tmux 會自動建立一個。執行 tmux 後終端機底部會顯示一條狀態欄：

![tmux](http://lh5.googleusercontent.com/-eG1xoULfnn0/UWPNebkAeDI/AAAAAAAAF_c/m3RIih2HklI/s690/tmux.png)

tmux 預設的操作要加上 <kbd>Ctrl-b</kbd> 功能鍵；tmux 也有類似 Vim 的指令模式，快捷鍵 <kbd>Ctrl-b</kbd> 後按 `:` 可以執行指令。以下是一些基本、常用的功能：

* <kbd>Ctrl-b</kbd> + <kbd>c</kbd>：建立新的視窗；
* <kbd>Ctrl-b</kbd> + <kbd>d</kbd>：卸載目前的 client；
* <kbd>Ctrl-b</kbd> + <kbd>l</kbd>：與先前選擇的視窗間切換；
* <kbd>Ctrl-b</kbd> + <kbd>n</kbd>：移動到下個視窗；
* <kbd>Ctrl-b</kbd> + <kbd>p</kbd>：移動到上個視窗；
* <kbd>Ctrl-b</kbd> + <kbd>&</kbd>：刪除目前的視窗；
* <kbd>Ctrl-b</kbd> + <kbd>,</kbd>：重新命名目前的視窗；
* <kbd>Ctrl-b</kbd> + <kbd>%</kbd>：將目前的視窗分離到兩個區塊；
* <kbd>Ctrl-b</kbd> + <kbd>q</kbd>：顯示各分割區塊的號碼（用來切換到不同的區塊）
* <kbd>Ctrl-b</kbd> + <kbd>o</kbd>：切換到下個區塊；
* <kbd>Ctrl-b</kbd> + <kbd>?</kbd>：列出所有快捷鍵的說明；
* <kbd>Ctrl-b</kbd> + <kbd>w</kbd>：列出目前 clinet 的視窗，可以用數字鍵切換；

要將目前視窗切割多個區塊，快捷鍵如下：

* <kbd>Ctrl-b</kbd> + <kbd>%</kbd>：垂直分離視窗；
* <kbd>Ctrl-b</kbd> + <kbd>:split-window</kbd>：水平分離視窗；
* <kbd>Ctrl-b</kbd> + <kbd>o</kbd>：移往下一個區塊；
* <kbd>Ctrl-b</kbd> + <kbd>q</kbd>：顯示區塊的數字代號，當數字顯示時使用數字鍵移往該區塊；
* <kbd>Ctrl-b</kbd> + <kbd>{</kbd>：將目前的區塊移往左邊；
* <kbd>Ctrl-b</kbd> + <kbd>}</kbd>：將目前的區塊移往右邊；

切割面板區塊的快捷鍵好像有點難記，可以在根目錄下建立 _~/.tmux.conf_ 檔案，定義自己容易記憶的快捷鍵：

    unbind %
    bind | split-window -h
    bind – split-window -v

此外，推薦這兩個連結的文章，介紹非常詳細：

* [TMUX – The Terminal Multiplexer (Part 1)](http://blog.hawkhost.com/2010/06/28/tmux-the-terminal-multiplexer/)
* [TMUX – The Terminal Multiplexer (Part 2)](http://blog.hawkhost.com/2010/07/02/tmux-%E2%80%93-the-terminal-multiplexer-part-2/)

## 可以跟 iTerm2 整合

iTerm2 [wiki 頁面](https://code.google.com/p/iterm2/wiki/TmuxIntegration)上有教怎麼跟 tmux 整合，看了一下說要下載一些東西，然後還要編譯過才能用，可是我發現我直接輸入 `tmux -CC` 就可以用了，不清楚發生什麼事。

跟 iTerm2 整合的好處是可以用 iTerm2 的分頁和切割區塊功能取代 tmux 那些一堆的指令，可是我試用的結果發現很多不方便的地方，所以我還是用原本 tmux 切割區塊的方式。