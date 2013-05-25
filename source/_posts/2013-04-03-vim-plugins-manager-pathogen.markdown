---
layout: post
title: "Vim 補丁管理器：Pathogen"
date: 2013-04-03 09:07
comments: true
categories: Programing
---
![mvim-screenshot](https://lh5.googleusercontent.com/-8oZ4apnadkQ/UVuFq3nS-NI/AAAAAAAAF9A/oJi0DRBBXYk/s690/mvim-screenshot.png)

2013.04.12 更新：今天把 Command-T 移除掉換用 [ctrlp](https://github.com/kien/ctrlp.vim)，優點是用不用 Ruby 去編譯，用的是 Vimscript 去寫，所以效能跟問題也會比較少。
2013.05.25 更新：今天起改用 Vundle 來管理，可以參考我的新文章：「[更好用的 Vim 外掛管理工具：Vundle](/posts/vim-vundle/)」

## 安裝 Pathogen

管理 Vim 補丁我用的是 [Pathogen](https://github.com/tpope/vim-pathogen) 這套，可以讓所有補丁統一放在 _~/.vim/bundle_ 下，每個補丁就是一個獨立的資料夾，想要移除就直接砍掉即可。安裝方法是將 _pathogen.vim_ 腳本放在 _~/.vim/autoload_ 目錄下，可以用以下指令完成：

```
mkdir -p ~/.vim/autoload ~/.vim/bundle; \
curl -Sso ~/.vim/autoload/pathogen.vim \
    https://raw.github.com/tpope/vim-pathogen/master/autoload/pathogen.vim
```

然後將這行加進 _.vimrc_ 裡，啟動 Vim 時便會自動讀取 _~/.vim/bundle_ 下的補丁檔案：

    " auto load all plugins in vim bundle
    execute pathogen#infect()

## 安裝補丁

之後只要將下載的補丁資料夾拖進 _~/.vim/bundle_ 裡面即可，不過我更偏愛使用 [Git](http://git-scm.com/) 來管理。假設要安裝 [Command-T](https://github.com/wincent/Command-T) 補丁，可以用 Git 的 submodule 指令完成：

```
cd ~/.vim
mkdir ~/.vim/bundle // if you have not yet created this directory
git submodule add https://github.com/wincent/Command-T.git bundle/command-t
git add .
git commit -m "install Command-T plugin as a submodule"
```

<!-- git submodule update --init -->

## 更新補丁

使用 Git 安裝補丁的好處是可以自己動手 hack，也可以保持更新。對單一補丁更新只要變更所在目錄到該補丁的位置，然後 `git pull` 去拉 upstream 的最新程式碼：

```
cd ~/.vim/bundle/command-t
git pull origin master
```

要一次更新所有已安裝的補丁，則可以用 Git 的 `foreach` 指令來更新每個補丁的最新版本：

    git submodule foreach git pull origin master


## 後記

平常被 Sublime Text 的 Package Control 寵慣了，來用 Vim 感到非常的麻煩和不習慣，而且 Command-T 安裝後還要用 Ruby 跟 C 去編譯才能用，這到底是什麼鬼編輯器啦！XD
