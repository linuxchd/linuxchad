+++
title = "Arch Installation Guide"
+++

This guide will show step-by-step how to Install Arch Linux on UEFI mode like a Chad.

---

## Bootable Flash Drive
First of all, you need the Arch Linux image, that can be downloaded from the [Official Website](https://www.archlinux.org/download/).
After that, you should create the bootable flash drive.

If you're on a GNU/linux distribution, you can use the `dd` command for it.
```sh
$ dd bs=4M if=/path/to/archlinux.iso of=/dev/sdx status=progress oflag=sync && sync
```
> Note that you need to update the `of=/dev/sdx` with your USB device location (it can be discovered with the `lsblk` command).

Otherwise, if you're on Windows, you can follow this [tutorial](https://wiki.archlinux.org/index.php/USB_flash_installation_media#In_Windows).

---

## BIOS
We'll install Arch on UEFI mode, so you should enable the UEFI mode and disable the secure boot option on your BIOS system.
> Also remember to change the boot order to boot through your USB device.

---

## Pre installation
I'm presuming that you're already in the Arch Linux zsh shell prompt.

### 1. Check boot mode
To check if the UEFI mode is enabled, run:

```sh
# ls /sys/firmware/efi/efivars
```

If the directory does not exists, the system may be booted in BIOS.

### 2. Update System Clock
Ensures that the system clock is accurate.
```sh
# timedatectl set-ntp true
```

### 3. Connect to a Wifi Network
First, test if you alredy have wifi connection, so run:
```sh
# ping google.com
```
If you're not connected, follow one of these steps:

#### Connect to Wifi with iwd
1. Check network interface name
   ```sh
   # ip link
   ```
   The response will be something like:
   ```
    1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT
        link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    2: enp2s0f0: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN mode DEFAULT qlen 1000
        link/ether 00:11:25:31:69:20 brd ff:ff:ff:ff:ff:ff
    3: wlp3s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP mode DORMANT qlen 1000
        link/ether 01:02:03:04:05:06 brd ff:ff:ff:ff:ff:ff
    ```
2. Run iwd
   ```sh
   # iwctl
   ```
   if $interface is off, then `quit` iwd and run:
   ```sh
   # rfkill unblock all
   # ip link set $interface up
   ```
3. Connect to wifi SSID
  
   run `device list` to list all network interface and run the following commands:
   ```sh
   # station $interface scan
   # station $interface get-networks
   # station $interface connect SSID
   ```
   enter your wifi password

---

