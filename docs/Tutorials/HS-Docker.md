---
template: blog_post.html
title: Home Server - Install and Use Docker
description: Home Server | Docker | نظام الحاويات واستخدامه على السيرفر المنزلي
date: 2022-12-19
---

# <div dir="rtl">السيرفر المنزلي | تنصيب واستخدام Docker و Docker Compose</div>

![type:video](https://www.youtube.com/embed/i1uOZXsU96M)

<div dir="rtl">
أسهل طريقة لتوفير عديد من الخِدْمَات على السيرفر المنزلي، هى نظام الحاويات أو الـ Containers، باستخدام Docker أشهر تطبيق لصناعة وإدارة الحاويات.
</div>
<div dir="rtl">
نشرح في الفيديو التقنية وكيفية تنصيها على السيرفر، وأقوى وأسهل أدواتها Docker Compose، وكيفية كتابة المِلَفّ واستخدامه.
</div>

<p hidden>#more</p>

## <div dir="rtl">إعداد الحزم والمستودعات</div>


<div dir="rtl">تحديث النظام</div>

```sh
sudo apt update
```

<div dir="rtl">تنصيب الحزم اللازمة</div>

```sh
sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
```

<div dir="rtl">إضافة مفاتيح المستودعات</div>

```sh
sudo mkdir -p /etc/apt/keyrings
```

```sh
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

<div dir="rtl">تنصيب المستودعات</div>

```sh
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

## <div dir="rtl">تنصيب Docker</div>

<div dir="rtl">تحديث النظام</div>

```sh
sudo apt update
```

<div dir="rtl">تنصيب التطبيقات</div>

```sh
sudo apt install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

## <div dir="rtl">إضافة اسم المستخدم الحالي إلى مجموعة Docker</div>

```sh
sudo usermod -aG docker $USER
```

## <div dir="rtl">إعداد مجلدات Docker</div>

<div dir="rtl">إنشاء المجلدات</div>

```sh
sudo mkdir -p /mnt/srv/{docker/{comp,cont},data}
```

<div dir="rtl">ضبط الصلاحيات</div>

```sh
sudo chown -R $USER:$USER /mnt/srv/{docker,data}
```

## <div dir="rtl">إعداد مجلدات الخدمات للخطوة التالية</div>

```sh
mkdir -p /mnt/srv/docker/cont/{nginx,portainer}
```

## <div dir="rtl">إنشاء docker-compose file</div>

<div dir="rtl">بالأمر:</div>

```sh
nano /mnt/srv/docker/comp/docker-compose.yml
```

<div dir="rtl">وإضافة ما يلي:</div>

``` yaml title="docker-compose.yml"

version: "3.9"

networks:
  hs:
    driver: bridge
    ipam:
     config:
       - subnet: 172.14.4.0/24

services:

 # =====================================
  portainer:
    image: portainer/portainer-ce
    container_name: portainer
    networks:
      hs:
    command: --no-analytics
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - /mnt/srv/docker/cont/portainer:/data
      - /etc/timezone:/etc/timezone:ro
      - /etc/localtime:/etc/localtime:ro
    ports:
      - 9000:9000
      - 8000:8000
      - 9443:9443
    restart: always
  # =====================================
  nginx:
    image: lscr.io/linuxserver/nginx
    container_name: nginx
    networks:
      hs:
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Africa/Cairo
    volumes:
      - /mnt/srv/docker/cont/nginx:/config
    ports:
      - 8082:80
      #- 443:443
    restart: unless-stopped
 # =====================================

```

## <div dir="rtl">كيفية استخدام أمر Docker Compose</div>

<div dir="rtl">تحميل صور الحاويات:</div>
```sh
docker compose -f /mnt/srv/docker/comp/docker-compose.yml pull
```

<div dir="rtl">إنشاء وتشغيل الخدمات:</div>
```sh
docker compose -f /mnt/srv/docker/comp/docker-compose.yml up -d
```

<div dir="rtl">إيقاف الخدمات:</div>
```sh
docker compose -f /mnt/srv/docker/comp/docker-compose.yml stop
```

## <div dir="rtl">مراجع</div>

- [Dokcer on Debian 11](https://docs.docker.com/engine/install/debian/)
- [Nginx Image](https://github.com/linuxserver/docker-nginx)
- [Portainer Image](https://docs.portainer.io/start/install/server/docker/linux)
- [Docker Compose](https://docs.docker.com/get-started/08_using_compose/)
