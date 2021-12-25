---
layout: post
title: Termux Baru Masalah Baru Buat Ruby
date: 2021-12-25 13:26 +0700
tags:
  - ruby
  - termux
  - curhat
---

Jadi kemarin niatnya kan saya mau bersih-bersih isi Termux, karena dirasa udah banyak sampah _bejibun_ di sana. Maklum dulu buat coba-coba dan udah 2 tahun gak diutak-atik itu Termux.

Biasalah, kalau di Android cara paling gampang bersih-bersih ya lewat `clear cache` dari application info.

Namun masalah baru timbul setelah `clear cache`. Pertama, saya baru tau kalau Termux di PlayStore udah deprecated sejak tanggal 20 November 2020.

Jadi kalau kita mau pake versi terbaru cuma ada di F-Droid. Alasannya karena Android 10 mewajibkan aplikasinya memakai SDK minimal versi 29.

Intinya sih, karena SDK baru bikin ngebreak si Termux shellnya jadi gak jalan. Makanya gak dilanjutin pengembangannya di PlayStore. Ente bisa baca-baca di sini <https://www.xda-developers.com/termux-terminal-linux-google-play-updates-stopped/>

Padahal waktu di versi PlayStore saya udah beli `Termux Styling` eh sekarang malah gak bisa jalan, rugi deh wkwkwkw.

Gak juga sih, saya beli karena emang support developernya. Soalnya adanya Termux, saya bener-bener terbantu.

Akhirnya saya pasang Termux dari F-Droid. Di sana juga udah ada Termux Styling gratis pula, jadi bisa langsung pake.

Kenapa sih saya butuh Termux Styling? Krena saya butuh warna-warna biar keliatan gak kek heker gitu yg cuma sukanya item-putih doang wkwkwk.

Juga saya butuh font `JetBrains Mono`, ini font biasa saya pake di desktop, cakep dan support ligatures. Jadi tetep keren meskipun ngoding di Neovim, gak kalah deh sama IDE hehehe.

Bisa aja sih tanpa Termux Styling, toh cuma edit konfigurasinya di directory `~/.termux`. Tapi saya males, pake aja yang udah mudah wkwkwk.

Nah Termux udah dipasang udah saya setup juga sesuai dengan konfigurasi yang biasa saya pake buat ngoding.

Biasa deh cuma masang, `tmux`, `neovim`, `git`, `openssh`, `build-essetial`, `rbenv`, `nodejs`, dan `postgresql`. Kamaren-kemaren saya tambahin juga `php` dan `mariadb` sih, cuma sekarang enggak. Termux saya pake buat ngoding ruby aja.

Dari sini semua gak ada masalah, namun muncul ketika saya meng-compile Ruby 3 dari rbenv. Yakni error gak bisa dicompile. Gak tau ini masalahnya di mana, padahal dependensi sudah saya install.

Sebelum di clear cache itu Termux bisa Ruby 3 di pasang, kenapa sekarang malah jadi gak bisa wkwkkw. Saya pikir ini masalah di rbenv. Yaudah saya coba pindah ke `rvm`.

Kenapa pindah ke rvm? Biasanya rvm nyediain versi binarynya, kalaupun gak ada ia otomatis compile dari source Ruby dengan dipasangin dependensinya otomatis.

Tapinya tetep gak bisa lewat rvm, baru mau download dependensi udah ditolak.

Oh iya, Termux yang baru juga sudah menghilangkan paket-paket yang berakhiran `-dev`. Semua dijadiin satu dengan paketnya. Contoh kamu mau install `libssl`, biasanya ada dipake `libssl-dev` tapi sekarang cukup pasang `libssl` aja karena versi devnya sudah termasuk.

Beruntung kebetulan ruby versi 3 ada direpository Termux, saya pasang ruby dari repo aja gak dari rbenv atau rvm. Yuhuuu.. senengnya ...

Tapi kesenangan berakhir, masalah baru lagi muncul pas saya mau pasang Rails dan Jekyll, yakni gak bisa compile gems `saasc`, `nokogiri`, dan lain-lain pokoknya gems yang pake compile dah. Semuanya gak bisa dicompile.

Hemat saya ini bukan karena dependensinya gak ada, tapi karena pembatasan dari Android 10. Haduh, kecewa deh.

Tapi alhamdulillah nih, masih ada aplikasi `AnLinux`, yakni GNU/Linux minimal yang bisa dipasang di Termux.
Deskripsi aplikasinya `Run Linux on Android Without Root Access`.

Keliatanya asik nih, saya emang males banget ngeroot HP, soalnya ribet tar sama aplikasi banking wkwkwk.

AnLinux isinya cuma bash script, yang mana ia nge-`chroot` si rootfs nya Linux. Jadi Linux versi arm, dijalanin dari Termux. Bagus sih, intinya masih ada harapan buat saya pasang kebutuh ngoding Ruby di Termux.

Di AnLinux kita bisa pilih berbagai macam distro GNU/Linux yang nanti mau dijalanin. Saya pasang Ubuntu aja biar gak ribet. Tadinya mau pasang ArchLinux tapi nanti aja deh. Yang penting kebutuhan ngoding ruby saya lancar dulu.

Alhamdulilah berkat adanya AnLinux semuanya udah beres sekarang. Jadi bisa ngoding lagi deh wkwkwkw
