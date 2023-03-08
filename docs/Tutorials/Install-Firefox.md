---
template: blog_post.html
title: Install Firefox and Use Profiles
description: Firefox | التنصيب الرسمي على أي توزيعة واستخدام أكثر من متصفح بتطبيق واحد
date: 2023-03-08
---

# <div dir="rtl">تنصيب وترقية فايرفوكس من الموقع الرسمي واستخدام خاصية البروفايل </div>

![type:video](https://www.youtube.com/embed/vR7vN5WWvwE)

<div dir="rtl">
طريقة تنصيب فايرفوكس من الموقع الرسمي على أي توزيعة، والتحديث والترقية التلقائية الفورية من المصدر مباشرة. ونشرح بشكل مُفصل كيفية استخدام خاصية البروفايل، لتوفير أكثر من متصفح بنفس التنصيب، لفصل الاستخدام الشخصي عن العمل.
</div>

<p hidden>#more</p>

## Install Firefox

1. Download [Firefox](https://www.mozilla.org/firefox/linux/?utm_medium=referral&utm_source=support.mozilla.org)
2. Open the download dir
```sh
cd ~/Downloads
```
2. Extract the app
```sh
tar xjf firefox-*.tar.bz2
```
3. Move or copy Firefox folder to /opt
```sh
sudo mv firefox /opt
```
4. Create a symlink to the Firefox executable
```sh
sudo ln -s /opt/firefox/firefox /usr/local/bin/firefox
```
5. Download the desktop file to `/usr/local/share/applications`
```sh
sudo wget https://raw.githubusercontent.com/mozilla/sumo-kb/main/install-firefox-linux/firefox.desktop -P /usr/local/share/applications
```

## Use Firefox profiles

### Bash script file

=== "personal.sh"

    ``` ini
    #!/bin/bash

    firefox --profile "$PWD/personal"
    ```

=== "work.sh"

    ``` ini
    #!/bin/bash

    firefox --profile "$PWD/work"
    ```

### Desktop file

=== "personal.desktop"

    ``` ini
    [Desktop Entry]
    Type=Application
    Name=Firefox Personal
    Comment=Firefox Official for Personal Use
    Path=/home/mbesar/Apps/Frefox-Pofiles
    Exec=/home/mbesar/Apps/Frefox-Pofiles/personal.sh
    Icon=/home/mbesar/Apps/Frefox-Pofiles/personal.png
    Categories=Browser;Internet;
    ```

=== "work.desktop"

    ``` ini
    [Desktop Entry]
    Type=Application
    Name=Firefox Work
    Comment=Firefox Official for Work
    Path=/home/mbesar/Apps/Frefox-Pofiles
    Exec=/home/mbesar/Apps/Frefox-Pofiles/work.sh
    Icon=/home/mbesar/Apps/Frefox-Pofiles/work.png
    Categories=Browser;Internet;
    ```

### Firfox Icon
Download the Icon form [wikimedia](https://commons.wikimedia.org/wiki/File:Firefox_logo,_2017.png)

## Reference

- [Firefox Official Installation Guide](https://support.mozilla.org/en-US/kb/install-firefox-linux)