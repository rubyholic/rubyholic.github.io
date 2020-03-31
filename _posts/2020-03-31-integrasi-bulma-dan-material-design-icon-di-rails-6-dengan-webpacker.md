---
layout: post
title: Integrasi Bulma dan Material Design Icon di Rails 6 dengan Webpacker
date: 2020-03-31 11:04 +0700
tags: rails6
---

Bulma merupakan salah satu CSS fremework yang sedang naik daun saat ini. Salah satu yang saya suka dari Bulma adalah ia hanya CSS tanpa JS, berbeda dengan Bootstrap yang memerlukan JS (jQuery, popper.js). Karena Bulma hanya CSS otomatis cara integrasi di Rails 6 dengan Webpacker sangatlah mudah.

Berikut ini cara integrasi Bulma di Rails 6 dengan Webpacker:

## Bulma

Pertama-tama tambahkan dulu Bulma dengan yarn.

```bash
yarn add bulma
```

Kemudian tinggal kita import dengan edit pada file `app/javascript/packs/application.js` lalu isi dengan skrip berikut

```js
import "bulma"
```

Selesai kalian tinggal compile assets nya dengan perintah berikut:

```bash
rails assets:precompile
```

Untuk mengetes Bulma suda berjalan di Rails, kalian tinggal coba ambil sample code di dokumentasi Bulma.

## Material Design Icon

Nah, kalau Material Design Icon atau disingkat dengan MDI sama seperti Fontawesome yakni web font. Integrasi MDI di Rails juga sangat mudah. Yang pertama dilakukan adalah:

Tambahkan MDI dengan yarn:

```bash
yarn add @mdi/font
```

Lalu jangan lupa import css di `app/javascript/packs/application.js` isi dengan skrip berikut:

```js
import '@mdi/font/scss/materialdesignicons';
```

Seperti biasa jika kita menambahkan assets wajib untuk melakukan precompile di Rails agar assets tersebut dapat dipanggil.

```bash
rails assets:precompile
```

Sekarang kalian tinggal coba di view kalian. Contohnya:

```html
<i class="mdi mdi-account-circle"></i>
```

Semoga bermanfaat :)
