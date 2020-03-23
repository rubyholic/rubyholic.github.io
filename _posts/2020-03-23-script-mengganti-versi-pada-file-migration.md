---
layout: post
title: Script Mengganti Versi pada File Migration
date: 2020-03-23 15:04 +0700
---

Ketika upgrade Rails, saya melakukan drop database. Tujuannya untuk memastikan seluruhnya berjalan dengan baik, jika aplikasi saya di setup dari awal. Yang terjadi adalah tidak saya mengalami error seperti berikut:

```
StandardError: An error has occurred, this and all later migrations canceled:

Directly inheriting from ActiveRecord::Migration is not supported. Please specify the Rails release the migration was written for:

  class RolifyCreateRoles < ActiveRecord::Migration[4.2]

```

Skripnya sebagai berikut:

## Khusus Pengguna GNU/Linux

```bash
grep -rl "ActiveRecord::Migration$" db | xargs sed -i 's/ActiveRecord::Migration/ActiveRecord::Migration[5.1]/g'
```

## Pengguna macOS

```bash
grep -rl "ActiveRecord::Migration$" db | xargs sed -i "" "s/ActiveRecord::Migration/ActiveRecord::Migration[5.1]/g"
```

## Sumber
<https://github.com/RolifyCommunity/rolify/issues/444>
