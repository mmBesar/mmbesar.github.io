---
template: blog_post.html
title: Debian 11 Post Install
description: Debian 11 | خطوات يجب تنفيذها بعد التنصيب
date: 2022-08-30
---

# Debian 11 Post Install

![type:video](https://www.youtube.com/embed/bSDoSdpsyYY)

<div dir="rtl">
نتعرف بشكل عملي على أهم الخطوات الواجب تنفيذها بعد تنصيب توزيعة ديبيان 11، ومع واجهة Xfce بشكل خاص.
</div>

<p hidden>#more</p>

## Install `sudo` if not installed

```sh
su
apt install sudo
```

## Add User to `sudo` Group

```sh
su
gpasswd -a $USER sudo
```

## Enable None Free

```sh
cat /etc/apt/sources.list | grep "contrib non-free" || sudo sed -i "s/main/main contrib non-free/g" /etc/apt/sources.list
```

## Install Codecs, Microsoft Fonts and rar support (Ubuntu restricted-extras)

```sh
sudo apt install libavcodec-extra ttf-mscorefonts-installer rar unrar gstreamer1.0-libav gstreamer1.0-plugins-ugly gstreamer1.0-vaapi
```

## Microsoft Fonts alternatives for "Cambria, Calibri"

```sh
sudo apt install fonts-crosextra-caladea fonts-crosextra-carlito
```

## Install and Enable Firewall

```sh
sudo apt install ufw
sudo ufw enable
sudo ufw allow ssh
```

## Apps

```sh
sudo apt install plocate htop
sudo updatedb
```

## Flatpak Support

```sh
sudo apt install flatpak
sudo flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```

## Nvidia Drivers

```sh
sudo apt install nvidia-detect
sudo nvidia-detect
```

## CPU Microcode

- For Intel CPUs

```sh
sudo apt install intel-microcode
```

- For AMD CPUs

```sh
sudo apt install amd64-microcode
```

## Better Software center (for Xfce Only)

```sh
sudo apt install gnome-software
sudo apt install gnome-software-plugin-flatpak
```

## Better Battery Life - Optional and for laptops only

```sh
sudo apt install tlp
```
