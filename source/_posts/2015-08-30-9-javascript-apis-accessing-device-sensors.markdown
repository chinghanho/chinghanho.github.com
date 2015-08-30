---
layout: post
title: "9 個存取硬體裝置的 Javascript API"
date: 2015-08-30 17:09
comments: true
categories: Programing
---

原生 HTML5 和 Javascript API 可以直接從瀏覽器存取硬體組件和感測器。這裏的硬體裝置主要是指手機。

### 點擊連結傳送簡訊

``` html
<a href="tel:0912345678">打給我！</a>

<!-- Android -->
<a href="sms:0912345678?body=你到哪了？">傳簡訊給我！</a>
<!-- iOS -->
<a href="sms:0912345678&amp;body=你到哪了？">傳簡訊給我！</a>
```

延伸資料：

* [How to pre-populate the sms body text via an html link - StackOverflow](http://stackoverflow.com/questions/6480462/how-to-pre-populate-the-sms-body-text-via-an-html-link)

### 地理定位 API

``` js
if (navigator.geolocation) {
  navigator.geolocation.getCurrentPosition(function (position) {
    var whereareyou = position.coords.latitude + ' ,' + position.coords.longitude
    alert(whereareyou)
  })
}
```

### 裝置方向與裝置體感 API

這兩個功能是建立在裝置的陀螺儀和指南針感測器上，它能夠傳回目前裝置的旋轉角度，以及三個維度。

``` js
if (window.DeviceOrientationEvent) {
  window.addEventListener('deviceorientation', function(event) {
    // gamma 是左到右傾斜的角度
    // beta  是前到後傾斜的角度
    // alpha 是裝置面向在指南針方位上的角度
    document.body.innerHTML = event.gamma + ' ,' + event.beta + ' ,' + event.alpha

  }, false)
}
```

有了裝置體感的 API，也可以做出搖一搖復原輸入的功能。

``` js
if (window.DeviceMotionEvent) {
  window.addEventListener('devicemotion', function(evnet) {

    document.body.innerHTML = evnet.acceleration.x + '<br/>' + evnet.acceleration.y + '<br/>' + evnet.acceleration.z

    // 加速感測
    console.log(evnet.acceleration.x)
    console.log(evnet.acceleration.y)
    console.log(evnet.acceleration.z)

    // 加速感測包括重力參數
    console.log(evnet.accelerationIncludingGravity.x)
    console.log(evnet.accelerationIncludingGravity.y)
    console.log(evnet.accelerationIncludingGravity.z)

    // 旋轉率
    console.log(evnet.rotationRate.alpha)
    console.log(evnet.rotationRate.beta)
    console.log(evnet.rotationRate.gamma)
  }, false)
}
```

延伸資料：

* [裝置動作 - Web Fundamentals](https://developers.google.com/web/fundamentals/device-access/device-orientation/dev-motion?hl=zh-tw)
* [裝置定向 - Web Fundamentals](https://developers.google.com/web/fundamentals/device-access/device-orientation/?hl=zh-tw)

### 震動馬達

目前只支援 Android 的 Chrome 以及 Opera、Firefox，iOS 尚不支援。

``` js
var btn = document.getElementById('btn')
var vibrate = navigator.vibrate || navigator.mozVibrate || navigator.webkitVibrate
btn.addEventListener('click', function (event) {
  vibrate(1000)
})
```

延伸資料：

* [Navigator.vibrate() - MDN](https://developer.mozilla.org/en-US/docs/Web/API/Navigator/vibrate)

### 剩餘電力管理

``` js
var battery = navigator.battery || navigator.webkitBattery || navigator.mozBattery

function log(_battery) {
  document.body.innerHTML = '目前電力剩下：' + (_battery.level * 100) + '%'
  _battery.addEventListener('chargingchange', function() {
    alert('開始充電！')
  }, false)
}

if (navigator.getBattery) {
  navigator.getBattery().then(log)
} else if (battery) {
  log(battery)
}
```

### 亮度感測器

目前只有 Firefox 支援這個 API。

``` js
if ('ondevicelight' in window) {
  window.addEventListener("devicelight", function (event) {
  // 回傳的是亮度單位 lux
  console.log(event.value + " lux")
  })
}
```

### 近距離感測 API

這個也是只有 Firefox 支援。

``` js
if('ondeviceproximity' in window) {
  // Fired when object is in the detection zone
  window.addEventListener('deviceproximity', function (event) {
      // Object distance in centimeters
      console.log(event.value + " centimeters")
    })
} else {
  console.log("deviceproximity not supported")
}

if('ondeviceproximity' in window){
  // Fired when object is in the detection zone
  window.addEventListener('userproximity', function (event) {
    if(event.near == true) {
      console.log("Object is near")
    } else {
      console.log("Object is far")
    }
  })
} else {
  console.log("userproximity not supported")
}
```

----

參考：

* [9 JavaScript APIs accessing Device Hardware](http://www.webondevices.com/9-javascript-apis-accessing-device-sensors/)
