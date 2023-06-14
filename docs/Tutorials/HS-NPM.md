---
template: blog_post.html
title: Home Server - Nginx Proxy Manager
description: Home Server | Nginx Proxy Manager | وصول سريع لكل الخدمات عبر الشبكة المحلية
date: 2023-06-14
---

# <div dir="rtl">استخدام خدمة Nginx Proxy Manager للوصول للخدمات عبر عنوان سهل</div>

![type:video](https://www.youtube.com/embed/emLFTyf31MQ)

<div dir="rtl">
شرح كيفية إضافة واستخدام Nginx Proxy Manager لتسهيل الوصول إلى كل خدمات السيرفر، عبر أسماء وروابط سهلة ومباشرة.
</div>

<p hidden>#more</p>

## <div dir="rtl">إعداد مجلدات الخدمة</div>

```sh
mkdir -p /mnt/srv/docker/cont/npm/{data,letsencrypt}
```

## <div dir="rtl">إضافة الخدمة إلى docker-compose file</div>

<div dir="rtl">بالأمر:</div>

```sh
nano /mnt/srv/docker/comp/docker-compose.yml
```

<div dir="rtl">وإضافة ما يلي:</div>

``` yaml title="docker-compose.yml"

 # =====================================
  npm:
    image: jc21/nginx-proxy-manager:latest
    container_name: npm
    networks:
      - hs
    environment:
      - DISABLE_IPV6=true # Uncomment this if IPv6 is not enabled on your host
      - TZ=Africa/Cairo
    volumes:
      - /mnt/srv/docker/cont/npm/data:/data
      - /mnt/srv/docker/cont/npm/letsencrypt:/etc/letsencrypt
    ports:
      - 80:80
      - 443:443
      - 81:81
    restart: always

```

## <div dir="rtl">إنشاء وتشغيل الخدمة</div>

```sh
docker compose -f /mnt/srv/docker/comp/docker-compose.yml up -d
```

## <div dir="rtl">صفحة الخدمة الرئيسية</div>

<div dir="rtl">
الوصول لصفحة الخدمة الرئيسية عبر IP السيرفر متبوعًا ببورت رقم 81.
</div>

<div dir="rtl">مثال:</div>

192.168.101.101:81

<div dir="rtl">بيانات تسجيل الدخول الافتراضية:</div>

Email | Password
--- | ---
admin@example.com | changeme

## <div dir="rtl">مراجع</div>

- [Nginx Proxy Manager](https://github.com/NginxProxyManager/nginx-proxy-manager)