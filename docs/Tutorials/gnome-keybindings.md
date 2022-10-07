---
template: blog_post.html
title: GNOME  Keybindings
description: ضبط اختصارات لوحة المفاتيح على واجهة جنوم
date: 2022-07-08
---

# gnome-keybindings

<div dir="rtl">
تخصيص وضبط اختصارات لوحة المفاتيح على واجهة جنوم
</div>

<p hidden>#more</p>

## Set `switch-input-source` to `Alt+Shift` open terminal and run:

* Set `switch-input-source` switch to Shift+Alt LEFT

```bash
gsettings set org.gnome.desktop.wm.keybindings switch-input-source "['<Shift>Alt_L']"
```

* set `switch-input-source-backward` to Alt+Shift LEFT

```bash
gsettings set org.gnome.desktop.wm.keybindings switch-input-source-backward "['<Alt>Shift_L']"
```

to view current keybindings run:

```bash
gsettings get org.gnome.desktop.wm.keybindings switch-input-source
gsettings get org.gnome.desktop.wm.keybindings switch-input-source-backward
```
