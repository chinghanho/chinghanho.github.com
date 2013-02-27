---
layout: post
title: "Retina Display Is Not Industry Standard"
date: 2012-06-30 22:39
comments: true
categories: Tech
---
<!-- MathJax -->
<script type="text/javascript"
  src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>

前幾個月新 iPad 剛發表老爸就從美國弄了一台回來玩，我最喜歡的就是閱讀 Pocket、Zite、Flipboard 這些文字為主的應用。說也奇怪，我不是很喜歡看書的人，但是卻超愛看有的沒的網路文章。Retina Display 帶來視覺上細膩的體驗真的是前所未有的驚艷，現在再回去看老姊那台 iPad 2，螢幕就像是被打上一層薄薄的馬賽克一樣，感受真的超級明顯。

## Retina Display

Retina Display 這個名詞最早來自 Apple WWDC 2010 的 iPhone 4 發表會，是一個噱到翻的行銷名詞，也是近幾年來網頁設計不斷被討論的話題{% fn_ref 1 %}。前幾天~~[好奇怪][05]的~~ Wired.tw 有位叫顏國偉的作者寫了這麼一篇「[新款MacBook Pro Retina在打什麼行銷算盤？][10]」分析文章，以下是他對 Retina Display 的敘述：

> 2012 年初發表的 New iPad 亦同樣是長寬像素倍增，從 1024 x 768 增至 2048 x 1536，不過實際解析度僅是從 132dpi 增至 264dpi，不如 iPhone 4 的 326dpi，解析度較低就更不可能超越人類視網膜的辨識能力，雖然如此，但 Apple 依然不斷行銷 Retina 一詞。
> 
> 而最新款的 MacBook Pro 也開始導入 Retina，顯示器的長寬畫素變成 2880 x 1800，也正好是原有 1440 x 900 的倍增，但實際解析度僅至 110dpi 增至 220dpi，其實更沒有資格聲稱超越人類視網膜了。

先不說他把 dpi 跟 ppi 混用的問題，就單單針對「解析度下降稱不上視網膜螢幕」的問題來討論，從 iPhone 4 326ppi 降低到新 iPad 264ppi，到了 Macbook Pro with Retina Display 的解析度降低到 220ppi，似乎已經跟 Steve Jobs 講的「人類視網膜極限是 300ppi」有所不合？

才怪！

## The limit and distance

