---
template: blog_post.html
title: elementary OS 6 Post Install
description: elementary OS 6 | خطوات يجب تنفيذها بعد التنصيب
date: 2021-08-17
---

# elementary OS 6 - Post Install

![type:video](https://www.youtube.com/embed/0rY9geTvRzo)

<div dir="rtl">
من توزيعات لينكس المميزة، يتم الترويج لها كبديل جيد لنظام ماك macOS من أبل، ذلك بسبب واجهة التوزيعة المشابهة لواجهة ماك بشكل كبير.
</div>

<div dir="rtl">
التوزيعة مبنية على أبونتو طويلة الدعم، أول إصدار كان في 2012 وإصدارتها غير مرتبطة بموعد محدد.
في الفيديو مراجعة كاملة للتوزيعة في إصدارها السادس.
</div>

<div dir="rtl">
نطرح في المقال أهم الخطوات التي يجب تنفيذها بعد تنصيب توزيعة elementary OS 6.
</div>

<p hidden>#more</p>

## Update and Clean

* Update

```bash
sudo apt update && sudo apt upgrade -y
```

* Clean

```bash
sudo apt autoremove && sudo apt autoclean
```

## Change the Hostname

1. Change the hostname using `hostnamectl`

```bash
sudo hostnamectl set-hostname NEW-NAME
```

2. Edit the /etc/hosts file

```bash
sudo nano /etc/hosts
```

## Install Pantheon Tweaks

Install the required packages

```bash
sudo apt install -y software-properties-common
```

Add the repository

```bash
sudo add-apt-repository -y ppa:philip.scott/pantheon-tweaks
```

Install Pantheon Tweaks

```bash
sudo apt install -y pantheon-tweaks
```

## Flatpak

Add Flathub as a system remote:

```bash
flatpak remote-add --if-not-exists flathub --system https://flathub.org/repo/flathub.flatpakrepo
```

Install Apps (e.g. VLC)

* system-wide:

```bash
flatpak install --system flathub org.videolan.VLC
```

* user:

```bash
flatpak install --user flathub org.videolan.VLC
```
