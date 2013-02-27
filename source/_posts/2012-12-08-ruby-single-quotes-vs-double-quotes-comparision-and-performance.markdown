---
layout: post
title: "Ruby 單引號、雙引號差別與效能測試"
date: 2012-12-08 14:32
comments: true
categories: Programing
---
Ruby 的單引號（`'`）跟雙引號（`"`）基本上只有在轉義字元（escaped character）和字串插值（string interpolation）有差異，單引號沒辦法解讀字串插值，例如：

``` ruby
var = "hello world"

puts 'hello world\nnew line'
# => hello world\nnew line

puts "hello world\nnew line"
# => hello world
# => new line

puts '#{var}'
puts "#{var}"
# => #{var}
# => hello world
```

如果兩個引號之間要插一個相同的引號，就要用不同風格的引號來包，否則就會噴出 SyntaxError 的錯誤，除非加一個 `\` 來轉義，例如：

``` ruby
puts 'This is David\'s pen'
puts "This is David's pen"
# => This is David's pen
# => This is David's pen
```

可是這樣很麻煩，所以這時就會需要用到 `%q` 和 `%Q` 這個方法，前者等於單引號的用法，後者等於雙引號的用法（也可以直接寫成 `%{}`），例如：

``` ruby
puts %q{Hello world!\n This is David's pen}
# => Hello world!\nThis is David's pen

puts %Q{Hello world!\n This is David's pen}
# => Hello world!
# => This is David's pen
```

單引號跟雙引號之間的差別大致上就這樣，那效能呢？我寫了一個腳本來測試兩者的速度差異，結果如下：

``` ruby
require "benchmark"

CYCLES = 10_000_000

Benchmark.bmbm do |x|
  x.report(%['hello world']) { CYCLES.times { a = 'hello world' }}
  x.report(%["hello world"]) { CYCLES.times { a = "hello world" }}

  x.report(%[%q{hello world}]) { CYCLES.times { a = %q{hello world} }}
  x.report(%[%Q{hello world}]) { CYCLES.times { a = %Q{hello world} }}

  x.report(%['hello world' + n.to_s]) { CYCLES.times { a = 'hello world' + 1.to_s }}
  x.report(%["hello world \#{n}"])    { CYCLES.times { a = "hello world #{1}" }}

  x.report(%[%q{hello world } + n.to_s]) { CYCLES.times { a = %q{hello world } + 1.to_s }}
  x.report(%[%Q{hello world \#{n}}])     { CYCLES.times { a = %Q{hello world #{1}} }}
end

# ruby-1.9.3-p327
# Rehearsal -------------------------------------------------------------
# 'hello world'               4.100000   0.010000   4.110000 (  4.112255)
# "hello world"               4.060000   0.000000   4.060000 (  4.069385)
# %q{hello world}             4.090000   0.010000   4.100000 (  4.099501)
# %Q{hello world}             4.090000   0.000000   4.090000 (  4.104130)
# 'hello world' + n.to_s     10.680000   0.020000  10.700000 ( 10.705543)
# "hello world #{n}"          9.870000   0.010000   9.880000 (  9.895310)
# %q{hello world } + n.to_s  10.710000   0.020000  10.730000 ( 10.744212)
# %Q{hello world #{n}}        9.810000   0.010000   9.820000 (  9.832551)
# --------------------------------------------------- total: 57.490000sec

#                                 user     system      total        real
# 'hello world'               4.010000   0.010000   4.020000 (  4.018492)
# "hello world"               4.010000   0.010000   4.020000 (  4.017989)
# %q{hello world}             4.010000   0.000000   4.010000 (  4.021088)
# %Q{hello world}             4.010000   0.010000   4.020000 (  4.019685)
# 'hello world' + n.to_s     10.700000   0.010000  10.710000 ( 10.730320)
# "hello world #{n}"          9.860000   0.020000   9.880000 (  9.890101)
# %q{hello world } + n.to_s  10.610000   0.010000  10.620000 ( 10.634330)
# %Q{hello world #{n}}        9.860000   0.020000   9.880000 (  9.884285)
```

得出兩個結果：

* 字串插值的寫法會比用 `+` 拼接的效能好（而且可讀性也比較高）；
* 雙引號比單引號效能好；

一般情況，我的習慣都是使用雙引號，個人覺得看起來也比較好看。:p