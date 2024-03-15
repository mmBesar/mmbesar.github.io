---
template: blog_post.html
title: Home Server - File Browser
description: Home Server | File Browser | الوصول لملفات السيرفر ومشاركتها عبر المتصفح
date: 2024-03-15
---

# <div dir="rtl">الوصول لملفات السيرفر ومشاركتها عبر المتصفح</div>

![type:video](https://www.youtube.com/embed/as89VX5da6I)

<div dir="rtl">
تنصيب وضبط خدمة File Browser على السيرفر المنزلي، للوصول إلى ملفات السيرفر المنزلي، ومشاركتها عبر أي متصفح.
</div>

<p hidden>#more</p>

## <div dir="rtl">إعداد مجلدات الخدمة</div>

```sh
mkdir -p /mnt/srv/docker/cont/filebrowser/{config,data/{home,srv}}
```

## <div dir="rtl">إعداد ملفات الخدمة</div>

```sh
touch /mnt/srv/docker/cont/filebrowser/config/{database.db,settings.json}
```

## <div dir="rtl">ضبط ملف الإعدادات</div>

```sh
nano /mnt/srv/docker/cont/filebrowser/config/settings.json
```

``` json title="settings.json"
{
  "port": 80,
  "baseURL": "",
  "address": "",
  "log": "stdout",
  "database": "/database.db",
  "root": "/data"
}
```

## <div dir="rtl">إضافة الخدمة إلى docker-compose file</div>

<div dir="rtl">بالأمر:</div>

```sh
nano /mnt/srv/docker/comp/docker-compose.yml
```

<div dir="rtl">وإضافة ما يلي:</div>

``` yaml title="docker-compose.yml"

 # =====================================
  filebrowser:
    image: docker.io/filebrowser/filebrowser:v2
    container_name: filebrowser
    networks:
      - hs
    user: 1000:1000
    volumes:
      - /mnt/srv/docker/cont/filebrowser/data:/data
      - /mnt/srv:/data/srv
      - /home/<Your_UserName>:/data/home
      - /mnt/srv/docker/cont/filebrowser/config/database.db:/database.db
      - /mnt/srv/docker/cont/filebrowser/config/settings.json:/.filebrowser.json
      - /etc/timezone:/etc/timezone:ro
      - /etc/localtime:/etc/localtime:ro
    ports:
      - 8098:80
    restart: unless-stopped
```

## <div dir="rtl">إنشاء وتشغيل الخدمة</div>

```sh
docker compose -f /mnt/srv/docker/comp/docker-compose.yml up -d
```

## <div dir="rtl">الوصول للخدمة عبر المُتصفح:</div>

http://SERVER-IP:8098

## <div dir="rtl">وصول أسرع عبر اسم مُختصر:</div>

<div dir="rtl">راجع الحلقات التالية ثم الحلقة أعلاه</div>

![type:video](https://www.youtube.com/embed/3MJxOnf0Hlc)

![type:video](https://www.youtube.com/embed/emLFTyf31MQ)

## <div dir="rtl">مراجع</div>

- [File Browser](https://filebrowser.org/)
- [File Browser Docker Image](https://hub.docker.com/r/filebrowser/filebrowser)
- [File Browser on GitHub](https://github.com/filebrowser/filebrowser)