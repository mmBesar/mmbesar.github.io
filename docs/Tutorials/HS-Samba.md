---
template: blog_post.html
title: Home Server - Install and Configure Samba
description: Home Server | خدمة مشاركة الملفات
date: 2022-12-06
---

# Home Server | مشاركة الملفات بين ويندوز وأندرويد ولينكس من خلال السيرفر المنزلي

![type:video](https://www.youtube.com/embed/ZeeaScxN3ZQ)

<div dir="rtl">
أول خدمة على السيرفر المنزلي، إنشاء نظام متكامل لمشاركة الملفات بين أنظمة التشغيل المختفلة، والوصول إلى ملفاتك من خلال ويندوز أو أندرويد وبالتأكيد لينكس.
</div>
<div dir="rtl">
شرح مفصل لكيفية إنشاء Samba Share على لينكس، وضبط إعدادات وصلاحيات المستخدم، وصلاحيات الوصول إلى الملفات.
</div>

<p hidden>#more</p>

## <div dir="rtl">تحديث النظام</div>

```sh
sudo apt update && sudo apt upgrade -y
```

## <div dir="rtl">تنصيب Samba</div>

```sh
sudo apt install samba
```

## <div dir="rtl">إضافة حسابك على Samba</div>

```sh
sudo smbpasswd -a $USER
```

## <div dir="rtl">إنشاء حساب للمشاركة وإضافته إلى Samba</div>

### <div dir="rtl">إنشاء الحساب</div>

```sh
sudo adduser --no-create-home --disabled-password --disabled-login share
```

### <div dir="rtl">إضافته إلى Samba</div>

```sh
sudo smbpasswd -a share
```

## <div dir="rtl">إنشاء مجلد للمشاركة</div>

### <div dir="rtl">إنشاء المجلد</div>

```sh
sudo mkdir /mnt/srv/share
```

### <div dir="rtl">ضبط صلاحيات المجلد</div>

```sh
sudo chown -R $USER:$USER /mnt/srv/share
```

## <div dir="rtl">ضبط إعدادات Samba</div>

<div dir="rtl">أخذ نسخة إحتياطية من الملف الأصلي</div>

```sh
sudo mv /etc/samba/smb.conf /etc/samba/smb.conf.bk
```

<div dir="rtl">إنشاء ملف جديد</div>

```sh
sudo nano /etc/samba/smb.conf
```

<div dir="rtl">وإضافة ما يلي:</div>

!!! danger ""

    <div dir="rtl">تأكد من تعديل البيانات لتناسبك</div>

``` ini title="smb.conf"
[global]
server string = Virtual Server
netbios name = VSERVER
workgroup = WORKGROUP
security = user
map to guest = Bad User
name resolve order = bcast host
include = registry
passdb backend = tdbsam

[home]
path = /home/mbesar
browsable = yes
valid users = mbesar
write list = mbesar
read list = mbesar

[srv]
path = /mnt/srv
browsable = yes
valid users = mbesar
write list = mbesar
read list = mbesar

[share]
path = /mnt/srv/share
browsable = yes
writable = yes
valid users = mbesar share
create mask = 0755
force create mode = 0755
directory mask = 0755
force directory mode = 0755

[printers]
comment = All Printers
browseable = yes
path = /var/spool/samba
printable = yes
guest ok = yes
read only = no
create mask = 0700
```

<div dir="rtl">تفعيل الخدمة</div>

```sh
sudo systemctl enable --now {s,n}mbd
```

<div dir="rtl">إعادة تشغيل الخدمة</div>

```sh
sudo systemctl restart {s,n}mbd
```

## <div dir="rtl">السماح لحساب share بالوصول الكامل لملفات المجلد share</div>

```sh
chmod 0777 /mnt/srv/share
```

## <div dir="rtl">الوصول السريع لمجلدات السيرفر من خلال أنظمة لينكس المكتبية</div>

### <div dir="rtl">تنصيب حزمة cifs-utils</div>

=== "Fedora"

    ``` sh
    sudo dnf install cifs-utils
    ```

=== "Ubuntu"

    ``` sh
    sudo apt install cifs-utils
    ```

=== "Arch"

    ``` sh
    sudo pacman -Sy cifs-utils
    ```

### <div dir="rtl">إنشاء مجلد جديد لتخصيصه نقطة وصول</div>

```sh
mkdir ~/share
```

### <div dir="rtl">إنشاء ملف بيانات الوصول</div>

```sh
nano ~/smb.cred
```

<div dir="rtl">وأضف البيانات كالتالي:</div>

``` ini title="smb.cred"
username=USER-NAME
password=PASSWORD
```

### <div dir="rtl">إضافة بيانات السيرفر إلى الـ FSTAB</div>

```sh
sudo nano /etc/fstab
```

<div dir="rtl">وإضافة السطر التالي:</div>

!!! danger ""

    <div dir="rtl">تأكد من تعديل البيانات لتناسبك</div>

``` title="fstab"
//server_IP/share_name  /home/YOUR_USER_NAME/share  cifs  credentials=/home/YOUR_USER_NAME/smb.cred,noperm  0  0
```