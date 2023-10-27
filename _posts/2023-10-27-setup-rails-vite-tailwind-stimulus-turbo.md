---
layout: post
title: Setup Rails + Vite + Tailwind + Stimulus + Turbo
date: 2023-10-27 22:16 +0700
tags:
 - vite
 - tailwind
 - stimulus
 - turbo
---

Kemaren saya bilang klo untuk urusan frontend saya lebih seneng pasang pakai bantuan NodeJS, dibandingkan install paket yang sudah tercompile menjadi css atau js.

Sewaktu pakai Rails 6 kita udah dipasangkan pake Webpack secara build-in. Nah sejak di Rails 7 Webpack gak lagi jadi satu, kita mesti pasang lagi. Justru bagus sih gitu, dengan demikian kita bisa bebas gak mesti pakai Webpack juga.

Nah, salah satu hal yang paling saya suka nih waktu mainan frontend yakni pakai Vite. Sekarang mau gabungin Vite di Rails. Menariknya Vite punya gem khusus Rails, jadi pasangnya gak terlalu ribet.

Ya sudah langsung kita mulai saja. Asumsi saya, kamu sudah punya project Rails ya.


## 1. Setup Vite

```bash
bundle add vite_rails
bundle exec vite install
```

Pasang vite plugin untuk Rails:

```bash
pnpm add vite-plugin-rails stimulus-vite-helpers
```

Edit `vite.config.ts` menjadi seperti ini

```js
import { defineConfig } from "vite";
import RubyPlugin from "vite-plugin-rails";

export default defineConfig({
  plugins: [RubyPlugin()],
});
```

Edit layout di `apps/views/layout/application.html.erb`

```html
<head>
  <title>Example</title>
  <%= csrf_meta_tags %>
  <%= csp_meta_tag %>

  <%= vite_client_tag %>
  <%= vite_javascript_tag 'application' %>
  <%= vite_stylesheet_tag 'application', 'data-turbo-track': 'reload' %>
</head>
```

Buat bekas di `apps/javascript/entrypoints/application.css`

```bash
touch apps/javascript/entrypoints/application.css
```

Lalu isi:

```css
@import '../../assets/stylesheets/application.css'
```

## 2. Pasang Tailwind, Turbo dan Stimulus

```bash
pnpm add tailwindcss autoprefixer postcss @hotwired/turbo @hotwired/stimulus
```

## 3. Setup Tailwind

```bash
npx tailwindcss init
```

Edit `tailwind.config.js`

```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: ["./app/**/*.{html,erb,slim}"],
  theme: {
    extend: {},
  },
  plugins: [],
};
```

Edit diberkas `app/assets/stylesheets/application.css`

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

Buat berkas untuk pengaturan postcss:

```bash
touch postcss.config.ts
```

Lalu isi

```js
module.exports = {
  plugins: {
    tailwindcss: {},
    autoprefixer: {},
  }
}
```

## 4. Setup Turbo

Edit di `apps/javascript/entrypoints/application.js`

Tambahin skrip berikut:

```js
import * as Turbo from "@hotwired/turbo";
Turbo.start();
```

## 5. Setup Stimulus

Edit di `apps/javascript/entrypoints/application.js`

Tambahin skrip berikut:

```js
import "../controllers";
```

Edit lagi diberkas `app/javascript/controllers/index.js`

Ganti isinya menjadi seperti berikut:

```js
import { Application } from "@hotwired/stimulus";
import { registerControllers } from "stimulus-vite-helpers";

const application = Application.start();
const controllers = import.meta.globEager("./**/*_controller.js");
registerControllers(application, controllers);
```

## 6. Buat bekas bin/dev (jika udah ada, gak perlu bikin)

```bash
touch bin/dev
chmod +x bin/dev
```

Edit `bin/dev` isi kaya berikut:

```bash
#!/usr/bin/env sh

if ! gem list foreman -i --silent; then
  echo "Installing foreman..."
  gem install foreman
fi

exec foreman start -f Procfile.dev "$@"
```

Pastikan `Procfile.dev` nya seperti berikut:

```
vite: bin/vite dev
web: bin/rails s
```

# Testing

Nah kalau sudah selesai semua setupnya. Berarti kita mesti test apakah setupan kita udah berhasil atau belum. Caranya gampang edit salah satu view kamu dan isi dengan semua attribut Tailwind dan Stimulus.

Contoh

```html
<p class="text-2xl text-bold">
  Halo Semua ...
  <span data-controller="hello"></span>
</p>
```

Kalau berhasil semestinya tag `span` nya terisi dengan kata-kata dari controller `hello` dari Stimulus.

Lalu untuk Turbo nya? Gampang buat saja salah satu navigasi antar page, Turbo bisanya punya preload (seperti ada animasi loading) sebelum pindah halaman. Kalau gak ada preload-nya berarti Turbonya belum jalan nih wkwkw

Sip, semoga tulisan ini bermanfaat.