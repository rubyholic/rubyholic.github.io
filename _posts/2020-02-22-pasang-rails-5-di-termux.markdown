---
layout: post
title:  "Install Rails 5.2.x in Termux"
date:   2020-02-22 21:47:37 +0700
categories: ruby25 termux rails5
---

Hanya bermodal smartphone, kalian sudah bisa belajar pemrograman ruby. Kalian hanya perlu pasang Termux dari PlayStore, kemudian pasang `rbenv` di dalam Termux. Caranya sama seperti postingan sebelumnya.

## Intro

Kali ini saya ingin membagikan kepada kalian tentang bagaimana cara memasang Ruby on Rails di Termux Smartphone.

Menurut pengalaman, kita tidak bisa memasang Rails terbaru, yakni Rails 6. Hanya bisa sampai di Rails 5. Untuk permasalahannya, saya sendiri belum tahu persis, yang jelas ketika di reproduce, terjadi error pada saat meng-compile gem `sass`. Jadi untuk sementara pakai versi 5.

Kemudian untuk versi ruby yang dipakai. Hanya bisa sampai versi 2.5.x. Tidak pada versi 2.6.x atau lebih. Permasalah yang timbul pada ruby versi 2.6.x adalah gagalnya meng-compile bigint di Rails. Lagi-lagi saya juga tidak tahu kenapa. Wkwkwk suram memang.

## Kebutuhan deploy

Sebelum memulai pastikan di Termux kalian sudah memasang kebutuhan deploy. Kalian tinggal pasang tools berikut:

```shell
$ pkg install clang make libxml2 libxslt libffi libsqlite nodejs vim
```

## Install Ruby 2.5.7

Jangan pakai ruby 2.6.x atau lebih. Maksimal versi 2.5.x seperti yang saya jelaskan diintro di atas.

```shell
$ rbenv install 2.5.7
$ rbenv local 2.5.7
$ ruby -v
ruby 2.5.7p206 (2019-10-01 revision 67816) [aarch64-linux]
```

## Pasang Gem

Beberapa gem ada yang menggunakan system library.

```shell
$ gem install -N bundler
$ gem install -N pkg-config
$ gem install -N nokogiri -- --use-system-libraries
```

## Pasang Rails 5.2.x

Di Termux hanya versi rails 5.2.x yang dapat berjalan mulus. Versi 6 tidak bisa.
```shell
$ gem install -N rails -v 5.2.4
```

### Buat Projek Rails

```shell
$ rails new blog
$ cd blog/
$ vim Gemfile
```

Tambahkan pada paling bawah `Gemfile`.
```shell
gem 'tzinfo'
gem 'tzinfo-data'
```

Lalu jalankan
```shell
$ bundle install
```

### Tes Projek

Untuk memastikan rails sudah berjalan atau belum kalian bisa tes dengan perintah:
```shell
$ rails s
```
Jika tidak terjadi error artinya semua instalasi sukses, dan kalian bisa buka projek rails kalian di browser.

Selamat belajar.
