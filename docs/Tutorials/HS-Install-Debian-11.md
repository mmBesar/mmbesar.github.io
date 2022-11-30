---
template: blog_post.html
title: Home Server - Install Debian 11
description: Home Server | تنصيب وضبط أفضل توزيعة للخوادم
date: 2022-11-30
---

# Home Server | تنصيب وضبط أفضل توزيعة للخوادم

![type:video](https://www.youtube.com/embed/jF9ChSLYsZE)

<div dir="rtl">
بعد استخدام لسنوات أصبح السيرفر المنزلي من أهم أدواتي، لمشاركة الملفات ومتابعة المحتوى المرئي أو المسموع عن طريق خدمة خاصة مثل NETFLIX، ولكن تحت تصرفي الكامل، توفير خِدْمَات أساسية بديلة لخدمات الشركات الكبرى على الإنترنت، مما يمنحني الخصوصية.
</div>
<div dir="rtl">
اليوم أشارك معكم أول خطوة في إنشاء الخادم المنزلي "Home Server"، وهي تحميل وتنصيب توزيعة Debian 11، أفضل توزيعة لتنفيذ المهمة، وباستخدام كمبيوتر قديم عمره 15 عام.
</div>

<p hidden>#more</p>

## تحديث النظام

```sh
apt update && apt upgrade
```

## `sudo` تنصيب

```sh
apt install sudo
```

## `sudo` إضافة حسابك إلى مجموعة

```sh
gpasswd -a $USER sudo
```

## locale ضبط إعدادات

```sh
export PATH=$PATH:/usr/sbin
```

```sh
export LANGUAGE=en_US.UTF-8
export LANG=en_US.UTF-8
export LC_ALL=en_US.UTF-8
locale-gen en_US.UTF-8
```

## Swap

### zram

- تنصيب:

```sh
sudo apt install zram-tools
```

- ضبط الإعدادات على 70% من مساحة الرام:

```sh
echo -e "ALGO=zstd\nPERCENT=70" | sudo tee -a /etc/default/zramswap
```

> I set it to 80%

- إعادة تشغيل الخدمة

```sh
sudo service zramswap reload
```

### swap file

- إنشاء ملف بمساحة 4 جيجا

```sh
sudo dd if=/dev/zero of=/swapfile bs=1024 count=4194304
```

- ضبط الصلاحيات على الملف

```sh
sudo chmod 600 /swapfile
```

- format as swap - تخصيص الملف للعمل

```sh
sudo mkswap /swapfile
```

- تفعيله

```sh
sudo swapon /swapfile
```

- `fstab` إضافة الملف إلى

أخذ نسخة إحتياطية من الملف الأصلي

```sh
sudo cp /etc/fstab /etc/fstab.bak
```

`fstab` تعديل

```sh
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
```

### List swap

```sh
sudo swapon --show
```