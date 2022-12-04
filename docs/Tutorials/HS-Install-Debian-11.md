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

## <div dir="rtl">تحديث النظام</div>

```sh
apt update && apt upgrade
```

## <div dir="rtl">تنصيب `sudo`</div>

```sh
apt install sudo
```

## <div dir="rtl">إضافة حسابك إلى مجموعة `sudo`</div>

```sh
gpasswd -a $USER sudo
```

## <div dir="rtl">ضبط إعدادات locale</div>

```sh
export PATH=$PATH:/usr/sbin &&
export LANGUAGE=en_US.UTF-8 &&
export LANG=en_US.UTF-8 &&
export LC_ALL=en_US.UTF-8 &&
locale-gen en_US.UTF-8
```

## Swap

### zram

- <div dir="rtl">تنصيب أدوات `Z-RAM`</div>

```sh
sudo apt install zram-tools
```

- <div dir="rtl">ضبط الإعدادات على 70% من مساحة الرام:</div>

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

<div dir="rtl">أخذ نسخة إحتياطية من الملف الأصلي</div>

```sh
sudo cp /etc/fstab /etc/fstab.bak
```

<div dir="rtl">تعديل ملف الـ fstab</div>

```sh
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
```

### <div dir="rtl">مراجعة الاعدادات</div>

```sh
sudo swapon --show
```

## <div dir="rtl">إضافة مستودعات none-free</div>

!!! danger "ملاحظة هامة"

    <div dir="rtl">
    التنصيب تم باستخدام  
    `debian-firmware-11.5.0-amd64-netinst.iso`
    </div>


```sh
cat /etc/apt/sources.list | grep "bullseye main contrib non-free" || sudo sed -i "s/bullseye main/bullseye main contrib non-free/g" /etc/apt/sources.list
```

```sh
sudo apt update
```

## <div dir="rtl">المعالج Microcode</div>

=== "Intel معالجات"

    ``` sh
    sudo apt install intel-microcode
    ```

=== "AMD معالجات"

    ``` sh
    sudo apt install amd64-microcode
    ```
    