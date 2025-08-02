---
template: blog_post.html
title: Home Server - Radicale
description: Home Server | Radicale | الحل النهائي لمزامنة جهات الاتصال والمواعيد والمهام بين كل أجهزتك
date: 2025-08-01
---

# <div dir="rtl">مزامنة جهات الاتصال والمواعيد والمهام بين كل الأجهزة عبر السيرفر المنزلي</div>

![type:video](https://www.youtube.com/embed/jcChfIyq224)

<div dir="rtl">
في هذا الفيديو ستتعلم كيفية تثبيت خادم Radicale على خادم منزلي باستخدام Docker، وهو خادم CalDAV وCardDAV خفيف وفعّال لإدارة التقويمات والمهام وجهات الاتصال.
</div>
<div dir="rtl">
سننتقل لاحقًا إلى نقل جهات الاتصال وجوجل تقويم من حسابك في Google إلى Radicale لتضمن خصوصية بياناتك وسيطرتها الكاملة عليك.
</div>
<div dir="rtl">
ثم نشرح بالتفصيل طريقة إعداد تطبيق DAVx⁵ على Android لربط هاتفك بالخادم المنزلي ومزامنة التقاويم والمهام تلقائيًا.
</div>
<div dir="rtl">
وفي الخطوة الأخيرة، نعرض كيفية اعداد Thunderbird على Linux ليتزامن بسلاسة مع خادم Radicale نفسه، لضمان تحديث البيانات على جميع أجهزتك.
</div>
<div dir="rtl">
تابع هذا الدليل الشامل للحصول على حل متكامل لمزامنة المواعيد والتقاويم "النتيجة" وجهات الاتصال والمهام عبر كل أجهزتك بأمان وسهولة!
</div>

<p hidden>#more</p>

## <div dir="rtl">إعداد مجلدات الخدمة</div>

```sh
mkdir -p /srv/containers/radicale/{config,data}
```

## <div dir="rtl">إعداد ملفات الخدمة</div>

```sh
touch /srv/containers/radicale/config/{config,users}
```

## <div dir="rtl">ضبط ملف الإعدادات</div>

```sh
nano /srv/containers/radicale/config/config
```

``` ini title="config"
[server]
hosts = 0.0.0.0:5232

[auth]
type = htpasswd
htpasswd_filename = /etc/radicale/users
htpasswd_encryption = bcrypt

[storage]
filesystem_folder = /var/lib/radicale/collections

```

## <div dir="rtl">تنصيب التطبيقات المطلوبة</div>

```sh
sudo apt install apache2-utils
```

## <div dir="rtl">انشاء وضبط المستخدمين</div>

```sh
htpasswd -Bb /srv/containers/radicale/config/users USERNAME PASSWORD
```

## <div dir="rtl">إضافة الخدمة إلى docker-compose file</div>

<div dir="rtl">بالأمر:</div>

```sh
nano /srv/docker/compose.yml
```

<div dir="rtl">وإضافة ما يلي:</div>

``` yaml title="compose.yml"
name: vs

networks:

  default:
    driver: bridge
    name: net
    ipam:
     config:
      - subnet: 172.24.44.0/24

services:

  radicale:
    image: ghcr.io/mmbesar/radicale:latest
    container_name: radicale
    networks:
      - ${NETWORK}
    environment:
      UID: ${PUID}
      GID: ${PGID}
      TZ: ${TZ}
    volumes:
      - ${CONTAINER_DIR}/radicale/data:/var/lib/radicale
      - ${CONTAINER_DIR}/radicale/config:/etc/radicale
    ports:
      - 5232:5232
    restart: always
```

## <div dir="rtl">إضافة الخدمة إلى docker-compose file</div>

<div dir="rtl">بالأمر:</div>

```sh
nano /srv/docker/.env
```


``` ini title=".env"
NETWORK=default
TZ=Africa/Cairo
PUID=1000
PGID=1000
CONTAINER_DIR=/srv/containers
DATA_DIR=/srv/data
```

## <div dir="rtl">إنشاء وتشغيل الخدمة</div>


```sh
docker compose -f /srv/docker/compose.yml up -d
```

## <div dir="rtl">الوصول للخدمة عبر المُتصفح:</div>

http://SERVER-IP:5232

## <div dir="rtl">وصول أسرع عبر اسم مُختصر:</div>

<div dir="rtl">راجع الحلقات التالية ثم الحلقة أعلاه</div>

![type:video](https://www.youtube.com/embed/3MJxOnf0Hlc)

![type:video](https://www.youtube.com/embed/emLFTyf31MQ)


## Google to Radicale

### Contacts:

```bash
curl -u mbesar:P@55w0rd \
     -X PUT "http://radicale.vs.mm/mbesar/gcontacts" \
     --data-binary @contacts.vcf
```

### Calendars:

```bash
curl -u mbesar:P@55w0rd \
     -X PUT "http://radicale.vs.mm/mbesar/gcalendar" \
     --data-binary @calendar.ics
```

## <div dir="rtl">مراجع</div>

- [Radicale](https://radicale.org/)
- [Radicale on GitHub](https://github.com/Kozea/Radicale)
- [Radicale Container Image](https://github.com/mmBesar/Radicale/pkgs/container/radicale)