---
title: "Arch Linux Installation /w UEFI"
date: 2023-12-17
description: "Arch Linux installation on UEFI/EFI/GPT devices without BS. Installation using LAN cable with grub, networkmanager, and gnome as additional software during post-installation."
tags: [archlinux, linux, tech, software]
---

Please read the requirements before starting the Arch Linux Install stage.

 
## Requirements
Outline/highlights the Arch Linux installation process:

  1. Have internet access and [live ISO Arch](https://geo.mirror.pkgbuild.com/iso/2023.12.01/) on bootable USB

  2. Provide [bootable USB](https://rufus.ie/en/), device and LAN cable

  3. Make the device boot via bootable USB to go to [live session](https://www.linux.com/training-tutorials/live-booting-linux/)

  4. Connecting the live session to the internet using [LAN cable](https://patchbox.com/blog/what-is-a-lan-cable/)

  5. Preparing the [partition](https://www.howtogeek.com/184659/beginner-geek-hard-disk-partitions-explained/) in the live session

  6. Crucial process to fill the partition with Arch Linux [system root](https://www.geeksforgeeks.org/linux-directory-structure/)

  7. Post-installation, preparing the user configuration and additional software

Device specification, the device specification used by the author when installing Arch Linux:

  - Dell Latitude E7270 with UEFI motherboard firmware.
  - Connected to the internet via LAN Cable.
  - USB Bootable Arch ISO.

Partition specifications, disk size and partitions to be created during the installation process:

  - `/` Root of 20GB
  - `/boot` EFI Boot of 512MB
  - `swap` Swap of 2GB
  - `/home` Home for the remaining
  - This partitioning assumes you are installing on an empty drive

## Arch Linux Pre-Install
 
### Entering Arch Linux Live Session

1. Plug the LAN cable and USB into the Device (off state), [make sure this USB is bootable](https://www.nesabamedia.com/cara-membuat-bootable-flashdisk-windows-10/)

2. Press the key combination to [enter the BIOS menu](https://id.wikihow.com/Masuk-ke-BIOS) of device, usually F1, F2, F8, F12 or Esc.

3. In the BIOS menu, [change the first priority of booting](https://softwarekeep.com/help-center/how-to-change-your-computers-boot-order) to the Bootable USB

4. If the above steps are correct, then you will enter the Arch Linux live session.

## Arch Linux Install
 
### Connect to The Internet

Since you're using a LAN cable, it's automatically connected to the network. If you are using wi-fi, [see this article](../18-12-2023-konfigurasi-wlan-live-session-arch-linux/).
 
Test respond with `ping 8.8.8.8`

### Preaparing Partition

Locate the disk you want to install arch with `fdisk -l` or `lsblk`. Disks usually have names like `/dev/sda`, `/dev/vda`, or `/dev/nvme0n1`. Commonly, if you have a SSD or HDD it is `sda, sdb, sdc, ...`, `vda, vdb, ...` for virtual disks, and `nvme0n1, nvme0n2,...` for NVMe.

Run commands below:

```plain
### substitute "/dev/sda" with yourself, e.g: /dev/vda ###
> cfdisk /dev/sda
```

A GUI will appear, create partitions following the order and recommendations of the table below:

 
| mount point | partition type | details | partition target |
|-|-|-|-|
| `/boot` | EFI System | Partition to store the bootloader, 512MB recommended | `/dev/sda1` |
| `[SWAP]` | Linux Swap | Swap partition, recommended 2GB+ | `/dev/sda2` |
| `/` | Linux Filesystem | Root partition, 20GB | `/dev/sda3` |
| `/home` | Linux Filesystem | Home partition, the rest press `enter` | `/dev/sda4` |
 
If you are confused about how to provide the partition size, see the message below when you are told to input the number of partitions, 512M for 512MB, 2G for 2GB, and so on.

{{< figure src="/posts/17-12-2023-instalasi-arch-uefi-quickstart/img/partition-sizing.png" >}}

If so, it's time to format the partition, follow these commands in order:

```plain
> mkfs.fat -F 32 /dev/sda1
> mkfs.ext4 /dev/sda3
> mkfs.ext4 /dev/sda4
```

`/dev/sda2` is the swap partition, enable swap by:

```plain
> mkswap /dev/sda2
> swapon /dev/sda2
```

It's time to mount the root partition to `/mnt` so you can install Arch later:

```plain
> mount /dev/sda3 /mnt
```

Create `/mnt/boot` and `/mnt/home` to prepare mounting boot and home partitions:

```plain
> mkdir -p /mnt/boot
> mkdir -p /mnt/home
```

Mount the partitions:

```plain
> mount /dev/sda1 /mnt/boot
> mount /dev/sda4 /mnt/home
```

Check with `lsblk` or `fdisk -l` and make sure everything is correct. According to the table, the right partition will look like this:

| partition | mounted to |
|-----------|------------ |
| `/dev/sda1` | /mnt/boot |
| `/dev/sda2` | swap  |
| `/dev/sda3` | /mnt |
| `/dev/sda4` | /mnt/home |

### Installing Base/Root System

Install Arch base and required packages with:

```plain
# pacstrap -K /mnt base base-devel linux linux-firmware vim nano networkmanager wpa_supplicant grub man-db man-pages xdg-user-dirs efibootmgr
```

Wait, till it finishes.

Before entering your new arch system, generate fstab with the command: 

```plain
> genfstab -U /mnt >> /mnt/etc/fstab
```

## Post-Install Arch Linux

### Essential Configs

Chroot into your new arch system:
```plain
> arch-chroot /mnt
```

Timezone Config:

```plain
> ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
> hwclock --systohc
```

Edit `/etc/locale.gen` with: 

```plain
> nano /etc/locale.gen
```

And remove `#` on `en_US.UTF-8 UTF-8` line.
1. `ctrl + O` to save
2. `ctrl + X` to exit

Generate locale with: 

```plain
> locale-gen
```

Create locale.conf with:
```plain
> echo "LANG=en_US.UTF-8" > /etc/locale.conf
```

Sets hostname with:

```plain
> echo "your_hostname" > /etc/hostname
```

To resolve your hostname, add this to `/etc/hosts` (edit file):
```plain
127.0.0.1        localhost
::1              localhost
127.0.1.1        your_hostname
```

Add a password for your root with the `passwd` command.

Additionally, it's a best practices to create a non-root user with:

```plain
> useradd -mG wheel user_name
> passwd user_name
```

Give sudo access for `wheel` group with:
```plain
> EDITOR=nano visudo
```
Search for `Uncomment to allow members of group wheel to execute any command`, and ramove leading `#` before `%wheel ALL=(ALL) ALL`, save and exit.

Enable NetworkManager service with:
```plain
> systemctl enable NetworkManager
```

Don't forget to generate GRUB, if you doesn't, it can't get inside the system.
```plain
> grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB
> grub-mkconfig -o /boot/grub/grub.cfg
```

### Reboot

Exit chroot with `exit` if you think all set up.

Before reboot, unmount the partitions first:

```plain
> umount -R /mnt
```

Reboot with `reboot`.

## Login

Login to your just created user, then fetch for the latest repo update with:
```plain
> sudo pacman -Syu
```

Install `gnome` desktop:
```plain
> pacman -S gnome gnome-tweaks gnome-extensions xorg-server gnome-terminal
> sudo systemctl enable gdm
```
Then `reboot` again for the results.