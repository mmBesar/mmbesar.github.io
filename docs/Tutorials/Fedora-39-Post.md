---
template: blog_post.html
title: Fedora 39 Post Installation
description: Fedora 39 | خطوات يجب تنفيذها بعد تنصيب فيدورا
date: 2023-11-16
---

# Fedora 39 | خطوات يجب تنفيذها بعد تنصيب فيدورا

![type:video](https://www.youtube.com/embed/HB-qbEUmofM)

<div dir="rtl">
خطوات هامة، ويجب تنفيذها بعد تنصيب فيدورا 39، للحصول على تجربة استخدام أفضل.
</div>

<p hidden>#more</p>

## Enable [rpmfusion](https://rpmfusion.org/Configuration)

``` sh
sudo dnf install https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
```

```sh
sudo dnf groupupdate core
```

## System Update

```sh
sudo dnf --refresh upgrade
```

**REBOOT**

## Firmware Update if Supported:

```sh
sudo fwupdmgr get-devices 
sudo fwupdmgr refresh --force 
sudo fwupdmgr get-updates 
sudo fwupdmgr update
```

## Install [Nvidia Drivers](https://rpmfusion.org/Howto/NVIDIA)

## [Multimedia](https://rpmfusion.org/Howto/Multimedia)

- Switch to full ffmpeg

``` sh
sudo dnf swap ffmpeg-free ffmpeg --allowerasing
```

- Install additional codec

```sh
sudo dnf groupupdate multimedia --setop="install_weak_deps=False" --exclude=PackageKit-gstreamer-plugin
```

```sh
sudo dnf groupupdate sound-and-video
```

``` sh
sudo dnf install lame\* --exclude=lame-devel
```

``` sh
sudo dnf install vlc mpv gthumb
```

## Hardware acceleration

### INTEL

=== "Intel(recent)"

    Using the rpmfusion-nonfree section

    ``` sh
    sudo dnf install intel-media-driver
    ```

=== "Intel(older)"

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

## Hostname

```sh
sudo hostnamectl set-hostname NAME
```

## Rar and 7z compressed

``` sh
sudo dnf install unrar p7zip p7zip-plugins
```

## GNOME 

- Tweaks:

``` sh
sudo dnf install gnome-tweaks
```

- GNOME Extra Themes and Enable Dark Mode for Legacy Apps

```sh
sudo dnf install gnome-themes-extra
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


## ZSH better Terminal

![type:video](https://www.youtube.com/embed/fDuGKsQ3bV4)
