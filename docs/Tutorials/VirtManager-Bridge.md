---
template: blog_post.html
title: VirtManager - Bridge Connection
description: Virt Manager | إنشاء وضبط Bridge Connection بنفس MAC الشريحة الحقيقية
date: 2023-08-20
---

# <div dir="rtl">Virt Manager | إنشاء وضبط Bridge Connection بنفس MAC الشريحة الحقيقية</div>

![type:video](https://www.youtube.com/embed/8ZAmUfRc1_M)

<div dir="rtl">
شرح كيفية إنشاء Bridge Connection بنفس الـ MAC Address الخاص بالشريحة - كارت الشبكة - الحقيقية، للحصول على نفس الـ IP المُحدد من الـ DHCP، واستخدامه بشكل فعال مع الأجهزة الافتراضية على Virt Manager - KVM / QEMU.
</div>

<p hidden>#more</p>

## Get the device name and MAC address

``` sh
ip addr
```

## Install and Run `nmtui`

=== "Fedora"

    ``` sh
    sudo dnf install NetworkManager-tui
    ```

=== "Ubuntu / Debian"

    ``` sh
    sudo apt install network-manager
    ```

=== "Arch"

    ``` sh
    sudo pacman -Sy networkmanager
    ```


``` sh
nmtui
```

## Edit `br0.nmconnection`

``` sh
sudo nano /etc/NetworkManager/system-connections/br0.nmconnection
```

add the following

> NOTE:  
Set your MAC address

```ini
[bridge]
stp=false
mac-address=FF:FF:FF:FF:FF:FF
```

to be as shown below

```ini
[connection]
id=br0
uuid=caf8d11-8f7b-5401-7ef1-87d1b5076624
type=bridge
interface-name=br0

[bridge]
stp=false
mac-address=FF:FF:FF:FF:FF:FF

[ipv4]
method=auto

[ipv6]
addr-gen-mode=stable-privacy
method=auto

[proxy]
```

## Create a new connection in Virt Manager with below values

``` xml
<network>
  <name>br0</name>
  <forward mode="bridge"/>
  <bridge name="br0" />
</network>
```