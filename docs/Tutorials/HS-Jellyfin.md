---
template: blog_post.html
title: Home Server - Install Jellyfin
description: Home Server | Jellyfin تنصيب وإعداد
date: 2022-01-27
---

# <div dir="rtl">تنصيب وإعداد Jellyfin على السيرفر المنزلي</div>

![type:video](https://www.youtube.com/embed/3qgMslcTUlM)

<div dir="rtl">
تنصيب وتشغيل خدمة Jellyfin على السيرفر المنزلي.
</div>
<div dir="rtl">
صناعة منصة وسائط متعددة الاستخدام مثل Netflix، ولكن دون إنترنت، وعلى السيرفر المنزلي.
</div>
<div dir="rtl">
تنصيب وإعداد خدمة Jellyfin على السيرفر المنزلي، وشرح كيفية إضافة ملفات الوسائط المتعددة "الميديا" إلى السيرفر.
</div>
<div dir="rtl">
الخدمة تسمح بمساحة واسعة لتخصيص مكتبات الفيديو والصوت حتى الصور، وتحديد صلاحيات الوصول لكل مستخدم.
</div>

<p hidden>#more</p>

## <div dir="rtl">إعداد مجلدات الخدمة</div>

```sh
mkdir -p /mnt/srv/docker/cont/jellyfin/config /mnt/srv/data/media
```

## <div dir="rtl">إضافة الخدمة إلى docker-compose file</div>

<div dir="rtl">بالأمر:</div>

```sh
nano /mnt/srv/docker/comp/docker-compose.yml
```

<div dir="rtl">وإضافة ما يلي:</div>

``` yaml title="docker-compose.yml"

  jellyfin:
    image: lscr.io/linuxserver/jellyfin
    container_name: jellyfin
    networks:
      - hs
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Africa/Cairo
      - JELLYFIN_PublishedServerUrl=http://jellyfin.vs-yt.mm #optional
    volumes:
      - /mnt/srv/docker/cont/jellyfin/config:/config
      - /mnt/srv/data/media:/data/media
    ports:
      - 8096:8096
      - 8920:8920 #optional
      - 7359:7359/udp #optional
      - 1900:1900/udp #optional
    restart: unless-stopped

```

## <div dir="rtl">إنشاء وتشغيل الخدمة</div>

```sh
docker compose -f /mnt/srv/docker/comp/docker-compose.yml up -d
```

## <div dir="rtl">مراجع</div>

- [Jellyfin الموقع الرسمي](https://jellyfin.org/)
- [Jellyfin Docker Image](https://hub.docker.com/r/linuxserver/jellyfin)
- [Jellyfin دليل استخدام](https://jellyfin.org/docs/)
- [Kid3 تطبيق](https://flathub.org/apps/details/org.kde.kid3)