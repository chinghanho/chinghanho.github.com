---
layout: post
title: "Rails 4 Mass Assignment"
date: 2012-12-30 16:07
comments: true
categories: Programing
---
![github-hacked](http://lh3.googleusercontent.com/-hgXyfwvn2go/UN_3CJlzFMI/AAAAAAAAFhg/GiaBdbUll8k/s690/%25E8%259E%25A2%25E5%25B9%2595%25E5%25BF%25AB%25E7%2585%25A7%25202012-12-30%2520%25E4%25B8%258B%25E5%258D%25884.09.42.png)

前陣子爆發的 [Github Hacked][github-hacked] 事件，就是因為沒有在 model 裡面正確設定 `attr_accessible` 或 `attr_protected` 過濾屬性，所造成的 Mass Assignment 漏洞。可是在 model 裡面寫 `attr_accessible` 來過濾傳進來的參數，這樣的動作很不太對勁……

[github-hacked]: https://github.com/rails/rails/commit/b83965785db1eec019edf1fc272b1aa393e6dc57

我想 Github 的開發者多是 hacker 級的高手，不致於犯這麼初級的錯誤。MVC 架構中 model 單純用來進行資料庫相關的動作，controller 才是負責所有邏輯部份，包括 client input、output，所以 Github 肯定是把過濾機制寫在 controller 裡面（像 37signals 就是這麼做的）。但是不管怎麼樣，最後漏洞還是出現了。:p

DHH 也在 [Twitter][dhh-twitter] 上說過：

> The right place to guard against these issues is in the controller, not the model. AR's attr_accessible is the wrong way about it.

[dhh-twitter]: https://twitter.com/dhh/status/176465643107909632

所以處理 Mass Assignment 的程式碼，應該要放在 controller 而不是 model，所有從 client 傳來的資料，都應該要在 controller 先行處理，對此 DHH 也提供了 [37Signals][37signals] 的解決方案，亮點在那個 `slice`：

[37signals]: http://37signals.com/

``` ruby
class PostsController < ActionController::Base
  def create
    Post.create(post_params)
  end

  def update
    Post.find(params[:id]).update_attributes!(post_params)
  end

  private
    def post_params
      params[:post].slice(:title, :content)
    end
end
```

基於上述的這些種種原因，所以到了 Rails 4 就把原本 `attr_accessor` 的工作拉到 controller 裡來做。以下是 [Santiago Pastorino][spastorino] 寫的範例，原本 Rails 3.2 的寫法是：

[spastorino]: https://twitter.com/spastorino

``` ruby
# app/models/user.rb
class User < ActiveRecord::Base
  attr_accessible :username, :password
end

# app/controllers/users_controller.rb
class UsersControlelr < ApplicationController
  def create
    @user = User.create! params[:user]
    redirect_to @user
  end
end
```

到了 Rails 4 的寫法改為以下，亮點在那個 `permit`：

``` ruby
# app/models/user.rb
class User < ActiveRecord::Base; end

# app/controllers/users_controller.rb
class UsersControlelr < ApplicationController
  def create
    @user = User.create! params.require(:user).permit(:username, :password)
    redirect_to @user
  end
end
```

這個方法其實就跟 DHH 提供的那個解法非常類似，在 actions 裡面決定哪些屬性是可以通過的，所以也可以把參數寫到 `private` 下方：

``` ruby
# app/controllers/users_controller.rb
class UsersControlelr < ApplicationController
  def create
    @user = User.create! user_params
    redirect_to @user
  end

  private
    def user_params
      params.require(:user).permit(:username, :password)
    end
end
```

這樣一來就可以決定哪些參數在通過 `create` 這個 action 的時候會被過濾掉，而且不會影響到其他的 actions，在維護上我想應該也會相較容易一些。

升級版本的過渡期間，如果想要使用舊寫法的可以安裝 [protected_attributes] 這個 gem，就可以繼續使用 `attr_accessible` 和 `attr_protected`；而想要嘗鮮使用新寫法，則可以安裝 [strong_parameters] 這個 gem，然後修改以下設定：

``` ruby
# config/application.rb
config.active_record.whitelist_attributes = false

# app/models/**.rb
include ActiveModel::ForbiddenAttributesProtection
```

最後修改所有 controllers 的參數寫法，參考上面 Santiago Pastorino 寫的 `user_params` 例子。等到 Rails 4 正式發佈時，只要把 application.rb 和 models 做的修改復原回來，還有把 strong_parameters 從 Gemfile 踢出去就完成了。

[protected_attributes]: https://github.com/rails/protected_attributes
[strong_parameters]: https://github.com/rails/strong_parameters