---
template: blog_post.html
title: Ubuntu 22.10 Post Installation
description: خطوات يجب تنفيذها بعد تنصيب أوبونتو 22.10
date: 2022-11-23
---

# Ubuntu 22.10 Post Installation

![type:video](https://www.youtube.com/embed/m4ACmfnjXjs)

<div dir="rtl">
أشارك معكم أهم الخطوات التي أنفذها دومًا بعد تنصيب أوبونتو 22.10، للحصول أفضل تجرِبة استخدام.
</div>

<p hidden>#more</p>

## Update

```sh
sudo apt update && sudo apt upgrade -y
```

## Remove Firefox Snap and Install Firefox Deb

- Remove Firefox Snap

```sh
sudo apt remove --autoremove firefox
```

```sh
sudo snap remove --purge firefox
```

- Add Mozilla Team PPA

```sh
sudo add-apt-repository ppa:mozillateam/ppa
```

- Set Mozilla team PPA as high Priority

```sh
echo '
Package: *
Pin: release o=LP-PPA-mozillateam
Pin-Priority: 1001
' | sudo tee /etc/apt/preferences.d/firefox-ppa-priority
```

- Block the Ubuntu repo

```sh
echo '
Package: firefox*
Pin: release o=Ubuntu*
Pin-Priority: -1
' | sudo tee /etc/apt/preferences.d/firefox-ubuntu-block
```

- Set Firefox for automatic updates
```sh
echo 'Unattended-Upgrade::Allowed-Origins:: "LP-PPA-mozillateam:${distro_codename}";' | sudo tee /etc/apt/apt.conf.d/51unattended-upgrades-firefox
```

- Update

```sh
sudo apt update
```

- Install Firefox DEB

```sh
sudo apt install firefox
```

## Install Ubuntu Restricted Extras

```sh
sudo apt install ubuntu-restricted-extras
```

## Minimize on Click

- Minimize on Click the old style

```sh
gsettings set org.gnome.shell.extensions.dash-to-dock click-action 'minimize'
```

- Minimize on Click 22.10 style

```sh
gsettings set org.gnome.shell.extensions.dash-to-dock click-action 'focus-minimize-or-appspread'
```

## Flatpak and Flathub

```sh
sudo apt install flatpak
```
```sh
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```

## Gnome Software

```sh
sudo apt install gnome-software gnome-software-plugin-flatpak
```

## AppImage Support

```sh
sudo apt install libfuse2
```

## Gnome Extensions

```sh
sudo apt install chrome-gnome-shell gnome-shell-extension-prefs gnome-shell-extension-manager
```

## Gnome Tweaks

```sh
sudo apt install gnome-tweaks
```

## Enable ‘New Documents’ context menu

```sh
touch ~/Templates/new
```

## Arabic Keyboard

## System Backup with Timeshift

```sh
sudo apt install timeshift
```

## Backup your files with [Deja Dup](https://flathub.org/apps/details/org.gnome.DejaDup)

## Update again

```sh
sudo apt update && sudo apt upgrade -y && flatpak update -y && sudo snap refresh
```