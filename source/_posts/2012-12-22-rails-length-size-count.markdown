---
layout: post
title: "Rails 的 length、size、count 差別"
date: 2012-12-22 12:26
comments: true
categories: Programing
---
## Ruby

在 Ruby 中對於陣列而言 `length` 和 `size` 是一樣的，`count` 也有相同的效果，可是用途更靈活，能用來設定條選篩選內容：

``` ruby
[1, 2, 3].count { |x| x > 2 }
```

此外，效能上 count 會比較差一點：

``` ruby
require "benchmark"

CYCLES = 10_000_000
array = []

10.times { array << rand }

Benchmark.bmbm do |x|
  x.report(%[count])  { CYCLES.times { array.count }}
  x.report(%[size])   { CYCLES.times { array.size }}
  x.report(%[length]) { CYCLES.times { array.length }}
end

# Rehearsal ------------------------------------------
# count    2.790000   0.000000   2.790000 (  2.796578)
# size     1.860000   0.000000   1.860000 (  1.860494)
# length   1.900000   0.010000   1.910000 (  1.908549)
# --------------------------------- total: 6.560000sec

#              user     system      total        real
# count    2.820000   0.000000   2.820000 (  2.827195)
# size     1.890000   0.000000   1.890000 (  1.893677)
# length   1.940000   0.000000   1.940000 (  1.950517)
```

## Rails

在 Rails 的 ActiveRecord 中有不一樣的操作方式，使用 `length` 會從資料庫撈出資料，在記憶體裡面去計數：

```
> user.topics.length
Topic Load (0.1ms)  SELECT "topics".* FROM "topics" WHERE "topics"."user_id" = 1
```

看到了嗎？把 user 底下的 topics 全部拉出來，只是為了去數他，瘋了！其實 SQL 已經提供很好的語法可以有效地計算欄位裡的數量，在 Rails 中只要用 `count` 就可以呼叫：

```
> user.topics.count
(0.3ms)  SELECT COUNT(*) FROM "topics" WHERE "topics"."user_id" = 1
```

可是這有個問題，這個指令每下一次就會向資料庫操作一次，如果已經讀過的話，沒必要老在那拉扯資料庫，所以這時候可以用 `size` 方法，這會先檢查有沒有讀過，讀過的話就用 `length` 返回長度，如果沒讀過就用 `count`。

順便參考一下[原始碼][active-record-size]這樣寫的：

[active-record-size]: https://github.com/rails/rails/blob/4657dba60eebc0d7cea11ffd18aa70d7a3d00e45/activerecord/lib/active_record/relation.rb#L201

``` ruby
# Returns size of the records.
def size
  loaded? ? @records.length : count
end
```

## One more thing…

如果在 model 中加上一個 topics_count 欄位，counter_cache 會讓 `size` 自動去參考這個欄位的值，這樣也能有效提昇效能。

參考 RailsCasts 的 [#23 Counter Cache Column][counter-cache-column]。

[counter-cache-column]: http://railscasts.com/episodes/23-counter-cache-column