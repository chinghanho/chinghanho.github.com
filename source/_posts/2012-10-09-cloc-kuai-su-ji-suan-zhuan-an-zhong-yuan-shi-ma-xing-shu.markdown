---
layout: post
title: "CLOC：快速計算專案中原始碼行數"
date: 2012-10-09 07:37
comments: true
categories: Tools
---
昨天在看新一期 [RailsCasts][exploring-rubygems] 的時候，見到 [Ryan Bates][rbates] 用了一個有趣的小工具叫做 [CLOC][cloc]，如果用的是 OS X 可以透過 [Homebrew][homebrew] 來安裝：

[exploring-rubygems]: http://railscasts.com/episodes/384-exploring-rubygems
[rbates]: http://twitter.com/rbates
[cloc]: http://cloc.sourceforge.net/
[homebrew]: http://mxcl.github.com/homebrew/

```
brew update
brew install cloc
```

儘管 Rails 有內建的指令 `rake stats` 可以查詢寫過多少程式碼，但是提供的資訊沒有很清楚；CLOC 不只可以詳細的計算寫了幾行程式碼，還可以清楚地辨識你用什麼語言來寫，每個語言個寫了多少行、多少註解、多少空白：

```
$ cloc rails-v3.2.8.tar.gz 
    2286 text files.
    2214 unique files.                                          
     533 files ignored.

http://cloc.sourceforge.net v 1.56  T=11.0 s (160.0 files/s, 19893.3 lines/s)
-------------------------------------------------------------------------------
Language                     files          blank        comment           code
-------------------------------------------------------------------------------
Ruby                          1548          31995          31994         145742
CSS                             37            126            367           4093
YAML                           116            221            336           1867
Javascript                      40            189            514            999
HTML                            17             30              3            298
SQL                              1              6              0             43
CoffeeScript                     1              0              3              0
-------------------------------------------------------------------------------
SUM:                          1760          32567          33217         153042
-------------------------------------------------------------------------------
```

如果說想要知道 Rails 3.2.7 和 Rails 3.2.8 之間程式碼的行數差異，可以到 Github 直接下載整個專案，連解壓縮都不用就可以直接進行比對，計算結果會詳細列出有多少相同、多少被修改、多少是新增的、多少被移除掉的程式碼：

```
$ cloc --diff rails-v3.2.7.tar.gz rails-v3.2.8.tar.gz 
    2286 text files.
    2286 text files.s
    1066 files ignored.                                         

http://cloc.sourceforge.net v 1.56  T=50.0 s (0.0 files/s, 0.0 lines/s)
-------------------------------------------------------------------------------
Language                     files          blank        comment           code
-------------------------------------------------------------------------------
HTML
 same                           17              0              3            298
 modified                        0              0              0              0
 added                           0              0              0              0
 removed                         0              0              0              0
SQL
 same                            1              0              0             43
 modified                        0              0              0              0
 added                           0              0              0              0
 removed                         0              0              0              0
Javascript
 same                           40              0            514            999
 modified                        0              0              0              0
 added                           0              0              0              0
 removed                         0              0              0              0
CoffeeScript
 same                            1              0              3              0
 modified                        0              0              0              0
 added                           0              0              0              0
 removed                         0              0              0              0
CSS
 same                           37              0            367           4093
 modified                        0              0              0              0
 added                           0              0              0              0
 removed                         0              0              0              0
YAML
 same                          116              0            336           1867
 modified                        0              0              0              0
 added                           0              0              0              0
 removed                         0              0              0              0
Ruby
 same                         1501              0          31943         145562
 modified                       47              0              7            124
 added                           0              6             44             56
 removed                         0             43             36            246
-------------------------------------------------------------------------------
SUM:
 same                         1713              0          33166         152862
 modified                       47              0              7            124
 added                           0              6             44             56
 removed                         0             43             36            246
-------------------------------------------------------------------------------
```

更多的玩法可以參考官方文件：[CLOC Options][cloc-options]

[cloc-options]: http://cloc.sourceforge.net/#Options