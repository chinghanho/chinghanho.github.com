---
layout: post
title: "Hacker News 推文系統演算法用 Ruby 實作"
date: 2012-10-12 23:37
comments: true
categories: Programing
---
<!-- MathJax -->
<script type="text/javascript"
  src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>

[Hacker News][hacker-news] 是現代資訊人相當重要的資訊來源，今天在 Github 上找到一個[用 Rails 製作的 clone][auth-love]，一時興起就抓來玩玩，這項 project 將整個前端幾乎都複製了過來，不過後端當然完全不同，可惜缺少了最重要的推文演算法，所以我就找了一些資源，然後用 Ruby 來實作。

[hacker-news]: http://news.ycombinator.com/
[auth-love]: https://github.com/happypeter/auth-love

參考了一下「[How Hacker News ranking algorithm works][hacker-news-ranking-algorithm]」的算法後，發現其實這套算法一點也不複雜，而且簡單得讓我有點意外，寫成比較熟悉的數學公式像是這樣：

[hacker-news-ranking-algorithm]: http://amix.dk/blog/post/19574

<div markdown="0">
$$Score = \frac{P-1}{(T+2)^{G}}$$
</div>

這裡翻譯一下每個變數代表的意義：

* P 表示每篇文章的 points，減一是為了刪掉作者自己投自己的那票（沒有人自己按自己讚的啦！XD）
* T 表示時間間距，單位為小時，加二是避免分母太小
* G 表示重力係數，意思是文章分數下降的速度

G 預設給 1.5，除非網站的文章提交數量很多很快，否則我認為沒必要太高{% fn_ref 1 %}。如果用 Ruby 可以這樣寫：

```ruby
def compute_score
  time = (Time.now - self.created_at)/3600.0
  gravity = 1.5
  points = self.points.to_i

  score = (points - 1)/(time + 2)**gravity
  self.update_attributes(:score => score*1000)
end
```

可是真的超級醜，「It just works!」能動就好（咦？），不過我已經想好該怎麼「重構」（是這樣講的嗎？）了，今天寫的 code 已經 pull request 給作者，明後天有時間再來修正{% fn_ref 2 %}。

然後要找個計時器讓應用程式每隔一段時間就要來刷 score，找了一下 [Toolbox][scheduling] 看起來 [rufus-scheduler] 比較簡單，只要在 Model 加上一些時間參數就可以自己跑了：

```ruby
require 'rufus/scheduler'
scheduler = Rufus::Scheduler.start_new

scheduler.every '1m' do
  self.all.each { |x| x.compute_score }
end
```

[scheduling]: https://www.ruby-toolbox.com/categories/scheduling
[rufus-scheduler]: https://github.com/jmettraux/rufus-scheduler

{% footnotes %}
{% fn 這個係數影響了更新速度，所以非常重要，憑個人感覺來看，如果網站一天沒有新增超過 10 篇文章，其實 1.5 還嫌高。 %}
{% fn 那欄 score 乘以 1000 是因為浮點數寫入資料庫會進位成 0，所以把它放大 1000 倍為整數。 %}
{% endfootnotes %}
