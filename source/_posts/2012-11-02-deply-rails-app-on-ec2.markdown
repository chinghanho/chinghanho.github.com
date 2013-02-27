---
layout: post
title: "如何佈署 Rails 應用程式到 Amazon EC2"
date: 2012-11-02 08:39
comments: true
categories: Programing
---
上禮拜練習佈署 Rails 到 [Amazon EC2][ec2] 伺服器上，也理解了一下如何透過 SSH 操作遠端 server，很多原理還不是很懂，大部分只是得過且過，碰到問題就用別人的解決方案，目前是能讓機器動起來就好。

[ec2]: http://aws.amazon.com/ec2/

如果哪天能夠不再只是按照別人的食譜來架設，能夠依照自己需求去調整，我想應該就是跨越了新手入門的級別了吧！

## 申請 Amazon AWS 帳號

對於新手而言使用 PaaS 的 [Heroku][heroku] 比較方便，不過 AWS 給的是更豐富的彈性。

[heroku]: http://www.heroku.com/

申請 Amazon AWS 需要信用卡帳號，新註冊的使用者可以有 12 個月的[免費方案][free]，適合初學者練習用。這部份可以參考[阿正老師][soft]寫的教學，整個申請流程和選擇機器的步驟，都有詳細的說明。而這篇文章用的作業系統環境是 Ubuntu 12.04 LTS 這個版本。

[free]: http://aws.amazon.com/free/
[soft]: http://blog.soft.idv.tw/?p=824

如果機器準備好了，應該會得到一個金鑰檔案（.pem）和一組 IP，這個時候就可以開終端機用 SSH 跟剛剛選好 server 連線：

``` bash
$ ssh -i YOUR_NAME.pem ubuntu@IP位址
```

等等會用到以下幾個工具，需要逐一安裝到伺服器上：

* RVM
* Nginx
* Passenger

## 調整 Ubuntu 環境設定

在安裝其他軟體之前，現對系統進行更新，確保軟體清單都是最新的：

``` bash
$ sudo apt-get update
$ sudo apt-get upgrade #對系統進行升級
```

修改系統時間，選擇到台北的時區，可以用 `date` 指令來確認時間有沒有正確：

``` bash
$ sudo dpkg-reconfigure tzdata
$ sudo apt-get install ntp
$ sudo ntpdate ntp.ubuntu.com # Update time
```

如果平常有偏好用 alias 可以這個時候先設定好，例如：

``` bash
$ echo "alias ls='ls -l'" >> ~/.bash_aliases
$ echo "alias sudo='sudo env PATH=$PATH'" >> ~/.bash_aliases
```

## RVM

有時候我們會需要同時安裝多個 Ruby 版本在機器上，所以需要一套版本控制系統來切換，這裡用的是 RVM（Ruby Version Manager）{% fn_ref 1 %}：

``` bash
$ curl -L https://get.rvm.io | sudo bash -s stable # 用 curl 指令安裝 RVM
$ source /etc/profile.d/rvm.sh # 啟用 RVM
```

把 RVM 安裝好以後輸入 `rvm requirements` 查看需要哪些 dependencies，例如我要安裝標準的 Ruby MRI 版本{% fn_ref 2 %}，那就需要安裝以下這些套件：

``` bash
$ sudo apt-get install build-essential openssl libreadline6 libreadline6-dev curl git-core zlib1g zlib1g-dev libssl-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt-dev autoconf libc6-dev ncurses-dev automake libtool bison subversion pkg-config
```

現在網路這麼方便，隨時可以在網路上找到最新的文件，所以沒有必要把文件下載到本機裡，以下這個指令可以預設不要下載 gem 的文件說明檔案：

``` bash
$ echo 'gem: --no-ri --no-rdoc'  >> ~/.gemrc
```

RVM 安裝 Ruby 前面要用 `rvmsudo`，因為 `sudo` 不會包含 RVM 的環境變數，這個步驟需要 compile 可能會花比較久的時間；安裝好以後安裝 Rails 套件，然後建立一個 app 來測試看看：

``` bash
$ rvmsudo rvm install 1.9.3 --default --with-openssl-dir=$HOME/.rvm/usr
$ gem list # 查看有哪些 Gems，主要是確保安裝成功
$ rvmsudo gem install rails
$ rails -v # 確認 Rails 安裝成功
$ rails new firstapp
```

Ubuntu 預設沒有 JavaScript 的直譯器，需要在 Gemfile 中解開這行註解，然後重新 `bundle install`：

``` ruby
gem 'therubyracer'
```

這個時候已經可以打開 server，執行 `rails server` 啟動 WEBrick server{% fn_ref 3 %}。在 Amazon EC2 Management Console 後台的 Security Groups 打開 3000 port，連到 Amazon 給的 Public 域名，順利的話應該可以看到 Rails 的初始畫面。

