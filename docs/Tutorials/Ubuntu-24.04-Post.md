---
template: blog_post.html
title: Ubuntu 24.04 Post Installation
description: أهم الخطوات بعد تنصيب أوبونتو 24.04
date: 2024-05-29
---

# Ubuntu 24.04 Post Installation

![type:video](https://www.youtube.com/embed/RGNtZu5nIRM)

<div dir="rtl">
شرح مبسط لأهم الخطوات بعد تنصيب أوبونتو 24.04ـ للحصول على أفضل تجربة مستخدم،
سوف أشرح كل خطوة، وتخير ما يناسبك.
</div>

<p hidden>#more</p>

## Update

```sh
sudo apt update && sudo apt upgrade -y
```

## Install Ubuntu Restricted Extras

```sh
sudo apt install ubuntu-restricted-extras
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
sudo apt install chrome-gnome-shell gnome-shell-extension-manager
```

## Gnome Tweaks

```sh
sudo apt install gnome-tweaks
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

## Enable ‘New Documents’ context menu

```sh
touch ~/Templates/new
```

## Set Keyboard Input Source Switching

* Set `switch-input-source` switch to Shift+Alt LEFT

```bash
gsettings set org.gnome.desktop.wm.keybindings switch-input-source "['<Shift>Alt_L']"
```

* set `switch-input-source-backward` to Alt+Shift LEFT

```bash
gsettings set org.gnome.desktop.wm.keybindings switch-input-source-backward "['<Alt>Shift_L']"
```

* to view current keybindings run:

```bash
gsettings get org.gnome.desktop.wm.keybindings switch-input-source
gsettings get org.gnome.desktop.wm.keybindings switch-input-source-backward
```

* to reset:

```sh
gsettings set org.gnome.desktop.wm.keybindings switch-input-source "['<Super>space', 'XF86Keyboard']"
gsettings set org.gnome.desktop.wm.keybindings switch-input-source-backward "['<Shift><Super>space', '<Shift>XF86Keyboard']"
```

## System Backup with Timeshift

```sh
sudo apt install timeshift
```

How to use Timeshift:
![type:video](https://www.youtube.com/embed/8cz28-r-xPc)

## Backup your files with [Deja Dup](https://flathub.org/apps/details/org.gnome.DejaDup)

How to use Deja Dup:
![type:video](https://www.youtube.com/embed/7eljgBxLWug)

## Update again

```sh
sudo apt update && sudo apt upgrade -y && flatpak update -y && sudo snap refresh
```

## Install GNOME Extensions

My Favorite Extensions
![type:video](https://www.youtube.com/embed/6yEJ43LLIJg)