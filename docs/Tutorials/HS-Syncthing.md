---
template: blog_post.html
title: Home Server - Syncthing
description: Home Server | Syncthing | مزامنة ملفاتك مع السيرفر المنزلي
date: 2023-09-05
---

# <div dir="rtl"></div>

![type:video](https://www.youtube.com/embed/Hy10VuIUuFI)

<div dir="rtl">
تنصيب وضبط خدمة Syncthing على السيرفر المنزلي، لمزامنة الملفات بشكل آمن، ومن أي مكان في العالم.
</div>

<p hidden>#more</p>

## <div dir="rtl">إعداد مجلدات الخدمة</div>

```sh
mkdir -p /mnt/srv/docker/cont/syncthing/config
```

## <div dir="rtl">إضافة الخدمة إلى docker-compose file</div>

<div dir="rtl">بالأمر:</div>

```sh
nano /mnt/srv/docker/comp/docker-compose.yml
```

<div dir="rtl">وإضافة ما يلي:</div>

``` yaml title="docker-compose.yml"

 # =====================================
  syncthing:
    image: lscr.io/linuxserver/syncthing:latest
    container_name: syncthing
    networks:
      - hs
    environment:
      PUID: 1000
      PGID: 1000
      TZ: Africa/Cairo
    volumes:
      - /mnt/srv/docker/cont/syncthing/config:/config
      - /mnt/srv/data:/data
    ports:
      - 8384:8384
      - 22000:22000/tcp
      - 22000:22000/udp
      - 21027:21027/udp
    restart: always
```

## <div dir="rtl">إنشاء وتشغيل الخدمة</div>

```sh
docker compose -f /mnt/srv/docker/comp/docker-compose.yml up -d
```

## <div dir="rtl">الوصول للخدمة عبر المُتصفح:</div>

http://SERVER-IP:8384

## <div dir="rtl">وصول أسرع عبر اسم مُختصر:</div>

<div dir="rtl">راجع الحلقات التالية ثم الحلقة أعلاه</div>

![type:video](https://www.youtube.com/embed/3MJxOnf0Hlc)

![type:video](https://www.youtube.com/embed/emLFTyf31MQ)

## <div dir="rtl">مراجع</div>

- [Syncthing](https://syncthing.net/)
- [Syncthing Docker Image](https://github.com/linuxserver/docker-syncthing)
- [Nginx Proxy Manager on GitHub](https://github.com/NginxProxyManager/nginx-proxy-manager)