{% blockquote Philip Schiller http://www.youtube.com/watch?v=EHcVjajO4LM WWDC2012 %}
The pixels of this display are so small that from a normal working distance your retina cannot discern those individual pixels.

每一個像素都非常非常小，在正常工作環境裡，你距離電腦螢幕的距離是無法分辨螢幕上每一個獨立的像素的。
{% endblockquote %}

我們剛好能夠從 Philip Schiller 的說明得到答案，Retina Display 從來就不是什麼工業標準規範，並不是必須要達到 300ppi 才能冠上這個稱號，事實上這是具備兩項條件的相對結果：

* 浩克級的超高解析度
* 眼球到螢幕之間的距離

Philip Schiller 還很宅的丟了一個公式出來嚇唬人，其中 a 代表人類眼球看到兩個不同像素的角度，h 代表兩個不同像素間距，d 代表肉眼跟螢幕的距離：

<div markdown="0">
$$a = 2 \tan^{-1} (\frac{h}{2d})$$
</div>

按照 Apple 的說法，iPhone 4 Retina Display 定義的前提是眼睛與螢幕保持 10 英吋的距離{% fn_ref 2 %}；而 iPad 因為螢幕較大就跟書一樣，一般人看書至少會保持大約 15 英吋左右的距離，當然囉，距離越遠對於 ppi 的要求自然不會那麼高，所以「解析度下降稱不上視網膜螢幕」這個問題顯然就不對。

可是要近到什麼程度？或是遠到什麼程度？才能稱作是 Retina Display？既然有了公式，我們秉著科學精神來實算一下，可以把參數套入公式得到了這個表格：

<table id="gradient-style" summary="Retina Display">
  <thead>
    <tr>
      <th scope="col">-</th>
      <th scope="col">ppi</th>
      <th scope="col">h（英吋）</th>
      <th scope="col">d（英吋）</th>
      <th scope="col">a（角度）</th>
      <th scope="col">retina ready?</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>視網膜極限</td>
      <td>300</td>
      <td>1/300</td>
      <td>10</td>
      <td>3.3*10^(-4)</td>
      <td>Yep!</td>
    </tr>
    <tr>
      <td>iPhone 4</td>
      <td>326</td>
      <td>1/326</td>
      <td>10</td>
      <td>3.1*10^(-4)</td>
      <td>Yep!</td>
    </tr>
    <tr>
      <td>The New iPad</td>
      <td>264</td>
      <td>1/264</td>
      <td>15</td>
      <td>2.5*10^(-4)</td>
      <td>Yep!</td>
    </tr>
    <tr>
      <td>MBP with RD</td>
      <td>220</td>
      <td>1/220</td>
      <td>20</td>
      <td>2.3*10^(-4)</td>
      <td>Yep!</td>
    </tr>
  </tbody>
</table>

我假設了一般人使用電腦距離至少會保持 50-60 公分，也就是大約至少 20 英吋左右。我數學不好，所以我拜託 [Google 幫我算][18]。如同表格所見，只要小於視網膜能夠辨識的最大極限，就符合 Apple 自己對於 Retina Display 的定義。

換句話說，如果我沒算錯的話 Macbook Pro with Retina Display 只要保持在 13.5 英吋（34 公分）以上都無法分辨每一個獨立像素的距離。

## Only Apple can?

我在文章開頭的部份就已經提過，Retina Display 是噱到翻的行銷名詞，事實上早有其他廠商做出這種裝置，不同的是大家的螢幕尺寸不一樣而已，這裡有更清楚的[列表][19]可以查看。但是毫無疑問的是 Apple 厲害的地方在於，把這麼高解析度的螢幕裝在 9.7 吋的 iPad 上，而且價格還可以保持這麼低。

還有一點很重要的就是軟體的支援度，不同裝置的解析度不同對於開發者而言絕對是個風險，這也是目前混亂的 Android 軍海常見問題。對於開發者而言，Apple 有良好的解決方案可以讓他們願意冒這個險。

## Wired.tw so weird

Wired.tw 那篇文章底下關於 iPod 部分的描述也是很詭異，他認定 Apple 的成功是有賴於洗腦的傳教辭令、精美的工業設計上，而完全忽略 iTunes 的存在。後來去查了一下這位作者以前寫的文章，原來這篇「[Apple iOS優勢不再，顯示生態優勢未如想像中重要][20]」奇文也是他寫的，順便看了一下他的[簡介][30]：

> 長達 20 年以上的 EE、IT 產業及趨勢觀察經驗，小到一個奈米等級的微電路、微機械元件，大到虛擬、雲端的全球一體化融合運算體均有研究。長期擔任技術編輯、產業分析師、市場分析師等工作，具有高科技產業的供貨、銷售分析實務歷練，近期也若干探索、涉獵 ID、UX 領域。

這提醒我自己還是專心在一個領域就好了吧！XD

我不曉得 Wired.tw 跟 TechOrange 是什麼關係，看起來總編輯好像是同一個人？而且之前發現好幾個作者都跟這位總編輯都是從台灣 ZDnet 出來的，喔對了，台灣 ZDnet 現在收攤關門了。如果 Wired.tw 文章繼續找這些人來寫，遲早也會關門大吉。

{% footnotes %}
  {% fn 可以看看這篇文章「<a href="http://www.creare-webdesign.co.uk/blog/videos/hd.html">Think Vector, Not HD</a>」，很有意思。:p %}
  {% fn 這可能還是要看使用情境吧，有時候用手機玩遊戲就會不知不覺越拿越近。XD %}
{% endfootnotes%}

[05]: http://weird.tw/
[10]: http://wired.tw/2012/06/26/blogger-macbook-pro-retina/index.html
[17]: http://www.mathjax.org/
[18]: https://www.google.com/search?sugexp=chrome,mod%3D8&sourceid=chrome&ie=UTF-8&q=a*arctan((1/220)/48)&hl=en#hl=en&gs_nf=1&tok=Js3AQkNmrBPMFa_5Mi-bIQ&pq=2*arctan((1%2F220)%2F48)&cp=19&gs_id=58&xhr=t&q=2*arctan((1/220)/40)&pf=p&sclient=psy-ab&oq=2*arctan((1/220)/40)&gs_l=&pbx=1&fp=1&biw=1440&bih=805&bav=on.2,or.r_gc.r_pw.r_cp.r_qf.,cf.osb&cad=b&sei=0ePvT7P5HuPPmAWl1ojjDQ
[19]: http://en.wikipedia.org/wiki/List_of_displays_by_pixel_density
[20]: http://wired.tw/2012/05/15/blogger-ecosystem/index.html
[30]: http://wired.tw/blogger
