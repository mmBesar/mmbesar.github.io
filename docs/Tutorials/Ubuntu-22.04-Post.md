---
template: blog_post.html
title: Ubuntu 22.04 Post Install
description: Ubuntu 22.04 | أهم الخطوات بعد تنصيب أبونتو
date: 2022-07-07
---

## Ubuntu 22.04 Post Installation

![type:video](https://www.youtube.com/embed/7eljgBxLWug)

<div dir="rtl">
توزيعة أبونتو 22.04 توزيعة طويلة الدعم، حيث يستمر 5 سنوات. أضع بين يديك أهم الخطوات التي يجب تنفيذها بعد تنصيب التوزيعة، للحصول على أفضل تجربة استخدام.
</div>

<p hidden>#more</p>

* Update

```bash
sudo apt update && sudo apt upgrade -y
```

* Remove Firefox Snap and Install Firefox Deb

1. Remove Firefox Snap

```sh
sudo apt remove --purge firefox
```
```sh
sudo snap remove --purge firefox
```

2. add Mozilla team PPA

```sh
sudo add-apt-repository ppa:mozillateam/ppa
```

3. set Mozilla team PPA as high Priority

```sh
echo '
Package: *
Pin: release o=LP-PPA-mozillateam
Pin-Priority: 1001
' | sudo tee /etc/apt/preferences.d/firefox
```

OR

```sh
echo '
Package: firefox*
Pin: release o=LP-PPA-mozillateam
Pin-Priority: 501
' | sudo tee /etc/apt/preferences.d/firefox
```

- block the Ubuntu repo

```sh
echo '
Package: firefox*
Pin: release o=Ubuntu*
Pin-Priority: -1
' | sudo tee /etc/apt/preferences.d/firefox
```

> **About Pin and PPA Priority:**  
> *From apt man page*  
> - P >= 1000:  
causes a version to be installed even if this constitutes a downgrade of the package
> - 990 <= P < 1000:  
causes a version to be installed even if it does not come from the target release, unlessthe installed version is more recent
> - 500 <= P < 990:  
causes a version to be installed unless there is a version available belonging to the targetrelease or the installed version is more recent
> - 100 <= P < 500:  
causes a version to be installed unless there is a version available belonging to some otherdistribution or the installed version is more recent
> - 0 < P < 100:  
causes a version to be installed only if there is no installed version of the package
> - P < 0:  
prevents the version from being installed
> - P = 0:  
has undefined behaviour, do not use it.

4. set Firefox for automatic updates
```sh
echo 'Unattended-Upgrade::Allowed-Origins:: "LP-PPA-mozillateam:${distro_codename}";' | sudo tee /etc/apt/apt.conf.d/51unattended-upgrades-firefox
```

5. Update

```sh
sudo apt update
```

6. install Firefox DEB

```bash
sudo apt install firefox
```

* Install Ubuntu Restricted Extras

```bash
sudo apt install ubuntu-restricted-extras
```

* Minimize on Click

```bash
gsettings set org.gnome.shell.extensions.dash-to-dock click-action minimize
```

* Flatpak

```bash
sudo apt install gnome-software gnome-software-plugin-flatpak flatpak
```
```bash
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```

* Extensions and Tweaks

```bash
sudo apt install chrome-gnome-shell gnome-shell-extension-prefs
```

```bash
sudo apt install gnome-tweaks gnome-shell-extension-manager
```

* Arabic Keyboard

* Backup

1. Timeshift

```bash
sudo apt install timeshift
```

2. Backup your files with [Deja Dup](https://flathub.org/apps/details/org.gnome.DejaDup)
