---
template: blog_post.html
title: Home Server - Upgrade to Debian 12
description: Home Server | Debian 12 | ترقية نظام تشغيل السيرفر المنزلي إلى ديبيان 12
date: 2023-11-08
---

# <div dir="rtl">ترقية نظام تشغيل السيرفر المنزلي إلى ديبيان 12</div>

![type:video](https://www.youtube.com/embed/nFUa3xbBX5w)

<div dir="rtl">
ترقية نظام تشغيل السيرفر المنزلي، من ديبيان 11 إلى ديبيان 12.
شرح الترقية خطوة بخطوة.
</div>

<p hidden>#more</p>

## Debian 11 bullseye to Debian 12 bookworm

1. Update the system

```sh
sudo apt update
```

```sh
sudo apt upgrade
```

```sh
sudo apt full-upgrade
```

2. Update sources list

```sh
sudo sed -i "s/bullseye/bookworm/g" /etc/apt/sources.list
```

for docker:

```sh
sudo sed -i "s/bullseye/bookworm/g" /etc/apt/sources.list.d/docker.list
```

3. update

```sh
sudo apt update && sudo apt upgrade
```