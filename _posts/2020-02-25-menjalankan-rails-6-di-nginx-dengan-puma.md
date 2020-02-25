---
layout: post
title: Menjalankan Rails 6 di Nginx dengan Puma
date: 2020-02-25 14:48 +0700
categories: rails
tags: rails6 puma nginx
---

Mendaringkan aplikasi `rails`. Kita bisa memanfaatkan `puma` sebagai `reverse-proxy` yang nantinya bisa kita integrasikan dengan `nginx`. Berikut ini caranya:

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

Asumsi berkas `rails` kalian terletak di `/home/web/rails`, skrip `systemd` -nya sebagai berikut:

```ini
[Unit]
Description=Puma Rails Server
After=network.target

[Service]
Type=simple
User=web
WorkingDirectory=/home/web/rails/
ExecStart=/home/web/.rbenv/shims/puma -C /home/web/rails/config/puma.rb /home/web/rails/config.ru
ExecStop=/home/web/.rbenv/shims/pumactl -S /home/web/rails/tmp/pids/server.pid stop
TimeoutSec=15
Restart=always

[Install]
WantedBy=multi-user.target
```
Pada skrip di atas `puma` berada di direktori `.rbenv/shims` ini karena contoh di sini untuk pengguna [rbenv]. Untuk memastikan di mana `puma` berada kalian bisa menggunakan perintah `which`, contoh:

```bash
$ which puma
$ which pumactl
```

Setelah selesai, tinggal kita _reload_ dan jalankan `puma` -nya.

```bash
$ sudo systemctl daemon-reload
$ sudo systemctl start puma.service
$ sudo systemctl status puma
```

Jika sudah hijau berati kita sudah berhasil membuat `puma` dengan `systemd`.
Langkah selanjutnya adalah kita buat `server block` di Nginx. Dan masukan konfirgurasinya seperti berikut:

```nginx

upstream app {
    server unix:///home/web/rails/tmp/sockets/puma.sock;
}

server {
    root /home/web/rails/public/;

    location / {
	try_files $uri @app;
    }

    location @app {
	proxy_pass http://app;
	proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
	proxy_set_header Host $http_host;
	proxy_redirect off;
    }
}

```

### Khusus Pengguna https

Ubah pada baris `@app` menjadi seperti berikut:
```nginx
....

location @app {
    proxy_pass http://app;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header Host $http_host;
    proxy_set_header X-Forwarded-Ssl on;
    proxy_redirect off;
}
```

Terakhir tinggal kita _restart_ `nginx`

```bash
$ sudo nginx -t
$ sudo systemctl restart nginx
```

[rbenv]: https://rubyholic.github.io/rbenv/2020/01/31/install-rbenv.html
