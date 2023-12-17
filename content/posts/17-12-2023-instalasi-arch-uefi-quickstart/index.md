---
title: "Instalasi Arch Linux (No-BS) /w UEFI"
date: 2023-12-17T19:34:51+0700
description: "Instalasi Arch Linux di Device UEFI/EFI/GPT Tanpa BS. Instalasi menggunakan kabel LAN dengan grub, networkmanager, dan gnome sebagai software tambahan saat post-instalasi."
tags: [linux, tech, software]

resources:
    - name: partition-sizing
      src: "partition-sizing.png"
      title: Contoh partisi 7.5GB
---

Harap membaca persyaratan sebelum memulai tahap Instal Arch Linux.

## Persyaratan

Outline/highlights proses instalasi Arch Linux:

  1. Memiliki akses internet dan [live ISO Arch](https://geo.mirror.pkgbuild.com/iso/2023.12.01/) dalam bentuk bootable USB

  2. Menyediakan [bootable USB](https://rufus.ie/en/), device dan kabel LAN

  3. Membuat device booting melalui bootable USB untuk menuju [live session](https://www.linux.com/training-tutorials/live-booting-linux/)

  4. Menghubungkan live session ke internet menggunakan [kabel LAN](https://patchbox.com/blog/what-is-a-lan-cable/)

  5. Mempersiapkan [partisi](https://www.howtogeek.com/184659/beginner-geek-hard-disk-partitions-explained/) di sesi live session

  6. Proses krusial untuk mengisi partisi dengan [root sistem](https://www.geeksforgeeks.org/linux-directory-structure/) Arch Linux

  7. Post-instalasi, mempersiapkan konfigurasi pengguna dan software tambahan

Spesifikasi device, spesifikasi device yang digunakan penulis saat instalasi Arch Linux:

  - Dell Latitude E7270 dengan firmware motherboard/board UEFI
  - Terhubung ke internet melalui Kabel LAN (10mbps)
  - USB Bootable Arch ISO (SanDisk Cruzer Switch)

Spesifikasi partisi, disk size dan partisi yang akan dibuat selama proses instalasi:

  - `/` Root sebesar 20GB
  - `/boot` EFI Boot sebesar 512MB
  - `swap` Swap sebesar 2GB
  - `/home` Home sebesar yang tersisa
  - Pembagian partisi ini akan mengasumsikan kamu menginstall di drive yang kosong

## Pre-Install Arch Linux

### Memasuki Live Session Arch Linux

1. Colok kabel LAN dan USB ke Device dalam keadaan mati, [pastikan USB ini sudah bootable](https://www.nesabamedia.com/cara-membuat-bootable-flashdisk-windows-10/)

2. Tekan tombol kombinasi untuk [masuk ke memu BIOS](https://id.wikihow.com/Masuk-ke-BIOS) masing masung device, biasanya F1, F2, F8, F12 atau Esc

3. Di menu BIOS, [ubah prioritas pertama booting](https://softwarekeep.com/help-center/how-to-change-your-computers-boot-order) menjadi Bootable USB tersebut

4. Jika tahap diatas benar, maka kamu akan masuk ke live session Arch Linux

## Install Arch Linux

### Terhubung ke Internet

Semenjak kamu menggunakan kabel LAN, maka sudah otomatis terhubung ke jaringan internet. Jika kamu menggunakan wi-fi, lihat artikel ini.

Test respon dengan `ping 8.8.8.8`

### Mempersiapkan Partisi

Cari disk yang ingin di install arch dengan `fdisk -l` atau `lsblk`. Disk biasanya memiliki nama seperti `/dev/sda`, `/dev/vda`, atau `/dev/nvme0n1`. Pada umumnya, jika kamu memiliki SSD atau HDD namanya `sda,sdb,sdc,...`, `vda,vdb,...` untuk disk virtual, dan `nvme0n1,nvme0n2,...` untuk NVMe.

Jalankan perintah dibawah:

```plain
### ganti "/dev/sda" dengan disk yang kamu punya, misal: /dev/vda ###
> cfdisk /dev/sda
```

Akan muncul sebuah GUI, buat partisi mengikuti urutan dan rekomendasi table dibawah:

| mount point | tipe partisi | details | partition target |
|-|-|-|-|
| `/boot` | EFI System | Partisi untuk menyimpan bootloader, disarankan 512MB | `/dev/sda1` |
| `[SWAP]` | Linux Swap | Partisi swap, disarankan 2GB+ | `/dev/sda2` |
| `/` | Linux Filesystem | Partisi root, 20GB | `/dev/sda3` |
| `/home` | Linux Filesystem | Partisi home, Sisanya (tekan `enter`) | `/dev/sda4` |

Jika kamu kebingungan untuk cara memberikan ukuran partisi, lihat pesan dibawah saat kamu disuruh input jumlah partisi, 512M untuk 512MB, 2G untuk 2GB, dan seterusnya.

{{< figure src="/posts/17-12-2023-instalasi-arch-uefi-quickstart/img/partition-sizing.png" >}}

Jika sudah, saatnya untuk format partisi, ikuti perintah berikut secara berurutan:

```plain
> mkfs.fat -F 32 /dev/sda1
> mkfs.ext4 /dev/sda3
> mkfs.ext4 /dev/sda4
```

`/dev/sda2` adalah partisi swap, aktifkan swap dengan cara:

```plain
> mkswap /dev/sda2
> swapon /dev/sda2
```

Saatnya mount partisi root ke `/mnt` agar bisa menginstall Arch nantinya, untuk itu lakukan:
```plain
> mount /dev/sda3 /mnt
```

Buat `/mnt/boot` dan `/mnt/home` agar kamu bisa mount partisi boot dan home:
```plain
> mkdir -p /mnt/boot
> mkdir -p /mnt/home
```

Mount partisi tersebut:
```plain
> mount /dev/sda1 /mnt/boot
> mount /dev/sda4 /mnt/home
```

Cek dengan `lsblk` atau `fdisk -l` dan pastikan semuanya sudah tepat. Sesuai tabel tadi, partisi yang tepat akan seperti ini:

| partition | mounted to |
|-----------|------------ |
| `/dev/sda1` | /mnt/boot |
| `/dev/sda2` | swap  |
| `/dev/sda3` | /mnt |
| `/dev/sda4` | /mnt/home |

### Menginstall Base/Root System

Install base Arch dan package yang diperlukan dengan:

```plain
# pacstrap -K /mnt base base-devel linux linux-firmware vim nano networkmanager wpa_supplicant grub man-db man-pages xdg-user-dirs efibootmgr
```

Tunggu sampai selesai.

Sebelum masuk ke arch baru kamu, generate fstab dengan perintah: 
```plain
> genfstab -U /mnt >> /mnt/etc/fstab
```

## Post-Instal Arch Linux

### Konfigurasi Esensial

Saatnya kamu chroot ke system arch kamu dengan: 
```plain
> arch-chroot /mnt
```

Konfigurasi waktu dengan:

```plain
> ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
> hwclock --systohc
```

Edit file `/etc/locale.gen` dengan: 
```plain
> nano /etc/locale.gen
```
Dan hilangkan `#` pada `en_US.UTF-8 UTF-8`.
1. Tekan `ctrl + O` untuk save
2. Tekan `ctrl + X` untuk exit

Kemudian, generate locale dengan: 
```plain
> locale-gen
```

Buat locale.conf dengan perintah:
```plain
> echo "LANG=en_US.UTF-8" > /etc/locale.conf
```

Atur hostname dengan:

```plain
> echo "nama_hostname" > /etc/hostname
```

Agar hostname kamu bisa di resolve, tambahkan ini di `/etc/hosts` (edit file):
```plain
127.0.0.1        localhost
::1              localhost
127.0.1.1        nama_hostname
```

Tambahkan password untuk root kamu dengan perintah `passwd`.

Sebagai tambahan, ada baiknya jika kamu membuat non-root user dengan:
```plain
> useradd -mG wheel nama_user
> passwd nama_user
```

Sekarang berikan akses sudo untuk member `wheel` dangan:
```plain
> EDITOR=nano visudo
```
Cari `Uncomment to allow members of group wheel to execute any command`, lalu hilangkan `#` sebelum `%wheel ALL=(ALL) ALL`, save dan exit.

Hidupkan service NetworkManager dengan:
```plain
> systemctl enable NetworkManager
```

Jangan lupa untuk generate GRUB, jika tidak kamu tidak bisa masuk ke system kamu.
```plain
> grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB
> grub-mkconfig -o /boot/grub/grub.cfg
```

### Reboot

Exit chroot dengan `exit` jika semuanya sudah pas.

Sebelum reboot, unmount dulu partisi tadi dengan:

```plain
> umount -R /mnt
```

Lalu reboot dengan `reboot`.

## Login

Login dengan user yang baru dibuat, lalu refresh repositori dengan:
```plain
> sudo pacman -Syu
```

Install `gnome` desktop:
```plain
> pacman -S gnome gnome-tweaks gnome-extensions xorg-server gnome-terminal
> sudo systemctl enable gdm
```
Kemudian `reboot` lagi untuk hasilnya.