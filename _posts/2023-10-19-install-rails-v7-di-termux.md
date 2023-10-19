---
layout: post
title: Install Rails v7 di Termux
date: 2023-10-19 20:55 +0700
tags: [ruby, termux, rails7]
---

Udah lama banget saya gak update mengenai hal-hal yang berbau Ruby atau Rails. Mengingat sudah 2 tahun saya gak ngoding Rails.

Biar gak lupa saya menyempatkan ngoding Rails dikit-dikit di termux di HP saya yg jelek ini :(

Oke langsung aja.

Buka termux dan pasang beberapa paket kebutuhan development berikut:

Sebelum pasang sebaiknya kamu update dulu package termux mu.

```bash
pkg update && pkg upgrade
```

Setelah itu baru pasang dependensinya.

```bash
pkg install ruby clang make binutils pkg-config libxslt
```

Nah, setelah itu kita pasang nokigiri disuaikan dengan device HP kita. (Ini udah pernah saya jelasin dulu pas install Rails 6 di Termux)

```bash
gem install nokogiri --platform=ruby -- --use-system-libraries
```

Nah baru deh kita pasang railsnya.

```bash
gem install rails
```

Bikin project Rails mu.

```bash
rails new blog
```

Tunggu sampai selesai proses instalasinya dan pastinya nanti akan error. Kok gitu?

Ini karena gem `tzinfo-data` yang ada di default Gemfile di Rails tidak memasang khusus untuk device HP. (Ini juga pernah saya jelasin di artikel install Rails 6).

Jadi kamu cuma perlu edit di Gemfile untuk tzinfo-data seperti berikut:

```
gem "tzinfo-data"
```

Save dan Ketik bundle install.

```bash
bundle install
```

Beres kamu bisa ngoding rails di HP, buakakak 🤣

## Limitasi

Karena kamu pasang Railsnya di Termux di HP ya jelas banget pasti ada limitasinya.

Selama ini yang saya temukan cuma dua:

1. Kamu gak bisa pasang database selain sqlite. Kalau nekat pingin pake database semisal postgresql kamu perlu server, jadi kamu jalankan melalui remote database.

2. Kamu gak bisa pasangan tailwind-rails (ini bentuknya executable). Kalau kamu mau pake tailwind, harap pasang lewat package nodejs saja.
