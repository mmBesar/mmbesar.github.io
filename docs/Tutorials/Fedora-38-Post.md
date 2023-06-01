---
template: blog_post.html
title: Fedora 38 Post Installation
description: Fedora 38 | خطوات هامة ويجب تنفيذها بعد تنصيب فيدورا
date: 2023-06-01
---

# Fedora 38 | خطوات هامة ويجب تنفيذها بعد تنصيب فيدورا

![type:video](https://www.youtube.com/embed/2mgFnBfnhLY)

<div dir="rtl">
أهم الخطوات التي يجب تنفيذها بعد تنصيب فيدورا، للحصول على أفضل تجربة استخدام على التوزيعة الأفضل.
</div>

<p hidden>#more</p>

## System Update

```sh
sudo dnf upgrade
```

## Enable Third-Party Repositories if You Didn't

### Enable [rpmfusion](https://rpmfusion.org/Configuration)

``` sh
sudo dnf install https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
```

```sh
sudo dnf --refresh upgrade
```

```sh
sudo dnf groupupdate core
```

## [Multimedia](https://rpmfusion.org/Howto/Multimedia)

### Switch to full ffmpeg

``` sh
sudo dnf swap ffmpeg-free ffmpeg --allowerasing
```

### Install additional codec

``` sh
sudo dnf install gstreamer1-plugins-{bad-\*,good-\*,base} gstreamer1-plugin-openh264 gstreamer1-libav --exclude=gstreamer1-plugins-bad-free-devel
```

``` sh
sudo dnf install lame\* --exclude=lame-devel
```

``` sh
sudo dnf group upgrade --with-optional Multimedia
```

## Install [NVIDIA Drivers](https://rpmfusion.org/Howto/NVIDIA)

## Hardware acceleration

### INTEL

- Intel(recent)

Using the rpmfusion-nonfree section

``` sh
sudo dnf install intel-media-driver
```

- Intel(older)

Using the rpmfusion-free section

``` sh
sudo dnf install libva-intel-driver
```

### AMD

``` sh
sudo dnf swap mesa-va-drivers mesa-va-drivers-freeworld
sudo dnf swap mesa-vdpau-drivers mesa-vdpau-drivers-freeworld
```

### NVIDIA

``` sh
sudo dnf install nvidia-vaapi-driver
```

## Microsoft Fonts on Fedora

``` sh
sudo dnf install curl cabextract xorg-x11-font-utils fontconfig
```

``` sh
sudo rpm -i https://downloads.sourceforge.net/project/mscorefonts2/rpms/msttcore-fonts-installer-2.6-1.noarch.rpm
```

check

``` sh
fc-match TimesNewRoman
```

## RAR Files

``` sh
sudo dnf install unrar
```

## GNOME 

- Tweaks:

``` sh
sudo dnf install gnome-tweaks
```

- Extension Manager:

``` sh
flatpak install flathub com.mattjakeman.ExtensionManager
```

## Backup system with Timeshift

```sh
sudo dnf install timeshift
```

![type:video](https://www.youtube.com/embed/8cz28-r-xPc)

## Backup your data with [Deja Dup](https://flathub.org/apps/details/org.gnome.DejaDup)

```sh
flatpak install flathub org.gnome.DejaDup
```

![type:video](https://www.youtube.com/embed/7eljgBxLWug)

## Top Gnome Extension

![type:video](https://www.youtube.com/embed/6yEJ43LLIJg)


## ZSH

![type:video](https://www.youtube.com/embed/fDuGKsQ3bV4)
