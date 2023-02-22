---
template: blog_post.html
title: Fedora Silverblue Post Installation
description: Fedora Silverblue | أهم الخطوات بعد التنصيب
date: 2023-02-22
---

# <div dir="rtl">خطوات يجب تنفيذها بعد تنصيب Fedora Silverblue</div>

![type:video](https://www.youtube.com/embed/NO9sy07r2Oc)

<div dir="rtl">
توزيعة المستقبل مازلت بحاجة إلى بعض الخطوات الهامة بعد التنصيب.
</div>

<p hidden>#more</p>

## System upgrades

- Update the system

```sh
rpm-ostree upgrade
```

- preview which packages will be updated

```sh
rpm-ostree update --preview
```

## Auto Update

- set `AutomaticUpdatePolicy`

```sh
sudo nano /etc/rpm-ostreed.conf
```

=== "check"

    ``` ini
    [Daemon]
    AutomaticUpdatePolicy=check
    ```

=== "stage"

    ``` ini
    [Daemon]
    AutomaticUpdatePolicy=stage
    ```

- reload rpm-ostree

```sh
rpm-ostree reload
```

- enable the automatic timer

```sh
systemctl enable rpm-ostreed-automatic.timer --now
```

## full flathub

- add full flathub remote:

```sh
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```

- update installed remotes:

```sh
flatpak update --appstream
```

- update installed apps:

```sh
flatpak update
```

## RPM Fusion repos

=== "from Fedora DOCS"

    ``` sh
    sudo rpm-ostree install https://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm https://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
    ```

=== "from RPM Fusion DOCS"

    ``` sh
    sudo rpm-ostree install https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
    ```

## Layering packages

- Install package

```sh
rpm-ostree install htop
```

- Remove it

```sh
rpm-ostree uninstall htop
```

- check layered packages

```sh
rpm-ostree status
```

- remove all layered packages

```sh
rpm-ostree uninstall --all
```

- Packages to Layer

```sh
rpm-ostree install htop gnome-tweaks unrar
```

## Nvidia Drivers

!!! danger ""

    <div dir="rtl">
    يجب إيقاف تفعيل الإقلاع الآمن "Secure Boot"
    </div>



[Install Nvidia from RPM Fusion](https://rpmfusion.org/Howto/NVIDIA#OSTree_.28Silverblue.2FKinoite.2Fetc.29)

## Install apps from Copr

- install [scrcpy](https://copr.fedorainfracloud.org/coprs/zeno/scrcpy/) as an example

```sh
sudo wget https://copr.fedorainfracloud.org/coprs/zeno/scrcpy/repo/fedora-$releasever/zeno-scrcpy-fedora-$releasever.repo -O /etc/yum.repos.d/copr-scrcpy-zeno.repo
```

## Rollbak to recover system failure

- use grup to boot tow a working img and run:

```sh
rpm-ostree rollback
```

## Reference

- [Silverblue User Guide](https://docs.fedoraproject.org/en-US/fedora-silverblue/)
- [rpm-ostree Administration](https://coreos.github.io/rpm-ostree/administrator-handbook/)
- [rpm-ostreed.conf - Man Page](https://www.mankier.com/5/rpm-ostreed.conf)