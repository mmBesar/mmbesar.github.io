---
template: blog_post.html
title: Debian 12 Post Install
description: Debian 12 | خطوات يجب تنفيذها بعد التنصيب
date: 2023-09-20
---

# Debian 12 Post Install

![type:video](https://www.youtube.com/embed/1efTU3Bs2-I)

<div dir="rtl">
نتعرف بشكل عملي على أهم الخطوات الواجب تنفيذها بعد تنصيب توزيعة ديبيان 12، ومع واجهة بلازما بشكل خاص.
</div>

<p hidden>#more</p>

## Double-Click to Open

## Update

```sh
sudo apt update && sudo apt upgrade -y
```

## Check or Install systemd-timesyncd

- check

```sh
sudo systemctl status systemd-timesyncd
```

- install

```sh
sudo apt install systemd-timesyncd
```

- check time:

```sh
timedatectl
```

## Enable `non-free` and `contrib`

!!! danger ""

    If you get `apt-add-repository: command not found`  
    install `software-properties-common`:
    ```sh
    sudo apt install software-properties-common
    ```

- List enabled repositories

```sh
apt-add-repository --list
```

- Enable `contrib`

```sh
sudo apt-add-repository contrib
```

- Enable `non-free`

```sh
sudo apt-add-repository non-free
```

- Enable `contrib` and `non-free` at once:

```sh
sudo apt-add-repository contrib non-free
```

- To disable a repository run:

```sh
sudo apt-add-repository --remove <REPO-NAME>
```

## Update again

```sh
sudo apt update && sudo apt upgrade -y
```

## Install VLC and Codecs

```sh
sudo apt install libavcodec-extra gstreamer1.0-libav gstreamer1.0-plugins-ugly gstreamer1.0-vaapi vlc
```

## rar support

```sh
sudo apt install rar unrar
```

## Microsoft Fonts and alternatives for "Cambria, Calibri"

```sh
sudo apt install ttf-mscorefonts-installer fonts-crosextra-caladea fonts-crosextra-carlito
```

## Install and Enable Firewall

```sh
sudo apt install gufw
sudo ufw enable
sudo ufw allow ssh
```

## Apps

```sh
sudo apt install htop neofetch
```

## Flatpak Support

- Install `flatpak`

``` sh
sudo apt install flatpak
```

- Enable Flathub

``` sh
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```

OR

``` sh
flatpak remote-add --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo
```

- Enable Flatpak support in Plasma Discover **KDE Plasma Only**

```sh
sudo apt install plasma-discover-backend-flatpak
```

- Enable Flatpak support in GNOME Software **GNOME Only**

```sh
sudo apt install gnome-software-plugin-flatpak
```

- Install Flatseal to manage flatpak apps

```sh
flatpak install flathub com.github.tchx84.Flatseal
```

![type:video](https://www.youtube.com/embed/PERYnPbAUy0)

## Firefox from Mozilla

![type:video](https://www.youtube.com/embed/vR7vN5WWvwE)

## Nvidia

- Install [Nvidia Drivers](https://wiki.debian.org/NvidiaGraphicsDrivers)

```sh
sudo apt install nvidia-detect
sudo nvidia-detect
```

- [NVIDIA Optimus](https://wiki.debian.org/NVIDIA%20Optimus)

## Arabic Keyboard Support

- Add Arabic layoout

Settings > Input Devices

- Set Keyboard Main shortcut

- Set Keyboard shortcut for 3rd level

## Better Software center **Xfce Only**

```sh
sudo apt install gnome-software
sudo apt install gnome-software-plugin-flatpak
```

## Better Battery Life - Optional and for laptops only

```sh
sudo apt install tlp
```

## Better Samba access **KDE Plasma Only**

```sh
sudo apt install kio-fuse
```