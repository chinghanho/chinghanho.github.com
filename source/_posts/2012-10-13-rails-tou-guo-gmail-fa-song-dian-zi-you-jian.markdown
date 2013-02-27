---
layout: post
title: "Rails 透過 Gmail 發送電子郵件"
date: 2012-10-13 21:50
comments: true
categories: Programing
---
從 Rails 寄信要先寫 Mailer 模組，這部份可以交給 generator 自動生成：

    rails g mailer CommentMailer

這樣就會有 Mailer 和 Views 兩個部分，跟 Controller 的概念其實很像，在 Mailer 裡面定義的 action 也會對應到同名的 Views。這裡以 CommentMailer 為例，來定義一個 `comment_notify_email` 方法，在 Views 裡面可以同時建立兩個檔案分別是：

* comment_notify_email.html.erb
* comment_notify_email.text.erb

之所以這麼做是因為有的郵件伺服器無法判別 HTML 格式，但不論哪一種機器都可以接收純文字，所以可以同時準備兩套，這樣 Rails 會依據對方的伺服器來推送不同格式的郵件。

{% codeblock lang:ruby /app/mailers/comment_mailer.rb %}
class CommentMailer < ActionMailer::Base
  default from: "myname@gmail.com"

  def comment_notify_email(comment)
    @comment = comment
    @url = post_url(@comment.post, :host => "localhost")
    mail(:to => "myname@gmail.com", :subject => "There is a new comment on your blog.")
  end
end
{% endcodeblock %}

Views 的部份就跟 Controller 完全一樣了，在 Mailer 裡面定義的實例會傳送到對應的 Views 裡去使用，寫法就跟寫 CRUD 的動作一樣。接下來就是設定跟 Gmail 之間的 [SMTP][smtp]。因為是在開發環境，所以這裡設定 development.rb 就好：

[smtp]: http://support.google.com/mail/bin/answer.py?hl=zh-Hant&answer=13287

{% codeblock lang:ruby /config/environments/development.rb %}
config.action_mailer.delivery_method = :smtp
config.action_mailer.smtp_settings = {
  :address              => 'smtp.gmail.com',
  :port                 => 587,
  :user_name            => 'myname@gmail.com',
  :password             => 'mypassword',
  :authentication       => 'plain',
  :enable_starttls_auto => true
}
{% endcodeblock %}

值得注意的地方是：

* `:user_name`：要把整個電子郵件完整輸進來，不要只打名稱
* `:password`：如果有啟動應用程式專用密碼功能，就要輸入 Google 給的密碼{% fn_ref 1 %}。

可以開啟 console 先建立一個 comment，然後來試寄一下郵件，順利的話應該可以馬上在 Gmail 裡面收到你寄出的測試信件：

    CommentMailer.comment_notify_email(Comment.last).deliver

如果老收不到信件，可以在 config 中將這個選項打開（改為 `true`），有問題會報錯誤出來方便除錯：

```ruby
# Don't care if the mailer can't send
config.action_mailer.raise_delivery_errors = true
```

最後一個步驟就是把 `comment_notify_email` 這個方法寫在 Controller 裡面，例如寫在 `create` 這個 action 可以在物件建立的時候觸發寄出郵件的這個功能。

## Resources:

* [Action Mailer Basics](http://guides.rubyonrails.org/action_mailer_basics.html)
* [#020 How to send emails in Rails](http://railscasts-china.com/episodes/how-to-send-emails-in-rails)
* [Signing in using application-specific passwords](http://support.google.com/accounts/bin/answer.py?hl=en&answer=185833)

{% footnotes %}
{% fn 我在這裡卡了好久，後來才想起來是應用程式密碼搞得鬼。XD %}
{% endfootnotes %}
