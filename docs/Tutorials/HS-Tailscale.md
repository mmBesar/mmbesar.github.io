---
template: blog_post.html
title: Home Server - Tailscale
description: Home Server | Tailscale | وصول لخدمات السيرفر المنزلي من أي مكان في العالم
date: 2023-07-19
---

# <div dir="rtl">وصول لخدمات السيرفر المنزلي من أي مكان في العالم باستخدام Tailscale</div>

![type:video](https://www.youtube.com/embed/5VdHnwK1Jug)

<div dir="rtl">
الوصول الآمن لكل خدمات السيرفر المنزلي من أي مكان في العالم.
</div>

<p hidden>#more</p>

## <div dir="rtl">إعداد مجلدات الخدمة</div>

```sh
mkdir -p /mnt/srv/docker/cont/tailscale/{state,config}
```

## <div dir="rtl">إنشاء ملف الإعدادات:</div>

```sh
nano /mnt/srv/docker/cont/tailscale/config/99-tailscale.conf
```

<div dir="rtl">وأضف ما يلي:</div>

``` ini
net.ipv4.ip_forward = 1
net.ipv6.conf.all.forwarding = 1
```

## <div dir="rtl">إضافة الخدمة إلى docker-compose file</div>

<div dir="rtl">بالأمر:</div>

```sh
nano /mnt/srv/docker/comp/docker-compose.yml
```

<div dir="rtl">وإضافة ما يلي:</div>

``` yaml title="docker-compose.yml"

 # =====================================
  tailscale:
    image: tailscale/tailscale:stable
    container_name: tailscale
    hostname: docker
    networks:
      - hs
    privileged: true
    volumes:
      - /mnt/srv/docker/cont/tailscale/state:/var/lib/tailscale
      - /mnt/srv/docker/cont/tailscale/config/99-tailscale.conf:/etc/sysctl.d/99-tailscale.conf
      - /dev/net/tun:/dev/net/tun
      - /etc/timezone:/etc/timezone:ro
      - /etc/localtime:/etc/localtime:ro
    command:
      - tailscaled
    restart: always
```

## <div dir="rtl">إنشاء وتشغيل الخدمة</div>

```sh
docker compose -f /mnt/srv/docker/comp/docker-compose.yml up -d
```

## <div dir="rtl">تفعيل الاتصال بالخدمة</div>

<div dir="rtl">استبدل "YOUR-KEY" في الأمر التالي بمفتاحك الخاص.</div>

```sh
docker exec tailscale tailscale up --authkey=YOUR-KEY --advertise-routes=192.168.101.0/24
```

## <div dir="rtl">مراجع</div>

- [Tailscale](https://tailscale.com/)