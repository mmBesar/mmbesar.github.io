---
template: blog_post.html
title: macOS Theme for Ubuntu
description: تخصيص أبونتو ليصبح شكل نظام ماك
date: 2022-07-08
---

## macOS Theme for Ubuntu

![type:video](https://www.youtube.com/embed/AlMniRLym4w)

<div dir="rtl">
التخصيص هو أحد أهم مميزات لينكس، في الفيديو شرح مفصل لكيفية محاكاة ابونتو لنظام ماك من حيث الشكل وبعض الخصائص.  
الشرح يشمل تغيير الشكل الأساسي والخلفية مرورًا بالأيقونات ومؤشر الماوس وحتى متصفح Firefox. إضافة إلى تطبيق Ulauncher لمحاكاة نظان ماك في البحث وفتح التطبيقات.
</div>

<p hidden>#more</p>

1. Download Packages
```bash
sudo apt install git gnome-tweaks -y
```

2. Install [Firefox Extension](https://extensions.gnome.org)

3. Install Extensions
	1. [Dash to Dock](https://extensions.gnome.org/extension/307/dash-to-dock)
		- Set Dock Position to the Bottom
		- Disable Intelligent autohide
	2. [User Themes](https://extensions.gnome.org/extension/19/user-themes)

4. Create a work directory

```bash
mkdir mac-theme && cd mac-theme
```

5. Main Theme
	- Download
	```bash
	git clone https://github.com/vinceliuice/WhiteSur-gtk-theme.git
	```
	- Install Main Theme
	```bash
	./WhiteSur-gtk-theme/install.sh -c dark -c light -i ubuntu -N mojave
	```
	- Install the Theme for Firefox and Dash to Dock
	```bash
	./WhiteSur-gtk-theme/tweaks.sh -f -d
	```

6. Install icons 
	- Download
	```bash
	git clone https://github.com/vinceliuice/WhiteSur-icon-theme.git
	```
	- Install
	```bash
	./WhiteSur-icon-theme/install.sh
	```

7. Cursors 
	- Download
	```bash
	git clone https://github.com/vinceliuice/McMojave-cursors
	```
	- Install
	```bash
	cd McMojave-cursors && ./install.sh && cd ..
	```

8. Wallpapers
	- Download
	```bash
	wget https://codeload.github.com/vinceliuice/WhiteSur-gtk-theme/zip/wallpapers
	```
	- Extract
	```bash
	unzip wallpapers
	```
	- Install
	```bash
	sudo ./WhiteSur-gtk-theme-wallpapers/install-gnome-backgrounds.sh
	```

9. Ulauncher - You have two options to install it

	1. via PPA
		- add the PPA
		```bash
		sudo add-apt-repository ppa:agornostal/ulauncher
		```
		- Update
		```bash
		sudo apt update
		```
		- Insatall the app
		```bash
		sudo apt install ulauncher -y
		```
  
	2. via DEB file - IF YOUR DISTRO NOT SUPPORTED VIA PPA  
		- Install `wget` if it's not installed already
		```bash
		sudo apt install wget -y
		```
		- Download the latest release from GitHub
		```bash
		wget https://github.com/$(wget https://github.com/Ulauncher/Ulauncher/releases/latest -O - | egrep '/.*/.*/.*deb' -o)
		```
		- Install the DEB file
		```bash
		sudo apt install ./ulauncher*.deb -y
		```

10. Install `wmctrl` for Wayland 
```bash
sudo apt install wmctrl -y
```

11. SET Super+u for `ulauncher-toggle`
