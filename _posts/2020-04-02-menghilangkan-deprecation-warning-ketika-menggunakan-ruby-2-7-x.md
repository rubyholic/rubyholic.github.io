---
layout: post
title: Menghilangkan Deprecation Warning Ketika Menggunakan Ruby 2.7.x
date: 2020-04-02 21:59 +0700
---

Sewaktu menulis tentang [rbenv], saya menyarankan agar kalian menggunakan ruby terbaru. Untuk saat ini April 2020 versi terbaru dari ruby ialah 2.7.0.

Pada saat kalian memasang Rails di ruby 2.7.0, ketika dijalankan. Kalian akan mendapatkan pesan peringatan `deprecation` (kode sudah usang). Memang peringatan tersebut tidak mempengaruhi aplikasi. Rails tetap berjalan dengan normal.

Namun saya rasa kalian akan merasa jengkel karena peringatan kode usangnya sangatlah banyak. Sejujurnya saya sendiri juga jengkel.

Untuk mengatasi masalah tersebut, ternyata caranya cukup mudah kita hanya perlu menambahkan perintah berikut:

```bash
RUBYOPT='-W:no-deprecated -W:no-experimental'
```

Diawal perintah `rails`. Contohnya begini. Kalau kalian ingin menjalankan `rails s` cukup tambahin local variable di atas. Jadi seperti berikut:

```bash
RUBYOPT='-W:no-deprecated -W:no-experimental' rails s
```

Gampang kan?. Biar lebih gampang lagi kalian bisa `export` variable di atas dan menarunya di `.bashrc`.

```bash
export RUBYOPT='-W:no-deprecated -W:no-experimental'
```

Selanjutnya kalian bisa langsung menjalankan Rails tanpa ada deprecation warning lagi :)

[rbenv]: https://rubyholic.github.io/install-rbenv
