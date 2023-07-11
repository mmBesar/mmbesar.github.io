---
template: blog_post.html
title: Create a SWAP File - Install and modify ZRAM
description: إنشاء SWAP File وتعديله - تنصيب وتعيين ZRAM
date: 2023-07-11
---

# <div dir="rtl">إنشاء SWAP File وتعديله - تنصيب وتعيين ZRAM</div>

![type:video](https://www.youtube.com/embed/yWuHI7uoftY)

<div dir="rtl">
شرح ألية عمل الذاكرة المؤقتة "العشوائية" على لينكس، وأهمية السواب SWAP على النظام، وكيفية إنشاء ملف SWAP وتعديله أو حذفه، وأخيرًا كيفية تنصيب وتعيين تقنية الـ ZRAM لأداء أفضل على الأجهزة القديمة والحديثة، إضافة إلى خطوات إيقاف وحذف الـ ZRAM.
</div>

<p hidden>#more</p>

## ZRAM

### Install and modify ZRAM Service

- Install:

```sh
sudo apt install zram-tools
```

- Configure it to allow up to 60% of the RAM to be used as a zstd compressed swap space:

```sh
echo -e "ALGO=zstd\nPERCENT=70" | sudo tee -a /etc/default/zramswap
```

> I set it to 80%

- Restart the service

```sh
sudo service zramswap reload
```

### Remove zram

```sh
sudo service zramswap stop
```
```sh
sudo apt purge zram-tools
```

## SWAP File

### Create a SWAP File

- Creat 4G file

```sh
sudo dd if=/dev/zero of=/swapfile bs=1024 count=4194304
```
or

```sh
sudo fallocate -l 4G /swapfile
```

Verify the file size:

```sh
ls -lh /swapfile
```

- Set file permissions

```sh
sudo chmod 600 /swapfile
```

Verify the permissions:

```sh
ls -lh /swapfile
```

- Format the file as swap

```sh
sudo mkswap /swapfile
```

- Set it active

```sh
sudo swapon /swapfile
```

- Verify it is available:

```sh
sudo swapon --show
```

- Add it to `fstab`

backup fstab

```sh
sudo cp /etc/fstab /etc/fstab.bak
```

append it to `fstab`

```sh
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
```

or

open `fstab`

```sh
sudo nano /etc/fstab
```

add it by `PATH`

``` ini
# Dedicated swap file created on 221109
/swapfile   none    swap    sw      0       0
```

### Remove the SWAP File:

- Delete it's line from `fstab`

- Stop it

```sh
sudo swapoff /swapfile
```

- Remove the file:

```sh
sudo rm /swapfile
```

## List all available SWAP

```sh
sudo swapon --show
```