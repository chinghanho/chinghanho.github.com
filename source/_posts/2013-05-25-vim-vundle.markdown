---
layout: post
title: "更好用的 Vim 外掛管理工具：Vundle"
date: 2013-05-25 16:38
comments: true
categories: Programing
---
![vim-vundle](https://lh4.googleusercontent.com/-_0FBYCCvjPU/UaB5LuYVrWI/AAAAAAAAGXY/ewSOLLQ8_xU/s690/vim-vundle.png)

先前曾經寫過怎麼使用 [Pathogen](/posts/vim-plugins-manager-pathogen/) 管理 Vim 的外掛，Git 的 submodule 用久了開始覺得很不方便，有時候只是要測試一下新玩具，用完就刪掉，可是[前後要操作的指令](/posts/git-submodule/)至少超過四個，實在有夠麻煩。

正打算來寫個 script 自動化過程，但這種東西怎麼可能沒人寫過，於是上網找了一下，被我發現 [Vundle](https://github.com/gmarik/vundle) 這個管理工具，看完 README 後馬上就把 Pathogen 刪掉了，然後寫了這篇文章。XD

<!-- more -->

## Vundle 的優點

* 可以保持 git repo 的簡潔
* 讓安裝/更新/刪除外掛更方便

需要參考的話，可以看我放在 [Github 上的 _.vimrc_](https://github.com/chinghanho/.dotfiles/blob/4c53d90d3e1efffb3fdc1ebc44bdac1781154b19/.vimrc#L1-L39) 範例。

### 保持 git repo 的簡潔

透過 Vundle 管理外掛我不再需要讓 git 追蹤 _.vim/bundle/_ 目錄，甚至連 Vundle 本身也不用追蹤，只要在 _.vimrc_ 裡這樣寫：

``` vim
let iCanHazVundle=1
let vundle_readme=expand('~/.vim/bundle/vundle/README.md')
if !filereadable(vundle_readme)
  echo "Installing Vundle.."
  echo ""
  silent !mkdir -p ~/.vim/bundle
  silent !git clone https://github.com/gmarik/vundle ~/.vim/bundle/vundle
  let iCanHazVundle=0
endif
```

這樣一來，啟動 Vim 的時候就會自動去檢查 Vundle 安裝了沒，因此可以在 _.gitignore_ 裡把 _bundle_ 目錄直接忽略掉，不用再拖著一坨外掛跟著跑了。XD

### 安裝/更新/刪除外掛更方便

安裝、更新外掛是同一條指令，在 _.vimrc_ 裡加上外掛的 Github 的作者名和 repo 名稱，像這樣：

``` vim
Bundle 'Lokaltog/vim-easymotion'
Bundle 'Lokaltog/vim-powerline'
Bundle 'airblade/vim-gitgutter'
Bundle 'Townk/vim-autoclose'
Bundle 'kien/ctrlp.vim'
```

然後下指令 `:BundleInstall`，Vundle 會去搜尋對應的 repo，如果已經下載過便會檢查有沒有更新。要移除也很簡單，只要把該外掛那行刪掉或是註解掉，重新下一次指令就行了。

Vim 的外掛腳本基本上都可以在 [vim-scripts.org](http://vim-scripts.org/vim/scripts.html) 搜尋到，如果沒有 host 在 Github 上也可以直接輸入該外掛的名稱，例如這樣：

``` vim
Bundle 'L9'
Bundle 'FuzzyFinder'
```

若是該外掛放在別的 git 主機上，也可以丟 git URL，Vundle 會很聰明地判斷該去哪裡抓：

``` vim
Bundle 'git://git.wincent.com/command-t.git'
```
