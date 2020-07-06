---
layout: post
title: Install Rails 6 di Termux
date: 2020-07-06 18:58 +0700
categories:
 - rails
tags:
 - rails6
 - termux
---

Saya pernah bilang dipostingan sebelumnya bahwa di Termux kita tidak bisa memasang rails 6. Dikarenakan ada dependensi yang harus di-compile, yakni `saasc` dan `nokogirii`. Termux gagal meng-compile dependensi tersebut. Akibatnya rails 6 tidak bisa dijalankan.

Sejak hadir Ruby 2.7.1, entah mengapa dependensi tersebut berhasil di-compile. Ini bukan magic! Yang ada saya tidak tahu cara kerjanya 😅

Sebelum memulai pastikan kalian sudah memasang `git`, `rbenv` dan Ruby 2.7.1.

Kalau sudah, kita tinggal pasang dependensi berikut untuk proses compile pada gem source.

```bash
pkg install make clang libxml2 pkg-config libxslt yarn sqlite postgresql
```

Kemudian pasang gem berikut:

```bash
gem install nokogiri -- --use-system-libraries
gem install sassc -- --use-system-libraries
```

Nah baru deh dipasang rails-nya.

```bash
gem install -N rails
```

Terakhir test buat project.

```bash
rails new blog
```

Biasanya akan terjadi galat. Mengapa? karena di Termux tidak terdapat `tzinfo-data`.

Kita tinggal sunting saja Gemfilenya.

```bash
vim Gemfile
```

Tambahkan gem berikut di akhir baris.

```
gem tzinfo-data
gem tzinfo
```

Lalu bundle ulang.

```bash
bundle
```
