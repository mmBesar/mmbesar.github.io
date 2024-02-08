---
template: blog_post.html
title: Home Server - NFS
description: Home Server | تنصيب وضبط خدمة NFS لمشاركة ملفات السيرفر
date: 2024-02-09
---

# <div dir="rtl">تنصيب وضبط خدمة NFS لمشاركة ملفات السيرفر</div>

![type:video](https://www.youtube.com/embed/aeoD503cGdg)

<div dir="rtl">
شرح كيفية تنصيب وضبط خدمة NFS لوصول أسرع لملفات السيرفر المنزلي، نشرح بشكل عملي تنصيب الخدمة على السيرفر، وتنصيب الحزم والإعدادات اللازمة في جانب العميل "الجهاز الآخر".
</div>

<p hidden>#more</p>

## Server side

- Install

```sh
sudo apt install nfs-kernel-server
```

- create the share

```sh
sudo nano /etc/exports
```

```
/home/mbesar     192.168.101.0/24(rw,sync,no_root_squash,no_subtree_check)
/mnt/srv    192.168.101.0/24(rw,sync,no_root_squash,no_subtree_check)

```

- Restart the server

```sh
sudo systemctl restart nfs-kernel-server
```

## Client side

- Install autofs

```sh
sudo apt install autofs
```

- Edit the Master map file `/etc/auto.master`

```sh
sudo nano /etc/auto.master
```

add the below

```ini
/vs  /etc/auto.vs --ghost --timeout=180
```

- Create a map file `/etc/auto.vs`

```sh
sudo nano /etc/auto.vs
```
add the below

```ini                                            
vs-home -rw,intr,rsize=8192,wsize=8192,retrans=1,_netdev,timeo=900  192.168.101.101:/home/mbesar
vs-srv -rw,intr,rsize=8192,wsize=8192,retrans=1,_netdev,timeo=900  192.168.101.101:/mnt/srv
```

- Enable auotfs service
```sh
sudo systemctl enable autofs.service
```

- Start auotfs service
```sh
sudo systemctl start autofs.service
```

- Check service status
```sh
sudo systemctl status autofs.service
```

- Reload auotfs service
```sh
sudo systemctl reload autofs.service
```