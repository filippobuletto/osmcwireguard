# Install WireGuard on OSMC

## 1. Prepare the system

```console
osmc@osmc:~$ sudo apt update
osmc@osmc:~$ sudo apt upgrade
osmc@osmc:~$ echo "deb http://deb.debian.org/debian/ unstable main" | sudo tee --append /etc/apt/sources.list.d/unstable.list
osmc@osmc:~$ sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 8B48AD6246925553
osmc@osmc:~$ printf 'Package: *\nPin: release a=unstable\nPin-Priority: 150\n' | sudo tee --append /etc/apt/preferences.d/limit-unstable
osmc@osmc:~$ sudo perl -pi -e 's/#{1,}?net.ipv4.ip_forward ?= ?(0|1)/net.ipv4.ip_forward = 1/g' /etc/sysctl.conf
osmc@osmc:~$ sudo reboot
```

Check if IP forwarding is enabled:

```console
osmc@osmc:~$ sysctl net.ipv4.ip_forward
net.ipv4.ip_forward = 1
```

## 2. Install and patch Kernel Headers

```console
osmc@osmc:~$ sudo apt-get install rbp2-headers-$(uname -r) rbp2-source-$(uname -r) libmnl-dev libelf-dev build-essential git pkg-config

osmc@osmc:~$ cd /usr/src/
osmc@osmc:/usr/src$ sudo rm -r rbp2-headers-`uname -r`/include/linux/*
osmc@osmc:/usr/src$ sudo tar -xvf rbp2-source-`uname -r`.tar.bz2 rbp2-source-`uname -r`/include/linux
osmc@osmc:/usr/src$ sudo cp -ar ./rbp2-source-`uname -r`/include/linux/* ./rbp2-headers-`uname -r`/include/linux/
osmc@osmc:/usr/src$ sudo ln -s /usr/src/rbp2-headers-`uname -r` /lib/modules/`uname -r`/build
```

## 3. WireGuard installation

First installation:

```console
osmc@osmc:~$ sudo apt update
osmc@osmc:~$ sudo apt install wireguard-dkms wireguard-tools wireguard
```

After updating kernel, repeat `2.` and:

```console
osmc@osmc:~$ sudo dpkg-reconfigure wireguard-dkms
```

## Configure WireGuard

[https://www.wireguard.com/quickstart/](https://www.wireguard.com/quickstart/)
