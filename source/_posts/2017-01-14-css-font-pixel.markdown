---
layout: post
title: "CSS Font (2) - 像素"
date: 2017-01-14 22:49
comments: true
categories: Programing
---

![](https://lh3.googleusercontent.com/QYgIGri4TjlqD74S4v_H39wBvIzcSIEr8wtKJAImXvSc-ZQO8B5WGE5qvYD3_k3PdinLcUXJ7UHZBLGtZJtAP880O5B9q-WcU_VXJPzVWAxgl3TsT27kXFIOYuPhXQ8koXbDjeQDmFOQ4btB2nw_cWIWaczmLL70eExzVdO_1_Wo7E6jXGdHlkYC8m2al-azmM_8vIY5zWKlrdT2ldDaAofCIV0wVOYZN3QwaRZ1vQyRLHa3Wtp1aP1pj2mIHhTpZBWtXcXOg6VFXS_ImBMKhzEFUtOHYkxKJ89p2aiYSq47XnLCDbBo-4ZdEHWqDvXBkWJcHXFalGwRm_HjhyravjJvcdiudp1NJdkQmYO8Yd06QNs7FY8DzSImi3sq-oJIuO4eHsWM8zOASEy0IgV6VubRVUq4VcOCjxd7DOxRzE6iEk2waFr7cI4IRiShvGQ2mkDjYRcf2bqk-aNlpSP1f3pMnK9B1pYSwz-_AgfIq4ZCkYSkKAxbxOF0q-HWb-QcDv_qnyU7S6bFnwjHXsAV8A1d41y9o5HCGIwJyejBdtbn4WFf_7o5gWJesDFU2Eg8sJoaFknTgvUMrbtoApsRlR8zNxayvAZZhILYPS_qXt1xfrI1SZYmeFPNgRbL818lTrQVd0lCf6a1UHrgtdZ2sndlRDRRi4nb-xlHiJYQHbM=w1276-h718-no)

（圖片取自 [unconed](https://acko.net/files/fullfrontal/fullfrontal/webglmath/online.html) 網站，版權為原網站所有。）

瞭解電腦字型如何被創造，能夠進一步理解文字如何在螢幕上顯示。本系列文章目的在於研究電腦字型在螢幕上顯示的原理，以便理解 CSS 有關字型的各項屬性背後的設計目的，以及要解決的問題。本文適合對象為從事網頁設計的專業人員，不論是網頁設計師、前端工程師，或是全端工程師。

<!-- more -->

## Pixel

現代螢幕最小的基本單位為像素（pixel），由非常非常多的「小燈泡」組成。一個像素是由紅、綠、藍三個顏色的「小燈泡」組成，他們透過控制開關、亮度，組成不同變化的加法混色，能夠產生非常多種顏色。為了簡化概念，可以將像素理解成一個個小格子，整齊排列構成一個巨大的矩形網格。電腦在繪圖時，是按照網格座標逐一設定每個格子（cell）顏色屬性的 [RGB 值](https://en.wikipedia.org/wiki/RGB_color_model)來呈現圖案。有個說法，像素的英文 pixel 的是 picture + cell 組成的詞彙。

Full HD 的螢幕解析度是 1920x1080，表示螢幕橫向 X 軸有 1920 個像素單位，縱向 Y 軸有 1080 個像素單位，總共超過 200 萬個像素單位，這種螢幕實際所擁有的像素數量通常稱為裝置像素（device pixel）。

有些藝術家、工程師用人工的方式在網格上一格一格地點上顏色來製作圖案，這種圖案風格稱為 pixel art。使用像素方格將文字筆劃一點一點點出來的字型，稱為[點陣字型](https://en.wikipedia.org/wiki/Computer_font#Bitmap_fonts)（bitmap font）。以前螢幕解析度不高，要在有限的像素內顯示可判斷的文字筆畫，需要使用點陣字型。現在使用最多的場合是大眾運輸上的跑馬燈，或是某些車票、收據仍可以看到點陣字型的使用。

![點陣字形](https://lh3.googleusercontent.com/4EZC27DSZk60uyIXHkeLNCaCfp6gDi5UzHX6EVZmAbOpDuBWonrSRePE4cgxeslOSH4dzN6a3G8VzCwCEECWxE2finNMJRZtRxFvjk14gxhuBq4518uAd91c-CeI7uXpoV_sIYR7gML4aYNrqzAgepVfSJYHzgle0XQXxU_qsDOWHE_4swdo2ega1XiRaSJcBseZq5zSIxduP5evzrq4sbs6zfgmIY7PkwQdsEsi8xp85WR3_N9n8u25glDHThBRMAdGLyMGpR2FYFkmldkIWT0YmuZ2jnK_d_NqGbdFjL1vzvwA5WAyChxapxpsWAHYu5ie4lvjsQOFgWJKHfYdVGE3fJXZpvmoEw5_uroTV2bDtsswRMB13sED_TI_oHAzD03IFHWvqZfVRi-ZArFwrbxKGpH3maY7liAV24zmXq519SI3NPT9FLFMn4RHprv3YoEJMK-09QwZK_Nn3-x_jZIId0zwnDT8LF4kcXr6HYH2IJCrJ7p5BplXpba7i-BK0ou4GJhY59UMbGScAShHSR1Ku7JngJDnAkPhZmkpNWJEoNHgBpBR63G2m6iInAyM5pPmSxHfe7UlYFuP9zUI4j13Uy2nk7qT78X8sOKYuA6miHvn9DJeK7a7KjCYJBYrqibTAveIFNG9QSdWS2PjB-I1_6Jik4x7jEAODuEyKs0=s1600)

若要滿足動輒超過一、兩百萬像素的現代螢幕，便不可能一點一點地點出文字的筆劃，需要用數學方程式來表達向量的線條、幾何圖形，例如 [Bresenham 演算法](https://en.wikipedia.org/wiki/Bresenham%27s_line_algorithm)繪製線條、[Bézier 曲線算法](https://en.wikipedia.org/wiki/B%C3%A9zier_curve)繪製曲線、[Scanline 填充算法](https://en.wikipedia.org/wiki/Scanline_rendering)填滿實心的圖形。

透過記錄文字線條變化的節點座標（x,y）來繪製筆劃輪廓的字型，叫做[輪廓字型](https://en.wikipedia.org/wiki/Computer_font#Outline_fonts)（outline font）。輪廓字型使用許多直線與曲線組成文字筆劃的輪廓形狀，再以填充演算法填滿內部空間，繪製成文字，這個過程稱為算圖（render）。

![輪廓字](https://lh3.googleusercontent.com/WHzAidFm9liuCdl8PO8dyPUWFYAOxwI2M4CNCh0xQoBCSo_tCdqQ3hX96y0qYyn4oLtZnjdIzlx4z_b6HBrgRII9UF_THi_EBNzDwsCOEHDqQzdzBb-EiohTtWNdvpNQWQPNI6z4FAesQROqPDil-i2-uLup3kagfCa-0qYAs5qrVYxG_Yb1ojAsxWJuuBQg2-h99jEHmypIBLc9J2MfUNuJReGsUl5tRKgJqjhVjJQatGfH01yjrTPNvSbjR3hnYZPm4CVSa1UT9byv9hGpH-nNl9EjvynQvArA0tzHXwDnV9BMNEaW7Q3A8iXwu03HbecCglRmc6grTr3EHoewmzRes15Hj79qxgCQXCEb3A-RHqZa71MQ9bykR1g4ZXmjTSwWRbdpDBRfHMNHYI7lS2JyaMonlPNWJxnMGg9ytfi2_kigx7EfCL5otj4YQQXnIPFKGt_6GoBBX0rjLYho5z3NkJk0mGrdRApua21Te0WPvR1RLuk7jm0v-0GUYMBX1rvoLvGmHemOuQPLISHFndtFTgPk4RYCWVTv1IvfqQI9HWpMsvFtgM_3QWJr56KcXwvCjMVhMvJ_waQZHQvj8D8sOSHGCPlSmAnwHeAWuVSwG2Bl-LshaZ4JgzboHdC7xR3-l5RL3h6bO4DgHCghpRf0UlsVi36JBtCADzzX6X4=w706-h628-no)

目前算圖並沒有一個統一的規範標準，各個引擎的實作方式皆有些微差異，因此在不同的瀏覽器（Chrome、Firefox）、作業系統（macOS、Windows）上相同一套字型的同一個字母，都會看起來不太一樣。

![相同頁面不同瀏覽器 render 的結果不同](https://lh3.googleusercontent.com/9Hy5Ta3RBWYdhc-IS8FaUwRFCVx795D_9jtScJhIaWsQJ2VLMU0HVf9S2ptpwuAyphS7x1sVuN9f3NPONlxsNUNU9DMQCWldsjKr12J-kLVV_4U0bfNiikauuq5RXB6Ytcf-HMZaN57hLk_o8HRXDubsg1jDvTISCCteow8C374bdfOxlmhbzHXMeTROMX9iZQWGWSvXT3aq_UXjHBBKZw_4uzn7utR3CIIp3MF9uy83JJscK-5-lhZiaXmhCJPYHJ3ZnhuGNJ2E_4FBdE1GkXJE2yr--GFiqW3vAR5BzQANp1h87f9iHIq_f4dWpC-IuvXKvzh5txrRBlVhGd06hXHFXjMDVy5OQQj-7Fq_FBTuWa-F5bnyq4pKdzsYh6NP1W3SENKqdn2-5RvKWJSUhg1m9M2pt93AD2PEejuMLzu4GkN1ivfCRoxOStabAi3fOlYv0z7DmlTaMrWBS1LgzpwT9M19gpTzXPRMImmXWJK9Mky66_Tezbd0cm0JXjjoEEfgwFtqCq6y5R4tG87Qi4fAcbcMOYFr_OOVbnxd9rcQ1cuXcB4sYgTh2jYTpBiPp4qGQpsC0r8rkrDJXJkL6t5g7DnroHftjSQWtMmofxT9Jld9unNWGWk3CyVqILN9IhbJZvb2HSB5otvOoI7ZXMCc1v-uR8Lj_0II1BtgHK4=w450-h74-no)

不過螢幕像是個巨大的矩形網格，無法直接顯示向量的圖形，顯示過程需要經過點陣化（raster），由電腦程式自動設定每個格子的顏色屬性，將向量線條轉映到一格一格的像素上。

## px 不是 Pixel

裝置像素的單位以 px 表示，但是 CSS 所定義的 px 並不與裝置像素 1:1 直接對應，至少不是想像中那麼單純的 1px 等於 1 個裝置像素單位，儘管他們都會使用相同的表示方式：1px、320px、1920px。可是兩者定義上卻有些微差異。

須知 CSS 支援的長度單位相當多，有字型排版歷史流傳下來的 pt 以及 pc（pica），也可以使用日常生活可能每天用到的公分（cm）、英吋（in），還有個最特別的單位是 px。對於已經熟悉 CSS 的工程師、設計師大概沒人不知道什麼是 px。px 不像其他單位有實際的物理長度可以相互轉換：

    1in = 2.54cm = 25.4mm = 72pt = 6pc

因為他們是由實際的物理長度組成的絕對單位，物理長度不論在哪都是相同的，基本上每個人伸兩根手指大概都可以比出一公分大概有多長，但是螢幕因為解析度不同，每英吋像素密度（pixels per inch，PPI）都不一樣，例如 iPhone 5s 的像素密度是 iPhone 3G 的兩倍，兩者裝置像素 1px 不會是相同的物理長度。

W3 早在 CSS Level 1 就提出了[參考像素（reference pixel）](https://www.w3.org/TR/css-values/#reference-pixel)的概念，定義了 CSS 的 px 單位是參考像素而不是裝置像素。參考像素 1px 等於 1/96 英吋（大約 0.26mm）。與其他絕對單位的轉換公式可以寫成：

    1in = 96px = 72pt = 25.4mm

當設定字級 12pt，電腦需要將 pt 單位轉換為參考像素 px，才能知道螢幕需要用多少像素來顯示文字。依照前述的公式做簡單的單位換算：

    12 / 72 * 96 = 16

計算結果是 12pt 的文字在網頁上顯示的合理邊長為 16px；當 CSS `font-size` 設定 16px 即表示設定 EM square 邊長的對應像素為 16px，換句話說，`font-size` 的大小等於 EM square 的大小。然而這只是字型的 EM square 的高度，並非字形本身的高度，要計算字形本身輪廓的像素寬高則需要從 EM square 的座標系統換算。

## 字形座標轉換 Pixel

技術上 EM square 是個二維空間的座標系統，FUnit 是等分座標系統的單位，沒有實際的物理長度，只用來表示相對的距離。為了能在螢幕上顯示字型文字，需要將相對的輪廓座標（x,y）等比例縮放到具體的像素座標上。

輪廓字型以 Roboto Black 為例，這個字型 EM square 邊長為 2048 FUnit，大寫 I 用了四個座標點，座標點之間皆為直線線條連接：

    x=474 y=0
    x=137 y=0
    x=137 y=1456
    x=474 y=1456

![roboto-black](https://lh3.googleusercontent.com/wwZ5ylRJC6VM5MVFhB7D4he8hF2x3-gjzHQmr_Lj01RJ0MonUrOe3Mzhgd4EgItDuynUZL9FhWjixWmHdfccX_MryTOFAFdy0A5E6s669-lMpzDTjqow_-P1PNVceVxeWud69JzRIh4g8gwVWVljvEZCCKwP9SJJifBjUxQFdGCaghsgvYhbomk2VXM2qdAu6KK-jMwZW_tQAuTq6SIaY294XZFBzMdvbkoFo4MGU89jTDJ0-elKs-WZQUlhBtjrQjbeVdlZA7e57SltdJNw2fCy94mO0GWhR4l_J8pe842wPBeI-qBnjJIimAGmTDwUJTKrhjhRUdyWt4BiMJ0sCW4MCXMh2-TcZXXB8rETfJpgWputpDI1S-QhvWNg56jxeOFee-MucQxPuD0bbicwBh8nQuIjRld4UEto2RyGgODq9T70Wgy_9MBRd30GzGMevFR_OZhFwG_IBhvzryxFS0FWloU90W6q8ispbs2LRnpU8058_JymUlRwmbfrIMJW-bplo_n23hIPV4N06oOFkB0_8zae2S4ecMQwGQO5CPhOoEun7_-TrmGdQioOFjpaX7k3gDlzpHENyrz_D5XcQCqkDTizEJStdsKxNUceH8--eEDAoLlbgIIpM57V4007j6hiVQnhGB8_7kWjni6XXEGX99bgv02Y6gAppvUxz7Q=w500-h382-no)

網頁上如果設定 12pt 已經知道合理邊長為 16px，做個簡單的等比例縮放計算：

    (round(x/2048*16), round(y/2048*16))

最後的像素座標分別為：

    (3.7, 0)
    (1.07, 0)
    (1.07, 11.38)
    (3.7, 11.38)

由此得知這個字形最後在螢幕像素上會是個寬高 2.63x11.38 的矩形。也可另外使用以下公式計算字形會用到的像素寬度跟高度：

    X/Y軸使用的FUnit * ((字級大小pt * PPI / 72 * 字型UPM尺寸))

試算結果：

    337 * ((12 * 96) / (72 * 2048)) = 2.63
    1456 * ((12 * 96) / (72 * 2048)) = 11.38

因此可以計算出 Roboto Black 的大寫 I 寬度使用 2.63 個像素，以及高度使用了 11.38 個像素。不過像素是螢幕上的最小單位，基本上是一個一個的整數，無法只顯示半個像素，為了實現這個目的，需要用到反鋸齒與子像素的技術。
