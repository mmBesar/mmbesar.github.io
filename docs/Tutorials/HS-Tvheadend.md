---
template: blog_post.html
title: Home Server - Tvheadend
description: Home Server | Tvheadend | التليفزيون على أي جهاز
date: 2026-04-10
---

# <div dir="rtl">التليفزيون على أي جهاز</div>

![type:video](https://www.youtube.com/embed/K6P_xSLafE0)

<div dir="rtl">

</div>

<p hidden>#more</p>

## <div dir="rtl">تنصيب تعريفات TBS</div>

1. Install packages:
```bash
sudo apt install git build-essential patchutils dkms libproc-processtable-perl linux-headers-$(uname -r)
```

2. Clone the repos:
```bash
git clone https://github.com/tbsdtv/media_build.git
git clone --depth=1 https://github.com/tbsdtv/linux_media.git -b latest ./media
```

3. Compile and install the drivers:
```bash
cd media_build
make dir DIR=../media
make allyesconfig
sed -i -r 's/(^CONFIG.*_RC.*=)./\1n/g' v4l/.config
sed -i -r 's/(^CONFIG.*_IR.*=)./\1n/g' v4l/.config
make -j4
sudo make install
```

4. Firmware
```bash
wget http://www.tbsdtv.com/download/document/linux/tbs-tuner-firmwares_v1.0.tar.bz2
sudo tar jxvf tbs-tuner-firmwares_v1.0.tar.bz2 -C /lib/firmware/
```

5. Reboot
```bash
sudo reboot
```

## <div dir="rtl">إعداد مجلدات الخدمة</div>

```sh
mkdir -p /srv/docker/containers/tvheadend/config
```

```sh
mkdir -p /srv/media/record/tvheadend
```


## <div dir="rtl">إضافة الخدمة إلى docker-compose file</div>

<div dir="rtl">بالأمر:</div>

```sh
nano /srv/docker/compose/compose.yml
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

  tvheadend:
    image: lscr.io/linuxserver/tvheadend:latest
    container_name: tvheadend
    networks:
      - default
    environment:
      PUID: ${PUID}
      PGID: ${PGID}
      TZ: ${TZ}
    volumes:
      - ${CONTAINER_DIR}/tvheadend/config:/config
      - ${MEDIA_DIR}/record/tvheadend:/recordings
    ports:
      - 9981:9981
      - 9982:9982
    devices:
      - /dev/dri:/dev/dri #optional
      - /dev/dvb:/dev/dvb #optional
    restart: unless-stopped
```

## <div dir="rtl">إضافة الخدمة إلى docker-compose file</div>

<div dir="rtl">بالأمر:</div>

```sh
nano /srv/docker/.env
```


``` ini title=".env"
TZ=Africa/Cairo
PUID=1000
PGID=1000
CONTAINER_DIR=/srv/docker/containers
MEDIA_DIR=/srv/media
```

## <div dir="rtl">إنشاء وتشغيل الخدمة</div>


```sh
docker compose -f /srv/docker/compose/compose.yml up -d
```

## <div dir="rtl">الوصول للخدمة عبر المُتصفح:</div>

http://SERVER-IP:9981

## <div dir="rtl">وصول أسرع عبر اسم مُختصر:</div>

<div dir="rtl">راجع الحلقات التالية ثم الحلقة أعلاه</div>

![type:video](https://www.youtube.com/embed/3MJxOnf0Hlc)

![type:video](https://www.youtube.com/embed/emLFTyf31MQ)


## <div dir="rtl">مراجع</div>

- [TBS Drivers](https://github.com/tbsdtv/linux_media/wiki)
- [Tvheadend](https://tvheadend.org/)
- [Tvheadend on GitHub](https://github.com/tvheadend/tvheadend)
- [Tvheadend Container Image](https://github.com/linuxserver/docker-tvheadend)