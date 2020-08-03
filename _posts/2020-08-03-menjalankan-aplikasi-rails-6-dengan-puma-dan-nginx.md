---
layout: post
title: Menjalankan Aplikasi Rails 6 dengan Puma dan Nginx
date: 2020-08-03 16:38 +0700
categories:
    - rails
tags:
    - rails6
    - puma
    - nginx
---

Pertama-tama kita sunting berkas `config/puma.rb`:

```ruby
## letakan app_dir paling atas
app_dir = File.expand_path '../..', __FILE__

### untuk bind bebas mau ditaru di mana pun, yang terpenting sesudah app_dir
bind ENV.fetch("SOCKETFILE") { "unix://#{app_dir}/tmp/sockets/puma.sock" }
```
Kemudian buat _service file_ menggunakan `systemd`:

```bash
$ sudo touch /etc/systemd/system/puma.service
$ sudo vim /etc/systemd/system/puma.service
```

Isi konfigurasi Puma dengan Systemd:

```ini
[unit]
Description=Puma Rails Server
After=network.target

[Service]
Type=simple
User=user
Environment=RAILS_ENV=production
Environment=SECRET_KEY_BASE=$(BUNDLE_GEMFILE=/home/user/my_rails_app/Gemfile bundle exec rake -f /home/user/my_rails_app/Rakefile secret)
WorkingDirectory=/home/user/my_rails_app/
ExecStart=/home/user/.rbenv/shims/puma -C /home/user/my_rails_app/config/puma.rb
ExecStop=/home/user/.rbenv/shims/puma -S /home/user/my_rails_app/config/puma.rb
PIDFile=/home/user/my_rails_app/tmp/pids/server.pid
TimeoutSec=15
Restart=always

[Install]
WantedBy=multi-user.target
```

Setelah selesai, tinggal kita _reload_ dan jalankan `puma` -nya.

```bash
$ sudo systemctl daemon-reload
$ sudo systemctl start puma.service
$ sudo systemctl status puma
```

Untuk konfigurasi Nginx:

```nginx
upstream app {
    server unix:///home/user/my_rails_app/tmp/sockets/puma.sock;
}

server {
    listen 80;
    listen [::]:80;

    server_name my-rails-app.com;
    root /home/user/my_rails_app/public/;

    location / {
        try_files $uri @app;
    }

    location @app {
        proxy_pass http://app;
        proxy_set_header  Host $host;
        proxy_set_header  X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header  X-Forwarded-Proto $scheme;
        proxy_set_header  X-Forwarded-Ssl off; # change to "on" if https
        proxy_set_header  X-Forwarded-Port $server_port;
        proxy_set_header  X-Forwarded-Host $host;
        proxy_redirect off;
    }
}
```

