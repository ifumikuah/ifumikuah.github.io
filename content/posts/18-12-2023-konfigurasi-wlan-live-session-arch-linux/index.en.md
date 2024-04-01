---
title: "WLAN Configs on Live Session Arch Linux"
date: 2023-12-18
description: "Part of \"Arch Linux Installation\", explains how to connect to Wi-Fi in Arch Linux Live Session"
tags: [archlinux, linux, tech, software]
---

Check your device name with `ip link`, wireless card devices usually have names starting with "w", for example. `wlan0`, `wlp3s0`. Turn on the device with `ip link set name_device up`.

```plain
> ip link
> ip link set nama_device up
```

## Rfkill

Run `rfkill` to check the block status of the wireless card, if the wireless card fails to start, then there are two possibilities: soft-block or hard-block.

- hard-block: check if your laptop has a physical switch to turn wi-fi on and off.

- soft-block : run `rfkill unblock wlan` to unblock wi-fi
```plain
> rfkill unblock wlan
```

## Iwctl

Use `iwctl` to enter iwd interactive shell mode, your prompt should become `[iwd]#`.

```plain
[iwd]# station device_name scan

[iwd]# station device_name get-networks

[iwd]# station device_name connect ssid_name
```

Exit the interactive shell if succeed.

## DHCP

One more step, run `dhcpcd device_name` to get yourself a DHCP IP. Check connectivity status with `ping 8.8.8.8`

```plain
> dhcpcd device_name
> ping 8.8.8.8
```