---
layout: post
title: "Ruby 的 Flat Scope 技巧"
date: 2013-01-16 15:30
comments: true
categories: Programing
---
2013.01.17 更新：為了簡化說明，本篇文章省略了類別變數（class variables）。

Ruby 的變數分為全域變數（global variables）、實例變數（instance variables）跟本地變數（local variables），變數的生命週期會隨著 scope 的不同而有所不同，而這個 scope 就是程式碼執行的環境。

在 Ruby 中決定 scope 範圍的分別是 `module`、`class` 和 `def`，也就是所謂的 Scope Gates，當 code 執行時每經過這三個「Gates」，某些變數可能就會隨著 scope 出現或是消失。

## Flat Scope

以下是簡單的例子，我用 `v1`、`v2` 和 `v3` 分別表示不同 scope 的本地變數：

``` ruby
v1 = "This is v1 outside of class C."

class C
  puts v1 # => NameError
  v2 = "This is v2 inside of class C."

  def call_method
    v3 = "This is v3 inside of call_method of class C."
    puts v1 # => NameError
    puts v2 # => NameError
    puts v3 # => "This is v3 inside of call_method of class C."
  end

  puts v2 # => "This is v2 inside of class C."
end

puts v1 # => "This is v1 outside of class C."

C.new.call_method # => NameError
```

`v1` 這個本地變數進不了 `C` class 的 scope，但是可以用 block 的方式傳給 `Class.new()`，這樣就不會通過「Gates」也就不會建立 scope，本地變數變得可以共用：

``` ruby
v1 = "This is v1 outside of class C."

C = Class.new do
  puts v1 # => "This is v1 outside of class C."
end
```

在 `C` class 裡 `v2` 這個本地變數也同樣進不了 `call_method` 這個 scope，因此可以用 `define_method` 來建立實例方法，避開使用 `def` 時的 Scope Gates，就能順利將本地變數傳進去：

``` ruby
class C
  v2 = "This is v2 inside of class C."

  define_method(:call_method) { puts v2 }
end

C.new.call_method # => "This is v2 inside of class C."
```

如果把這兩個方法串起來用，就會變成這樣，同個本地變數共用到底：

``` ruby
v1 = "This is v1 outside of class C"

C = Class.new do
  puts v1 # => "This is v1 outside of class C"

  define_method(:call_method) do
    puts v1 # => "This is v1 outside of class C"
  end
end
```

## 為什麼不用全域變數？

全域變數的確可以把所有的 scope 全部打平，或者是說它根本無視 scope 的存在。雖然聽起來好像是個不錯的解決方案，但這會造成維護上的困難，因為會不曉得到底程式的哪一個部分改變了全域變數，所以通常會盡量避免使用它。

## 為什麼不用實例變數？

實例變數可以讓不同的實例方法共用相同的變數，這個方法經常可以看到，應用相當廣泛，例如這個範例程式經常見到：

``` ruby
class C
  def initialize
    @shared = "Hello world!"
  end

  def call_method_a
    puts @shared
  end

  def call_method_b
    puts @shared
  end
end

c = C.new
c.call_method_a # => Hello world!
c.call_method_b # => Hello world!

```

可是有時候我們並不想讓實例變數被外部取得，例如可以用 `c.instance_eval { puts @shared }` 挖出實例變數是什麼，這樣探詢別人的狀態，一般被認為不是好事，因為如果它想讓我們知道它的某個狀態，它應該會提供個函式讓我們來使用。所以如果不想讓別人知道這個物件（object）裡面有什麼，就可以使用剛剛介紹的方法：

``` ruby
class C
  shared = "Hello world!"

  define_method(:call_method_a) {
    puts shared
  }

  define_method(:call_method_b) {
    puts shared
  }
end

c = C.new
c.call_method_a # => "Hello world!"
c.call_method_b # => "Hello world!"
```

這樣一來就沒有人能夠挖出 `shared` 到底是什麼了，隱私保護地相當好，而這個技巧被稱為 Shared Scope。:)
