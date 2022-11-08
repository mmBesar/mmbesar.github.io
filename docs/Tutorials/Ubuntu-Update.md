---
template: blog_post.html
title: Ubuntu Update
description: Ubuntu | تحديث أبونتو وكل التطبيقات بكلمة واحدة
date: 2022-10-07
---

# Update Ubuntu

![type:video](https://www.youtube.com/embed/iqeKB1fCijs)

<div dir="rtl">
تحديث توزيعة أبونتو أو ديبيان، وكل التوزيعات المبنية عليهم بكلمة واحدة فقط من الطرفية. التحديث يشمل كل المستودعات المختلفة.
</div>

<p hidden>#more</p>

## from Terminal

```sh
sudo apt update && sudo apt upgrade -y && flatpak update -y && sudo snap refresh
```

## add it as an alias

add the below line to `.bashrs` or `.zshrc`

```sh
alias update='sudo apt update && sudo apt upgrade -y && flatpak update -y && sudo snap refresh'
```
