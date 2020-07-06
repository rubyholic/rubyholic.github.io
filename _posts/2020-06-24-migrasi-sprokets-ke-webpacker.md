---
layout: post
title: Migrasi Sprockets ke Webpacker
date: 2020-06-24 15:44 +0700
categories: rails
---

Saya senang sekali webpacker telah hadir secara default di rails 6 sebagai penanganan web assets. Dengan hadirnya webpacker kita bisa menggeser untuk tidak menggunakan sprockets. Meski demikian, kita bisa menggunakan keduanya secara bersamaan.

Ketika kalian menggunakan rails 6 dan membuat project dengan menggunakan perintah:

```
rails new your_project
```

Perintah di atas, sudah menyertakan webpacker sebagai penangan `js` dan sprockets sebagai penanganan `css` dan `image`.

Bagi saya cukup boros mengunakan dua engine untuk menangani masalah assets. Padahal cukup menggunakan webpacker, itu sudah bisa semua.

Kalau kalian baru memulai membuat project di rails 6. Saya menyarankan gunakan perintah:

```
rails new your_project --skip-sprockets
```

Bagimana yang sudah terlanjur menggunakan dua engine webpacker dan sprockets di rails? Atau tidak menggunakan rails 6 namun ingin pakai webpacker?

Nah, tulisan inilah saya mencoba berbagi berdasarkan pengalaman saat migrasi dari sprockets menuju webpacker.

## Penggunan Rails 5 atau ke bawah

Di mulai dari para penggunakan rails <= 5. Pertama-tama kalian pasang dulu gem webpacker di GemFile.

```
gem 'webpacker'
```

Lalu pasang webpacker tersebut pada rails.

```
bundle
rails webpacker:install
```

Kemudian kalian ubah skrip `javascript_include_tag` menjadi `javascript_pack_tag` di `view/layouts`. Begitu juga untuk `stylesheet_include_tag` ganti menjadi `stylesheet_pack_tag`.o

```
- javascript_include_tag
+ javascript_pack_tag

- stylesheet_include_tag
+ stylesheet_pack_tag
```

Dengan mengubah skrip di atas, seharusnya tampilan web kalian menjadi berantakan. Karena css dan js bawaan sprockets tidak di load.

Terakhir, buat berkas `application.js` di direktori `app/javascript/packs`.

```bash
mkdir -p app/javascript/packs
touch application.js
```

Susunan berkasnya seperti berikut:

```
app/javascript:
 └── packs:
   └── application.js
```

## Test Pastikan Webpacker sudah berjalan

Nah untuk memastikan webpacker sudah berjalan atau belum kalian bisa mengetesnya dengan skrip sederhana berikut yang terletak pada `app/javascript/packs/application.js`:

```js
console.log('Hello world from Webpacker');
```

Kalau sudah terlihat di browser, selamat webpacker sudah terpasang baik di rails kalian!

## Konfigurasi Webpacker

Sebelum bermigrasi, adakalanya kita mesti konfigurasi dulu webpacker-nya.

Langkah pertama ialah, rename direktori `app/javascript` menjadi `app/webpacker`. Karena kita ingin semua assets ditangani oleh webpacker maka direktori javascript diubah menjadi webpacker.

Sebetulnya, tidak di-rename juga tidak apa-apa. Namun menurut saya itu tidak bagus. Karena di dalam direktori tersubut, nantinya kita juga akan memasukan css, images, fonts, dan berkas aset lainnya selain js. Makanya lebih baik diganti namanya menjadi webpacker.

Untuk mengubahnya kalian ubah pada berkas `config/webpacker.yml`

```bash
vim config/webpacker.yml
```

```
sourch_path: app/webpacker
extract_css: true
```

Setelah itu buat direktori seperti berikut:

```
app/webpacker:
├── fonts
├── images
├── packs
│  └── application.js
└── src
   ├── javascripts
   └── stylesheets
```

dengan perintah:

```bash
mkdir -p {fonts,images,src/javascripts,src/stylessheets}
```

Dengan susunan direktori seperti di atas. Kalian dapat memindahkan semua css/scss ke `app/webpacker/src/stylessheets` begitu juga dengan js, pindahkan semua ke `app/webpacker/src/javascripts`

