---
layout: post
title: "Ruby 的 Dynamic Method 技巧"
date: 2013-01-14 18:37
comments: true
categories: Programing
---
這是繼上一篇 [Dynamic Dispatch][dynamic-dispatch] 寫的筆記，今天要分享的是 Dynamic Method 這個程式技巧。而 Dynamic Method 就是在程式 runtime 時，動態地定義方法，而不是事先寫好。

[dynamic-dispatch]: /posts/dynamic-dispatch/

直接舉例子比較容易理解，假設原本有個 `SourceData` class，可以從這裡透過像是 `get_name` 之類的實例方法（instance method）取得實例變數（instance variables）的值，它的 code 可能長得像這個樣子：

``` ruby
class SourceData
  def initialize(name, email, location)
    @name     = name
    @email    = email
    @location = location
  end

  def get_name;     @name;     end
  def get_email;    @email;    end
  def get_location; @location; end
end
```

如果我們想要從另外一個叫做 `User` 的 class，透過 `SourceData` 的實例方法來獲取它的實例變數，我們可能會這麼做：

``` ruby
class User
  def initialize(source_data)
    @source_data = source_data
  end

  def name;     @source_data.get_name;     end
  def email;    @source_data.get_email;    end
  def location; @source_data.get_location; end
end

data = SourceData.new("chh", "user@example.tw", "Taiwan")
user = User.new(data)

puts user.name     # => chh
puts user.email    # => user@example.tw
puts user.location # => Taiwan
```

這會有個問題就是，當我們想要在 `SourceData` 塞入更多實例變數的時候，例如新增一個 `get_birthday` 實例方法，還要記得去 `User` class 去補上一條，這便違反了 [Open/closed principle][open_closed_principle]，而且也不符合 [Don't_repeat_yourself][Don't_repeat_yourself] 原則，這樣寫程式會變得非常沒有效率。

[open_closed_principle]: http://en.wikipedia.org/wiki/Open/closed_principle
[Don't_repeat_yourself]: http://en.wikipedia.org/wiki/Don't_repeat_yourself

假如當我在 `SourceData` class 加上新的 `get_*` 實例方法時，電腦會自動幫我把另外一部分的程式也寫好，那這個世界就太完美了。而仰賴 Ruby 的動態特性，真的就是可以這麼完美：

``` ruby
class User
  def initialize(source_data)
    @source_data = source_data
    @source_data.methods.grep(/^get_(.*)$/) {
      User.define_infos($1)
    }
  end

  def self.define_infos(name)
    define_method(name) {
      @source_data.send("get_#{name}")
    }
  end
end

data = SourceData.new("chh", "user@example.tw", "Taiwan")
user = User.new(data)

puts user.name     # => chh
puts user.email    # => user@example.tw
puts user.location # => Taiwan
```

這樣寫的好處就是當程式需要延展、擴充的時候，我們不需要去動到其他的部分，可以專注在焦點本身，比較容易維護。這個時候不論 `SourceData` 新增多少個 `get_*` 實例方法時，都可以動態地在 `User` class 產生方法。