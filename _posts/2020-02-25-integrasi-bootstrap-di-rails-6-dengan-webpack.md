---
layout: post
title: Integrasi Bootstrap di Rails 6 dengan Webpack
date: 2020-02-25 14:34 +0700
categories: rails
tags: rails6 bootstrap webpack
---

Jika kalian terbiasa dengan css framework seperti `bootstrap` mungkin kalian akan mengintegrasikan pula pada projek `rails` kalian. Kali ini saya ingin berbagi mengenai cara mengintegrasikan `bootstrap` di `rails 6`.

Dikarenakan `rails 6` sudah memakai `webpack` jadi kita dapat dengan mudah memasang `bootstrap` dari `yarn`. Berikut caranya:

```bash
$ yarn add bootstrap jquery popper.js
```

Kemudian tambahkan skrip di bawah ini di berkas `config/webpack/environment.js` :

```js
const { environment } = require('@rails/webpacker')

const webpack = require('webpack')
environment.plugins.append('Provide',
    new webpack.ProvidePlugin({
	$: 'jquery',
	jQuery: 'jquery',
	Popper: ['popper.js', 'default']
    })
)

module.exports = environment

```
Kemudian tambahkan juga skrip di bawah ini pada berkas `app/javascript/packs/application.js` :

```js
import 'bootstrap'
import './src/application.scss'
```
Selanjutnya buat direktori:

```bash
$ mkdir app/javascript/packs/src
$ cd app/javascript/packs/src
```
Dan buat berkas di dalamnya bernama `application.scss`

```bash
$ touch application.scss
$ vim application.scss
```
Kemudian isi dengan skrip di bawah ini:

```scss
@import '~bootstrap/scss/bootstrap';
```

Langkah terakhir:

```bash
$ rails assets:precompile
$ rails webpacker:compile
```
Sekarang `rails` kalian sudah terintegrasi dengan `bootstrap`