## Migrasi CSS/SCSS

Setelah melakukan konfigurasi pada webpacker. Langkah selanjutnya kita pindahkan semua css/scss ke `app/webpacker/src/stylessheets`.

Jika sudah, kita import css/scss terusebut di `packs/application.js`

```bash
vim app/webpacker/packs/application.js
```

```js
import "bulma"
import "../src/stylessheets/my_css"
```

Tanpa perlu extention, webpacker sudah langsung otomatis tau.

Kalian bisa perhatikan cara import baris pertama dan kedua berbeda. Mengapa bisa beda? Karena baris pertama di dapat dari `node_modules` yakni dengan perintah:

```bash
yarn add bulma
```

### Menggunakan application.scss (Optional)

Cara di atas, kita meng-import berkas stylesheets dari js. Sebetulnya kita juga bisa menggunakan berkas di `packs/application.scss`.

Caranya pun hampir sama. Pertama buat dulu berkasnya.

```bash
touch app/webpacker/packs/application.scss
```

Lalu kalian tinggal import seperti ini:

```scss
@import '~bulma/scss/bulma';
@import '../src/stylesheets/my_css';
```
Nah untuk baris pertama, caranya beda lagi. Kalau kita ingin import dari `node_modules` di scss, ditambah tilda `~` untuk meng-import berkas-berkas yang bersumber dari `node_modules`.

## Migrasi Javascript

Setelah semua stylesheet dipindakan. Selanjutnya berkas js juga dipindahkan.

Nah, caranya juga sama. Tinggal import lagi di `packs/application.js`

```bash
vim app/webpacker/packs/application.js
```

```js
import "bulma"
import "../src/javascript/my_script"
```

## Migrasi Images

Untuk berkas gambar. Kalian masukan di `webpacker/images`.

Dan sunting `packs/application.js` nya menjadi

```js
require.context('../images', true);
```

Jangan lupa. Ganti semua skrip yang menggunakan `asset_path` menjadi `image_pack_path`.


```
 - asset_path('icon.png')
 + image_pack_path('icon.png')
```

Jika dalam berkas stylesheet (css/scss) kalian ada yang meng-import gambar. Lakukan seperti contoh di bawah:

```scss
  .google-icon {
-   background-image: image-url('btn_google_dark.svg');
+   background-image: url('../images/btn_google_dark.svg');
  }
```

Tanda minus berarti diapus dan tanda plus berarti digantikan.

## Migrasi Fonts

Nah ini bagian terakhir dari sesi pemindahan. Sama seperti diberkas lainnya. Kalian mesti memindahkannya di `app/webpacker/fonts`

Dan jangan lupa ganti discript stylesheets kalian:

```scss
@font-face {
 font-family: YourFontFamily;
 src: url('../fonts/yourfile.ttf') format('truetype');
}
```

## Hapus Sprockets Hingga Keakar-akarnya!

Kalian sudah memindahkan seluruh assets ke webpacker. Namun, itu semua belum sepenuhnya migrasi. Kita masih tetap bisa menggunakan sprockets.

Kalau kalian ingin menghapus hingga keakar-akarnya. Kalian perlu melakukan ini:

1. Buang gem `sass-rails`.

    Hapus atau beri comment `sass-rails` dari GemFile

    ```rb
    # gem 'sass-rails'
    ```

    ```bash
    bundle
    ```

2. Hapus `require "sprockets/railtie"` dari `config/application.rb`.

    ```rb
    # require "sprockets/railtie"
    ```

3. Hapus `config.assets.*` dari dua file `development.rb` dan `production.rb` dari `config/environment`.

    ```bash
    cd config/environment
    vim development.rb
    vim production.rb
    ```

    Kode yang dihapus

    ```
    config.assets.debug = true
    config.assets.quiet = true
    ```

4. Hapus file `config/initializers/assets.rb`.
5. (Optional) Hapus direktori `app/assets`.

## Test it!

Langkah terakhir, yakni tes! Kalian bisa lakukan perintah berikut:

```bash
rails assets:precompile
```

Kalau tidak terjadi error berarti kalian sudah sukses mengapus sprockets dari rails kalian!

Semoga bermanfaat :)




