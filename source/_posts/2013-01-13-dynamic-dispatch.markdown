---
layout: post
title: "Ruby 的 Dynamic Dispatch 技巧"
date: 2013-01-13 22:41
comments: true
categories: Programing
---
三天前開始看 [Metaprogramming Ruby][metaprogramming-ruby]，希望可以在這個禮拜把整本讀完。讀書要寫筆記，一方面整理思緒、一方面幫助記憶、一方面還能分享，而且這本是英文原文，畢竟不是中文母語，有些內容當下看懂了，可是揮發得很快，所以我決定寫點東西下來。

[metaprogramming-ruby]: http://pragprog.com/book/ppmetr/metaprogramming-ruby

## Dynamic Dispatch

如果 `the_object.call_method`，那麼這個 `the_object` 就稱為接收者。[Dynamic Dispatch][Dynamic_dispatch] 是指依照接收者的物件類型，來決定呼叫的方法，這個技巧會非常需要用到 `send`。

[Dynamic_dispatch]: http://en.wikipedia.org/wiki/Dynamic_dispatch

## Send 用法

`send` 其實跟 `.` 一樣，不同的是他可以把方法當成參數，傳送給接收者使用，接收者可能是個 class，也可能是個實例（instance），例如一般的 Ruby code 會這樣子在 class 裡定義一個方法：

``` ruby
class ClassC
  def call_method
    puts "This is a method in class C."
  end
end
```

而我們可以透過以下的方法來呼叫這個 ClassC 的實例方法：

``` ruby
c = ClassC.new

c.call_method        # => "This is a method in class C."
c.send(:call_method) # => "This is a method in class C."
```

這裡把 `call_method` 當成 `send` 的參數（可以是 hash，也可以是 string），然後丟給接收者；既然方法可以被當成物件來丟，意思也就是可以把一些方法存起來成一個 array，然後依照不同需求的時候丟給接收者使用。

舉例來說，當 `ClassC` 有很多的實例方法時，可以把它們存成一個 array，然後用 Dynamic Dispatch 技巧傳給接收者：

``` ruby
class ClassC
  def call_method_a
    puts "This is method_a in class C."
  end

  def call_method_b
    puts "This is method_b in class C."
  end
end

c = ClassC.new

methods = ClassC.instance_methods(false)
methods.each { |method| c.send(method) }
# => This is method_a in class C.
# => This is method_b in class C.
```

有時候我們只需要部分的方法，而不是全部，所以會加上一些過濾條件，只撈出自己需要的部分，這樣的行為又稱為 Pattern Dispatch，因為是基於某種 pattern 來篩選方法。

以上就是 Dynamic Dispatch 的解釋，這個技巧經常可以在 Ruby 的 Test::Unit 標準庫裡看到，把 class 的實例方法（instance method）撈出來，然後一個一個代進測試程式碼裡面去跑。