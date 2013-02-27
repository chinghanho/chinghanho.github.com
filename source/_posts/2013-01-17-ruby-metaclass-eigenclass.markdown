---
layout: post
title: "關於 Ruby class 他們沒告訴你的真相：MetaClass"
date: 2013-01-17 20:19
comments: true
categories: Programing
---
## 先從 Singleton Methods 講起

透過以下的 code 可以察覺，其實定義 singleton method 的方式跟定義實例方法（instance method）很像，就好像把某個 class 實例化後命名為大寫的 C，然後對這個實例定義方法：

``` ruby
class C; end
c = C.new

# instance method
def c.call_method
  # do something...
end

# singleton method
def C.call_method
  # do something...
end
```

為什麼舉這個例子呢？因為其實 singleton method 就是這樣創建的。

在 Ruby 入門教學書籍上，幾乎每本書都說 class 是一種 Class，而 Class 的 superclass 是 Object，查找方法的路徑是這樣一層層往上找，但是他們沒有告訴我們，其實每一個 class 上面還有一個神祕的 class，就叫做 metaclass，而這個 metaclass 定義的實例方法就是該 class 的 singleton method。

## 再來說 MetaClass

metaclass 比較正規的稱呼是 eigenclass，會這樣叫可能是因為 Ruby 的歷史因素，metaclass 聽起來比較直覺所以我都習慣這樣用。而 metaclass 也是一種 Class，用我們比較熟悉的方法來理解的話，它就等於是以下這個例子的 `D` class：

``` ruby
class D; end
class C < D; end
```

也因為這樣的繼承結構，我們才能在「實例化」的 `C` class 物件上，呼叫定義在 metaclass 內的實例方法。不過跟一般的類別繼承關係比較不同的是，每個 metaclass 都只屬於一個 class，它不能同時被多個 class 繼承。

由於每個 class 都有個 metaclass，當建立一個 class 的時候，Ruby 會自動為這個 class 創建一個 metaclass；而 class 的 singleton method 是定義在這裡面的，所以可以說 metaclass 才是「真正的」class。

Ruby 用一個特殊的語法來取得 metaclass，即使在不知道 metaclass 之前，應該都有過這個語法，只是可能不知道原理，那就是 `class << self`，這裡的 `self` 可以替換成任何想要取得 metaclass 的物件：

``` ruby
class C; end

c_meta = class << C
  self
end

c_meta
# => #<Class:C> 這就是 C class 的 metaclass
# 而 C 只是 Class:C 的一個 instance

c_meta.instance_methods
# 這將會列出所有 C class 的 singleton method
```

這是 Ruby 很抽象的部分，有時候光看文字解釋很難理解，又礙於篇幅我沒寫得特別詳細，只是就重點部分做一下筆記，提供記憶和回顧使用，如果是完全不熟悉 metaclass 的初學者，最好去看 [Metaprogramming Ruby][metaprogramming-ruby] 這本書，相信會有很大的收穫，更有效的作法，就是打開 irb 實際敲敲看。

[metaprogramming-ruby]: http://pragprog.com/book/ppmetr/metaprogramming-ruby

此外，關於 Ruby class 之間的繼承關係，可以參考這張 [RubyForge][rubyforge] 整理的圖表，畫得不算很好，但是可以清楚看出類別之間關係，對於學習也是個很好的教材。

[rubyforge]: http://objectgraph.rubyforge.org/dotOG.html