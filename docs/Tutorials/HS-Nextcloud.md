---
template: blog_post.html
title: Home Server - Install Nextcloud
description: Home Server | Nextcloud تنصيب وإعداد
date: 2022-12-30
---

# <div dir="rtl">تنصيب وإعداد Nextcloud على السيرفر المنزلي</div>

![type:video](https://www.youtube.com/embed/00000000000000000)

<div dir="rtl">
تنصيب وتشغيل خدمة Nextcloud على السيرفر المنزلي.
</div>
<div dir="rtl">
تطبيق Nextcloud يقدم لك الكثير من الخِدْمَات السحابية، على السيرفر المنزلي، وبشكل يحفظ لك خصوصيتك.
</div>
<div dir="rtl">
خِدْمَات عدّة، مثل مزامنة ومشاركة الملفات والصور والاحتفاظ بنسخ احتياطية بشكل دائم منها، مزامنة جهات الاتصال والمهام والمواعيد، ومشاركتها مع أفراد الأسرة.
</div>

<p hidden>#more</p>

## <div dir="rtl">إعداد مجلدات الخدمة</div>

```sh
mkdir -p /mnt/srv/docker/cont/nextcloud/{db,html,db-backup} /mnt/srv/data/nc-data
```

## <div dir="rtl">إضافة الخدمة إلى docker-compose file</div>

<div dir="rtl">بالأمر:</div>

```sh
nano /mnt/srv/docker/comp/docker-compose.yml
```

<div dir="rtl">وإضافة ما يلي:</div>

``` yaml title="docker-compose.yml"

  nextcloud:
    image: nextcloud:25
    container_name: nextcloud
    user: 1000:1000
    networks:
      - hs
    links:
      - nextcloud-db
    hostname: nc.vs-yt.mm
    environment:
      - TZ=Africa/Cairo
      - POSTGRES_DB=nextcloud
      - POSTGRES_USER=nextcloud
      - POSTGRES_PASSWORD=nextcloud
      - POSTGRES_HOST=nextcloud-db
      - NEXTCLOUD_TRUSTED_DOMAINS=localhost nextcloud 192.168.100.76:8080 nc.vs-yt.mm *.vs-yt.mm
      # - NEXTCLOUD_ADMIN_USER=mbesar
      # - NEXTCLOUD_ADMIN_PASSWORD=password
      # - NEXTCLOUD_DATA_DIR=/var/www/html/data # if you need to change data folder
    volumes:
      - /mnt/srv/docker/cont/nextcloud/html:/var/www/html
      - /mnt/srv/data/nc-data:/var/www/html/data
    ports:
      - 8080:80
    restart: unless-stopped
 #--------------------------------------
  nextcloud-db:
    image: postgres:15
    container_name: nextcloud-db
    user: 1000:1000
    networks:
      - hs
    environment:
      - POSTGRES_USER=nextcloud
      - POSTGRES_PASSWORD=nextcloud
      - POSTGRES_DB=nextcloud
    volumes:
      - /mnt/srv/docker/cont/nextcloud/db:/var/lib/postgresql/data
      - /mnt/srv/docker/cont/nextcloud/db-backup:/var/lib/postgresql/backup
      - /etc/timezone:/etc/timezone:ro
      - /etc/localtime:/etc/localtime:ro
    restart: unless-stopped

```

## <div dir="rtl">إنشاء وتشغيل الخدمة</div>

```sh
docker compose -f /mnt/srv/docker/comp/docker-compose.yml up -d
```

## <div dir="rtl">مراجع</div>

- [Nextcloud الموقع الرسمي](https://nextcloud.com/)
- [Nextcloud Docker Image](https://hub.docker.com/_/nextcloud)
- [PostgreSQL Docker Image](https://hub.docker.com/_/postgres)
- [Nextcloud تطبيقات](https://nextcloud.com/install/)
- [Nextcloud التطبيق الأساسي لمزامنة الملفات](https://f-droid.org/en/packages/com.nextcloud.client/)
- [Nextcloud تطبيق الملاحظات](https://f-droid.org/en/packages/it.niedermann.owncloud.notes/)
- [Nextcloud تطبيق كروت المهام](https://f-droid.org/en/packages/it.niedermann.nextcloud.deck/)
- [DAVx⁵ لمزامنة جهات الاتصال والمواعيد](https://f-droid.org/en/packages/at.bitfire.davdroid/)
- [المزيد من التطبيقات](https://search.f-droid.org/?q=nextcloud&lang=en)
