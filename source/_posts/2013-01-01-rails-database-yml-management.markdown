---
layout: post
title: "Rails 的 database.yml 安全管理"
date: 2013-01-01 12:06
comments: true
categories: Programing
---
Rails 的資料庫設定都放在 config/database.yml 這個檔案裡面，包含伺服器位置、管理員帳號以及密碼等等的重要資訊，所以是不能放進版本控制系統中的。因此，除了要從版本控制系統中移除 database.yml，或者改製成樣板，也要告訴 git 停止追蹤檔案：

    $ git mv config/database.yml config/database.yml.example
    $ echo "config/database.yml" >> .gitignore

那在使用 Capistrano 自動化佈署時，就需要另外將這個檔案另外上傳到伺服器去，這裡以 EC2 為例，透過憑證連上伺服器，把本地檔案丟到上面去：

    $ scp -i path_to_cer.pem config/database.yml user_name@my_server.com:~/path_to_rails_app/shared/config/database.yml

最後在 config/deploy.rb 寫一個腳本，自動用 symlink 覆蓋上去就好了，以下是個範例：

``` ruby
# in RAILS_ROOT/config/deploy.rb:
after 'deploy:update_code', 'deploy:symlink_db'

namespace :deploy do
  desc "Symlinks the database.yml"
  task :symlink_db, :roles => :app do
    run "ln -nfs #{deploy_to}/shared/config/database.yml #{release_path}/config/database.yml"
  end
end
```