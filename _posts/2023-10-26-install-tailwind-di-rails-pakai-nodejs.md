---
layout: post
title: Install Tailwind di Rails Pakai NodeJS
date: 2023-10-26 22:23 +0700
tags:
    - rails
    - nodejs
    - rails7
---

Sebetulnya di Rails 7 kalau kita mau pakai Tailwind atau css framework lainnya sudah dipermudah. Apalagi Tailwind sekarang sudah punya binary tersendiri yang mana dengannya kita gak perlu lagi pasang NodeJS buat mengejalain Tailwind.

Nah, kenapa kalau udah ada binary-nya saya malah tetep pakai NodeJS? Simpelnya adalah biar kompabilitasnya lebih baik dan biar sekalian kalau mau pake package node modules lainnya buat kebutuhan frontend saya gak ribet, tinggal `pnpm add anu` dan semacamnya.

Oke langsung aja ya mulai. Sebelum mulai perhatikan deh saya pake `pnpm` bukan `npm` atau `yarn` ya, alasanya apa? Kekinian wkwkwkw.

Kalau kamu belum ada bisa langsung pasang aja.

```bash
npm -g pnpm
```

Beres masalah pnpm, langsung install Tailwindnya ye...

```bash
pnpm add tailwindcss
```

Buat config tailwindnya.

```bash
npx tailwindcss init
```

Dan edit file `tailwindcss.config.js` pada bagian content kira-kira jadi seperti ini.

```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: [
    './public/*.html',
    './app/**/*.{erb,haml,html,slim}'
  ],
  theme: {},
  plugins: [],
}
```

Lalu tambahin script tailwindnya diberkas `application.css` di `app/assets/stylesheets`

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

Edit lagi di bagian layout application kira-kira nanti jadi kaya gini

```erb
<!DOCTYPE html>
<html>
  <head>
    <title>Hore</title>
    <meta name="viewport" content="width=device-width,initial-scale=1">
    <%= csrf_meta_tags %>
    <%= csp_meta_tag %>
    <%= stylesheet_link_tag "tailwind", "data-turbo-track": "reload" %>
    <%= javascript_importmap_tags %>
  </head>
  <body>
    <%= yield %>
  </body>
</html>
```

Nah sebetulnya itu sudah bisa jalan pak haji, dengan perintah gini:

```bash
npx tailwindcss -i app/assets/stylesheets/application.css -o app/assets/stylesheets/tailwind.css --watch
```

Tapi itu cuma buat jalanin Tailwindnye aje, belum termasuk Railsnya Pak Haji.. Terus gimana nih? Biar jalan barengan?

Simpelnya kita bisa pakai `foreman`.

Buat file di root project rails kamu bernama `Procfile.dev`. Lalu isi skripnya berikt:

```
web: env RUBY_DEBUG_OPEN=true bin/rails server -p 3000
css: npx tailwindcss -i app/assets/stylesheets/application.scss -o app/assets/stylesheets/tailwind.css --watch
```

Kemudian buat juga script buat ngejalanin si foreman tadi.

Buat file bernama `dev` di direktori `bin` jadi gini:

```bash
touch bin/dev
chmod +x bin/dev
```

Edit berkas `bin/dev` nya dengan skrip berikut:

```bash
#!/usr/bin/env sh

if ! gem list foreman -i --silent; then
  echo "Installing foreman..."
  gem install foreman
fi

exec foreman start -f Procfile.dev "$@"
```

Nah sekarang tinggal jalanin deh Rails kamu seperti ini

```bash
./bin/dev
```

Hohoho Tailwind udah bisa dipake ya ...