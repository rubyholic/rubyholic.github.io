---
layout: post
title:  "Install Rbenv"
date:   2020-01-31 21:47:37 +0700
categories: rbenv
---
Sebelum memulai belajar pemrograman ruby. Langkah pertama tentunya ialah memasang ruby di sistem operasi. Mungkin beberapa sistem operasi sudah terpasang secara asali (default). Contohnya pengguna macOS. Bagi yang belum ada kalian bisa pasang sesuai dengan sistem operasi yang kalian pergunakan.

Untuk mengetahui kalian sudah memasang ruby atau belum, kalian bisa cek di Terminal Emulator dengan perintah berikut:

```shell
$ ruby -v
```

Jika berhasil maka akan keluar informasi seperti ini:

```shell
ruby 2.7.0p0 (2019-12-25 revision 647ee6f091) [x86_64-linux]
```

Kalau tidak berarti kalian perlu memasangnya.

Untuk memasang ruby, saya menyarankan kalian pergunakan `rbenv` mengapa? Karena untuk menyamakan versi ruby yang dipakai. Biasanya sistem operasi hanya menyediakan versi ruby tertentu, yang biasanya hanya pada versi ruby lawas. Sehingga ada beberapa method tidak terdapat dalam versi tersebut. Nah dengan rbenv inilah kalian dapat mengontrol versi ruby mana yang akan kalian pakai nantinya.

## Apa itu rbenv?

Rbenv adalah suatu aplikasi yang memiliki tugas mengatur berbagai macam versi ruby dalam satu lingkungan sistem operasi. Dengan adanya rbenv memungkinkan kalian dapat memasang multi versi dari ruby tanpa harus khawatir bentrok dengan dependensi.

## Mengapa Perlu rbenv?

Memaasang ruby dari bawaan manajer paket dari sistem operasi. Biasanya kita hanya akan mendapatkan versi stable-nya saja. Versi stable biasanya versi lawas. Memang bagus dengan versi stable. Akan tetapi, jika kalian memiliki rasa penasaran dengan versi teranyar karena ingin tahu beberapa perubahan maka disitulah gunanya rbenv.

## Cara Pasang rbenv

Sebelum memasang rbenv pastikan kalian sudah memasang `git`. Jika sudah, langsung saja:

1. Klon repositori rbenv dengan mengikuti perintah berikut:
    ```shell
    $ git clone https://github.com/rbenv/rbenv.git ~/.rbenv
    ```
2. Set rbenv di `$PATH` variabel. Seperti berikut:

   Bagi pengguna **bash**
   ```shell
   $ echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
   ```
   Bagi pengguna **zsh**
   ```shell
   $ echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.zshrc
   ```
3. Set up rbenv di shell kalian.
   ```shell
   $ ~/.rbenv/bin/rbenv init
   ```
   Maka akan keluar informasi berikut:
   ```shell
    # Load rbenv automatically by appending
    # the following to ~/.bashrc:

    eval "$(rbenv init -)"
    ```
    Nah, tinggal masukan `eval "$(rbenv init -)"` di `.bashrc`
4. Restart terminal kalian dengan cara tutup lalu buka kembali.
5. Pasang plugin `ruby-build` yang berguna untuk mengkompil kode sumber ruby.
   ```shell
   $ mkdir -p "$(rbenv root)"/plugins
   $ git clone https://github.com/rbenv/ruby-build.git "$(rbenv root)"/plugins/ruby-build
   ```
6. Cek apakah kalian sudah benar memasang rbenv?
   ```shell
   $ curl -fsSL https://github.com/rbenv/rbenv-installer/raw/master/bin/rbenv-doctor | bash
   ```
   Outpunya:
   ```shell
    Checking for `rbenv' in PATH: /home/ali/.rbenv/bin/rbenv
    Checking for rbenv shims in PATH: OK
    Checking `rbenv install' support: /home/ali/.rbenv/plugins/ruby-build/bin/rbenv-install (ruby-build 20200115-1-g4082f9e)
    Counting installed Ruby versions: none
      There aren't any Ruby versions installed under `~/.rbenv/versions'.
      You can install Ruby versions like so: rbenv install 2.2.4
    Checking RubyGems settings: OK
    Auditing installed plugins: OK
    ```

## Memasang Ruby

Pemasangan rbenv sudah selesai langkah selanjutnya adalah kita pasang ruby. Adapun cara pasangnya sangatlah mudah, kalian hanya perlu melakukan perintah berikut:
```shell
$ rbenv install 2.7.0
```

Jika kalian ingin versi ruby yang lainnya, kalian bisa pasang dengan melihat versi yang kalian mau. Berikut caranya:
```shell
$ rbenv install --list
```

## Set Ruby Global

Langkah terakhir adalah menjadikan ruby yang tadi kita telah pasang dapat diakses di mana saja.
```shell
$ rbenv global 2.7.0
```

Untuk memastikan bisa kalian tes dengan perintah berikut:
```shell
$ ruby -v
ruby 2.7.0p0 (2019-12-25 revision 647ee6f091) [x86_64-linux]
```
## Set Ruby Local Directory

Jika kalian hanya ingin versi ruby lainnya di direktori tertentu. Contoh, saya punya 2 ruby versi 2.7.0 dan 2.6.5. Saya ingin di salah satu direktori projek saya menggunakan ruby 2.6.5 sedangkan yang lain tetap menggunakan ruby 2.7.0. Nah berikut caranya:
```shell
$ cd /my/ruby/project/
$ rbenv local 2.6.5
$ ruby -v
ruby 2.6.5p114 (2019-10-01 revision 67812) [x86_64-linux]
$ cd ~
$ ruby -v
ruby 2.7.0p0 (2019-12-25 revision 647ee6f091) [x86_64-linux]
```
Lihat versi di direktori `/my/ruby/project` menggunakan versi 2.6.5 sedangkan versi globalnya menggunakan 2.7.0

