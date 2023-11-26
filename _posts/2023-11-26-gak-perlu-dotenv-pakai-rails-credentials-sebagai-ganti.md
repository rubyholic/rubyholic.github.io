---
layout: post
title: Gak Perlu Dotenv Pakai Rails Credentials Sebagai Ganti
date: 2023-11-26 07:23 +0700
---

Sebetulnya kalau pakai Rails kita udah gak perlu lagi pake Dotenv. Karena Rails sendiri menyediakan credentials generator yang jauh lebih keren dibandingkan dotenv.

## Apa itu Rails Credentials

Rails credentials adalah tool bawaan rails untuk membuat file config berformat Yaml yang ter-encrypt.

Karena ter-encrypt pasti punya kuncinya. Nah kunci ini terbentuk pada saat kamu generate dengan nama file `master.key` terletak di direktori `config`.

Isi kuncinya:

```bash
cat config/master.key
```

Jadi yang perlu kamu lakukan terhadap kunci tersebut adalah simpan baik-baik jangan sampai hilang atau kecolong.

Keungulan Rails Credentials dibandingkan dotenv yakni file confignya ter-encrypt sehingga gak sembarang orang bisa liat. Di sisi lain, Rails Credentials udah ada sejak versi 4, jadi kita gak perlu lagi pasang `gem` tambahan.

## Cara Pakai

Kamu cukup ketik perintah berikut:

```bash
bundle exec rails credentials:edit
```

Lalu taru isi credentials-nya pakai format Yaml. contoh:

```yml
google_api: anu-key-nya-google
```

Kemudian untuk aksesnya cukup pakai method.

```ruby
Rails.application.credentials.google_api
```

Perintah di atas sudah cukup buat kamu yang baru buat project Rails dan variabel pada terminal kamu sudah terpenuhi.

Kalau belum? Set dulu terminal variablenya.

## Set $EDITOR atau $VISUAL variable

Buat variable `$EDITOR` dan `$VISUAL` variable di `.bashrc` atau `.zshrc` mu.

Isi variable tersebut mengikuti editor yang biasa kamu pakai contoh vscode atau vim.

### Vim

```bash
echo "export EDITOR=vim" >> ~/.bashrc
echo "export VISUAL=$EDITOR" >> ~/.bashrc
source ~/.bashrc
```

### VSCode

```bash
echo "export EDITOR='code --wait'" >> ~/.bashrc
echo "export VISUAL=$EDITOR" >> ~/.bashrc
source ~/.bashrc
```

Nah kalau sudah bisa langsung lakukan edit credentials seperti command sebelumnya.

```bash
bundle exec rails credentials:edit
```

## Tambahan

### Environment

Perlu diketahui bahwa pembuatan credentials di Rails kamu bisa set berdasarkan tiga environment yakni `development`, `test` dan `production`.

Perintahnya seperti ini:

```bash
bundle exec rails credentials:edit --environment development
```

### Cara Akses

Untuk akses variable jika cuma satu level kamu bisa menggunakan `method`. Kalau dua atau lebih wajib gunakan kurung dengan symbol.

Contoh satu level

```ruby
Rails.application.credentials.google_api
```

Contoh dua level

```ruby
Rails.application.credentials[:google][:api]
```

atau bisa pakai `dig`

```ruby
Rails.application.credentials.dig(:google, :api)
```

## Akan terjadi Error

Untuk existing project mungkin akan mengalami error, kamu akan diberi peringatan bahwa `key` yang kamu gunakan untuk meng-generate credentials salah, meskipun kamu belum pernah sama sekali menggunakan Rails Credentials ini.

Kenapa bisa begitu? Karena `master.key` yang kamu gunakan sudah tidak berlaku pada saat pertama kali generate project rails atau pada saat kamu pindah komputer atau pada saat kamu baru meng-cloning repository project.

### Cara Mengatasinya

Cara mengatasinya paling gampang kamu hapus `master.key` dan semua file confignya.

```bash
rm -f config/master.key
rm -f config/credentials*.enc
```

Tapi cara di atas tidak disarankan karena kamu akan menulis ulang semua config dari project mu. Cara lainya kamu bisa tanya ke senior yang buat project itu.

Jika dia tidak tau baru kamu perlu bikin ulang dengan menghapusnya. Kalau, dia tau kamu perlu mengganti saja isi `master.key` dengan keynya.

```bash
vim config/master.key
```

Atau bisa bypass dengan environment `RAILS_MASTER_KEY`

```bash
RAILS_MASTER_KEY=anu-keynya bundle exec rails credentials:edit
```

Sip sampai di sini dulu ya, moga bermanfaat hehehe 😅
