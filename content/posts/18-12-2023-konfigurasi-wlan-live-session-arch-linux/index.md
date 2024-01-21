---
title: "Konfigurasi WLAN di Live Session Arch Linux"
date: 2023-12-18
description: "Bagian dari \"Instalasi Arch Linux\", menjelaskan bagaimana cara terhubung ke internet melalui Wi-Fi di Live Session Arch Linux."
tags: [archlinux, linux, teknologi, software]
---

Cek nama device kamu dengan `ip link`, device wireless card biasanya punya nama berawalan “w”, misal. `wlan0`, `wlp3s0`. Hidupkan device tersebut dengan `ip link set nama_device up`.

```plain
> ip link
> ip link set nama_device up
```

## Rfkill

jalankan `rfkill` untuk mengecek status block wireless card, jika wireless card gagal hidup, maka ada dua kemungkinan soft-block atau hard-block.

- hard-block : cek apakah laptop kamu memiliki switch fisik untuk hidup-matikan wi-fi

- soft-blok : jalankan `rfkill unblock wlan` untuk unblock wi-fi
```plain
> rfkill unblock wlan
```

## Iwctl

Gunakan `iwctl` untuk masuk ke mode shell interaktif iwd, lihat prompt kamu akan berubah menjadi `[iwd]#` maka kamu berada dalam shell interaktif iwd.

```plain
[iwd]# station nama_device scan

[iwd]# station nama_device get-networks

[iwd]# station nama_device connect nama_ssid_jaringan_wifi
```
Keluar dari iwd jika berhasil tersambung.

## DHCP

Satu langkah lagi, jalankan `dhcpcd nama_device` untuk mendapatkan IP DHCP. Cek internet kamu dengan `ping 8.8.8.8`

```plain
> dhcpcd nama_device
> ping 8.8.8.8
```