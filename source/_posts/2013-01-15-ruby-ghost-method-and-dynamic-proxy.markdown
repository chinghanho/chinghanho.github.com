---
layout: post
title: "Ruby 的 Ghost Method 與 Dynamic Proxy 技巧"
date: 2013-01-15 12:04
comments: true
categories: Programing
---
昨天寫的 [Dynamic Method][dynamic-method] 是 code 在 runtime 時才動態產生方法，也就是說這個方法的確存在，只是在需要的時候才會建立，而且用 `respond_to?` 是可以驗證為 true 的；而 Ghost Method 則是像鬼一般的看不到，甚至用 `respond_to?` 也會返回 false 的，可是我們卻可以很神奇地呼叫它。

[dynamic-method]: /posts/dynamic-method/

## 關鍵在 method_missing

凡事都有例外，當呼叫方法時如果找不到，Ruby 給了 `method_missing` 這個方法來解決找不到方法時的補救措施，而 Ghost Method 技巧的關鍵就在於給了接收者一個不存在的方法時，然後由 `method_missing` 來當補救網。這裡引用昨天的範例程式，假設有一個 `SourceData` class：

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

然後我想在 `User` class 裡用 Ghost Method 技巧來取得 `SourceData` 的實例變數，那就要建立一個 `method_missing` 方方法來捕捉所有不存在的方法，然後用 `send` 方法轉送出去，這又稱為 Dynamic Proxy，因為就像是代理人將方法傳送給另一個物件（object）：

``` ruby
class User
  def initialize(source_data)
    @source_data = source_data
  end

  def method_missing(name, *args)
    @source_data.send("get_#{name}")
  end
end

data = SourceData.new("chh", "user@example.tw", "Taiwan")
user = User.new(data)

puts user.name     # => chh
puts user.email    # => user@example.tw
puts user.location # => Taiwan
```

## 除 bug 可能比較困難

看起來好像很好用，可是會有個問題，因為這個方法本來就不存在，因此很容易造成一些奇怪的 bug，例如透過 `user.respond_to?(:name)` 來檢查方法存不存在時，返回的竟然會是 false！用起來卻沒問題。所以使用 `method_missing` 時也要修改 `respond_to?` 的查找方式，以免測試一直跑不過。