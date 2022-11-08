---
template: blog_post.html
title: 2nd Drive on Linux
description: إضافة هارد أخر إلى لينكس وحل مشكلة عدم الإقلاع عند فصله
date: 2022-11-08
---

![type:video](https://www.youtube.com/embed/CPvJvY79vEw)

<div dir="rtl">
شرح كيفية إضافة هارد أخر إلى أي توزيعة من توزيعات لينكس بعد التنصيب، بحيث يتوفر للاستخدام كمساحة تخزين داخلية دائمة. شرح كيفية حل مشكلة عدم إقلاع النظام عند غياب أو فصل هذا الهارد.
</div>

<p hidden>#more</p>

# 2nd drive on Linux


## Add the hard drive via `fstab`

- Connect the drive to your system and start it.

- The drive should appear under `/media/$USER/`

- To make it auto mount after boot.

- Get Drive `UUID`

```sh
sudo blkid
```

- Add it to `fstab`

```sh
sudo nano /etc/fstab
```

```
UUID="107E6F7D6A8CD554" /mnt/2nd-ntfs ntfs defaults  0  0
```

## Missing hard drive from `fstab`

- You have to remove the missing hard dive from `fstab`

backup the original file first:

```sh
cp /etc/fstab /etc/fstab.bk
```

open `fstab` file:

```sh
sudo nano /etc/fstab
```

Add `#` at the start of the line or delete the line:

```
# UUID="107E6F7D6A8CD554" /mnt/2nd-ntfs ntfs defaults  0  0
```

reboot