## Nginx + Phusion Passenger

要先把 Passenger 的 gem 安裝起來：

``` bash
$ rvmsudo gem install passenger
```

然後再安裝 Nginx 的 Passenger 模組：

``` bash
$ rvmsudo passenger-install-nginx-module
```

這時會先檢查軟體需要的 dependencies，如果找不到也不用擔心，會出現說明指導該怎麼做，例如我遇到這個軟體沒有安裝：

    * To install Curl development headers with SSL support:
      Please run apt-get install libcurl4-openssl-dev or libcurl4-gnutls-dev, whichever you prefer.

那就按照指示把它裝起來就好了，然後重新執行一次安裝模組的指令繼續安裝 Nginx：

``` bash
$ sudo apt-get install libcurl4-openssl-dev
$ rvmsudo passenger-install-nginx-module
```

如果軟體需要的 dependencies 都安裝好了以後，會詢問是要自動下載安裝，還是自訂安裝：

    Automatically download and install Nginx?
    
    Nginx doesn't support loadable modules such as some other web servers do,
    so in order to install Nginx with Passenger support, it must be recompiled.

    Do you want this installer to download, compile and install Nginx for you?

     1. Yes: download, compile and install Nginx for me. (recommended)
    The easiest way to get started. A stock Nginx 1.2.3 with Passenger
    support, but with no other additional third party modules, will be
    installed for you to a directory of your choice.

     2. No: I want to customize my Nginx installation. (for advanced users)
    Choose this if you want to compile Nginx with more third party modules
    besides Passenger, or if you need to pass additional options to Nginx's
    'configure' script. This installer will  1) ask you for the location of
    the Nginx source code,  2) run the 'configure' script according to your
    instructions, and  3) run 'make install'.

    Whichever you choose, if you already have an existing Nginx configuration file,
    then it will be preserved.

    Enter your choice (1 or 2) or press Ctrl-C to abort: 1

因為沒有特別的需求，所以選擇 `1` 完整安裝，詢問安裝路徑時，就用預設的即可。接下來等待 Nginx 編譯完成，這可能會花不少時間，可以去 [Hacker News][hacker-news] 先看個新聞。

[hacker-news]: http://news.ycombinator.com/

這裡我參考了 [@chloerei][chloerei] 的作法，新增 Nginx 伺服器啟動腳本，事實上我不曉得這腳本是怎麼運作的，這也就是我前言講的，目前功力太弱只能照著別人的食譜作為解決方案：

[chloerei]: http://chloerei.com/2012/08/05/rails-deploy-guides-1-base-deploy/

```
$ cd /etc/init.d
$ sudo wget https://raw.github.com/chloerei/nginx-init-ubuntu-passenger/master/nginx
$ sudo update-rc.d nginx defaults
$ sudo chmod +x nginx
$ sudo service nginx start
```

這樣以後可以用 `sudo service nginx [start|stop|restart]` 方式來管理伺服器了，變得比較方便一些。

修改 /opt/nginx/conf/nginx.conf 文件設定，裡面看起來很多東西，但是重點只有以下這幾個：

{% codeblock %}
http {
  ...
  passenger_root /usr/local/rvm/gems/ruby-1.9.3-p286/gems/passenger-3.0.17;
  passenger_ruby /usr/local/rvm/wrappers/ruby-1.9.3-p286/ruby;
  ...
  }

server {
  listen 80;
  server_name www.yourhost.com;
  root /somewhere/public;   # <--- be sure to point to 'public'!
  passenger_enabled on;
  }
{% endcodeblock %}

`passenger_root` 和 `passenger_ruby` 讓 Nginx 可以找到 Passenger 執行路徑；`server_name` 是網域名稱，`root` 指向 Rails app 下的 /public 目錄（!important）。

如果在 nginx.conf 檔案中有 `location /` 這段程式碼，把它註解掉，因為這樣可能會導致你看到 403 Forbidden。最後只要在 EC2 後台開啟 80 port，作法跟前面測試 Rails app 一樣，應該就可以正常連上了。

{% footnotes %}
{% fn 當然也有其他選擇，例如 <a href="https://github.com/sstephenson/rbenv">rbenv</a> 是相較下較輕量的 Ruby 版本控制系統。 %}
{% fn 以 C 語言實作的官方版本，通常稱為 CRuby 或是 MRI，意思是 Matz's Ruby Interpreter。 %}
{% fn WEBrick 是用 Ruby 實作的簡單 HTTP server，開發的時候很好用，可是拿來當 production server 就會不敷使用。 %}
{% endfootnotes %}