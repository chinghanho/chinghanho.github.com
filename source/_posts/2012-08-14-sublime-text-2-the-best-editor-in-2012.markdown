---
layout: post
title: "Sublime Text 2: The Best Editor in 2012"
date: 2012-08-14 09:54
comments: true
categories: Tools
---
{% img center https://lh6.googleusercontent.com/-vvJS7em2Nds/UCnUAAIMynI/AAAAAAAAEKk/sjrjf4f31XQ/s700/IMAG0143.jpg "Sublime Text 2" "sublime-text-2" %}

* 2012.12.26 更新：停用 ChangeQuotes，因為 BracketHighlighter 已經有單雙引號轉換的功能。
* 2012.12.23 更新：停用 Alignment、Http Requester 和 Indent XML 套件，加入新使用的 Rspec、INI 套件。
* 2012.11.25 更新：開始使用 Rails Partial 套件。
* 2012.11.16 更新：ZenCoding 已經停止維護，更改專案名稱為 Emmet，並且加入新的功能。
* 2012.11.13 更新：更新最近已安裝套件的清單。
* 2012.10.08 更新：更新最近已安裝套件的清單。
* 2012.09.25 更新：今天 Nettuts 釋出一系列 [Sublime Text 2 教學影片](https://tutsplus.com/course/improve-workflow-in-sublime-text-2/)，推薦看這個學習。

****

以前使用 Windows 的時候是使用可愛的 Notepad++，雖然那時有嘗試過 Sublime Text 2 但沒太深入瞭解，直到九月份買了 Mac 之後必須尋找文字編輯器替代品，也聽聞 TextMate 這個 Mac 界的 coding 神器，可是這傢伙要賣 $50 美元實在太貴，而且很久沒有更新，相較下 Sublime Text 2 可以永久免費試用，開發社群也相當活躍，所以最後選擇了後者。

這幾天關於編輯器最大的消息就是 [TextMate 2 開源][textmate-2-open-source]了，慶幸自己當初還好沒有買，現在可以在 Github 上自由取得 source code，有需要的話可以上去 clone 一份下來自己編譯使用，我也在別處取得了編譯好的版本安裝來玩，稍微把玩了一下，可以確定的是這篇文章標題可以大膽的下了。

[textmate-2-open-source]: http://notes.whiteball.tw/blog/textmate-kai-fang-yuan-shi-ma/

以前有寫過一篇專文介紹 Sublime Text 2，可是在轉換部落格的時候沒順便帶過來，剛好最近使用也快滿一年，再來寫一次自己是怎麼使用這套 2012 年最紅的編輯器。

## Command Palette

這項設計非常的方便，只需要透過快捷鍵 `⌘ + ⇧ + P` 呼叫出控制面板，不需要記憶過多的快捷鍵，只要透過查詢的方式，在這裡可以下達各種指令，包括設定語法、使用程式碼片段、呼叫套件功能等等，都可以從這裡操作。很多套件的快捷鍵如果記不住也不用傷腦筋去做小抄，打開這個面板一切好解決。

## Package Control

幾乎所有好用的套件都會提交到 [Package Control][package-control]，這個就好像 iOS 的 App Store、Android 的 Google Play，統一安裝、更新、移除套件變得非常容易，也不需要另外開資料夾把補丁裝進去了{% fn_ref 1 %}。以下是我目前已經安裝的套件：

* ~~[Alignment](https://github.com/wbond/sublime_alignment)：自動對齊等號左右的程式碼；~~
* [BracketHighlighter](https://github.com/facelessuser/BracketHighlighter/)：程式碼括號上色，方便辨讀；
* ~~[ChangeQuotes](https://github.com/colinta/SublimeChangeQuotes)：自動轉換單、雙引號；~~
* [CoffeeScript](https://github.com/Xavura/CoffeeScript-Sublime-Plugin)：讓 Sublime Text 2 可以支援 CoffeeScript 格式以及辨識語法；
* [DetectSyntax](https://github.com/phillipkoebbe/DetectSyntax)：辨識 Guardfile 等檔案的語法；
* [Gist](https://github.com/condemil/Gist)：在 Sublime Text 2 裡管理 Gist 程式碼片段；
* [Gitignore](https://github.com/github/gitignore)：自動生成 Git 要忽略的項目；
* ~~[Http Requester](https://github.com/braindamageinc/SublimeHttpRequester)：在編輯器內進行 HTTP request 測試；~~
* ~~[Indent XML](https://github.com/alek-sys/sublimetext_indentxml)：將單行的 XML 自動整理縮進；~~
* [INI](https://github.com/clintberry/sublime-text-2-ini)：支援 INI 語法，例如 .gitconfig 等等；
* ~~[iTodo](https://github.com/chagel/itodo)：待辦事項套件，個人 GTD 管理；~~ （如果想要在 Sublime Text 2 上使用 GTD 可以參考最近很紅的 [PlainTasks](https://github.com/aziz/PlainTasks)）
* ~~[LiveReload](https://github.com/dz0ny/LiveReload-sublimetext2)：不用另開 guard，即時更新瀏覽器畫面；~~ （會拖爛啟動速度）
* ~~[KeymapManager](https://github.com/welefen/KeymapManager)：`⌃ + ⌥ + K` 快速查詢快捷鍵；~~
* ~~[MarkdownEditing](https://github.com/ttscoff/MarkdownEditing)：仿 [iA Writer](http://www.iawriter.com/) 介面的 Markdown 編輯模式；~~
* [Rails Partial](https://github.com/wesf90/rails-partial)：選擇要建立到 partial 的區塊，按一個快捷鍵就能完成；
* [Rspec](https://github.com/SublimeText/RSpec)：支援 RSpec 語法；
* ~~[RSpec (snippets and syntax)](https://github.com/rspec/rspec-tmbundle)：讓 Sublime Text 2 可以支援 RSpec 格式；~~
* [SCSS](https://github.com/kuroir/SCSS.tmbundle/tree/SublimeText2)：讓 Sublime Text 2 可以辨識 SCSS 格式；
* [SideBarEnhancements](https://github.com/titoBouzout/SideBarEnhancements/)：升級側邊欄功能等級到三攻三防；
* [SublimeAllAutocomplete](https://github.com/alienhard/SublimeAllAutocomplete)：自動完成已開啟視窗的單詞；
* [SublimeAutoSemiColon](https://github.com/LewisW/SublimeAutoSemiColon)：寫 JavaScript 要加上分號時很方便；
* [SublimeERB](https://github.com/eddorre/SublimeERB)：自動補全 ERb 的 `<%= %>` 這類的符號；
* [SublimeLinter](https://github.com/SublimeLinter/SublimeLinter)：自動偵錯，主要用來檢查我的 Ruby 語法；
* [SublimeTODO](https://github.com/robcowie/SublimeTODO)：專案開發註解用的 Todo 套件；
* ~~[ZenCoding](https://github.com/sublimator/ZenCoding)：加速 HTML、CSS 撰寫的戰鬥藥劑；~~ （已經改名為 [Emmet][emmet]）

[package-control]: http://wbond.net/sublime_packages/package_control
[emmet]: https://github.com/sergeche/emmet-sublime

## Goto Line, Symbol and Goto anything

使用 `⌃ + G` 輸入行數可以快速跳至該行， `⌘ + R` 可以快速跳至當前倒案下的所有類別或方法，比如說一個 class 中定義了非常多 method（`def...end`），用這個方法可以快速切換到該方法的位置。

如果你是把整個目錄都拉近編輯器裡，你也可以直接用 `⌘ + T` 或是 `⌘ + P` 開啟切換檔案的面板，這功能超級棒，切換不同目錄或是不同目錄下的檔案，也不必輸入完全對應的字元，只要差不多相對正確就可以。

最近我才曉得還有另外一招，打開切換檔案的面板，例如用這樣的語法 `controller@correct_user`，會在所有包含 controller 關鍵字的檔案裡去抓 `correct_user` 方法，這樣能減少一些查找的步驟。

要在所有檔案裡面查找東西，可以用 `⌘ + ⇧ + F`，路徑如果不填的話，預設就是專案裡的所有檔案。查出來的資料會另外跳一個 Find Result 分頁，用滑鼠雙擊兩下找到的結果可以跳至該檔案那個結果的位置。

## Multiple Seletion

快捷鍵是 `⌘ + D` 選擇輸入符號所在的單字，也可以連續按就可以一直往下選，中間要跳過字的可以用 `⌘ + K + D`。輸入符號要統一列在所選單字的右邊可以用 `⌘ + ⇧ + L`，如果要縮成一行就用 `⌘ + J`，真的超級方便。

## Switch Projects

將目前開啟的資料夾、檔案儲存成專案後（Save Project As...），能夠記錄你目前已經開啟哪些視窗、游標位置、偏好設定等等，快捷鍵 `⌘ + ⌃ + P` 可以迅速在不同的專案之間切換，這功能真的非常酷炫。

## Preference Setting

我的個人偏好設定備份在 [Gist][sublime-text-2-preferences-user-setting] 上，會定期去更新一下，為了避免篇幅跟 [Dropbot][dropbot-octopress-theme-inspired-by-tweetbot-for-mac] 那篇一樣可怕，我就不要嵌入了{% fn_ref 2 %}。我的設定主要都是以 Ruby 開發風格為主，例如程式碼縮進用的是兩格。

Theme 指的是 Sublime Text 2 用的佈景主題，我用的是 Soda Dark，黑客就是要黑色嘛！嘿嘿！Color Scheme 則是指程式碼配色方案，我從 [@chongzhi][chongzhi] 的 [scheme-sunburst-ex][scheme-sunburst-ex] 配色方案 fork 來的，針對自己寫 Ruby 上的需求，稍微做了一些些修改。

[sublime-text-2-preferences-user-setting]: https://gist.github.com/4143262
[dropbot-octopress-theme-inspired-by-tweetbot-for-mac]: http://127.0.0.1:4000/posts/dropbot-octopress-theme-inspired-by-tweetbot-for-mac/
[chongzhi]: https://github.com/chongzhi
[scheme-sunburst-ex]: https://github.com/chongzhi/scheme-sunburst-ex

## Useful Shortcuts

Sublime Text 2 有非常多好用的快捷鍵，練熟了對於 coding 真的非常有效率，上網 fork 了一份別人[整理好的表格][useful-shortcuts]，如果我覺得有用得快捷鍵也會更新上去。

[useful-shortcuts]: https://gist.github.com/3346205

{% footnotes %}
{% fn 有的還是會沒有提交到 Package Control，不過安裝的方法很簡單，只要把下載的檔案拖進 Packages 資料夾就行了。 %}
{% fn 我的設備環境是 Mac 平台，不太確定相同配置能不能在其他平台上適用。 %}
{% endfootnotes %}
