---
template: blog_post.html
title: Tesseract OCR
description: Tesseract OCR | استخراج النصوص من الصور
date: 2022-07-26
---

# Tesseract OCR

![type:video](https://www.youtube.com/embed/9xfrD_LPk4M)

<div dir="rtl">
استخراج النصوص العربية وغير العربية من الصور، وتحويلها نصوص مكتوبة، يمكن التعامل معها من خلال أي محرر نصوص عادي.
باستخدام تطبيق Tesseract OCR بتقنية Optical Character Recognition، التطبيق مفتوح المصدر ومجاني ويقدم الدعم لـ 100 لغة، وفي المقدمة اللغة العربية واللغة الإنجليزية.
</div>

<p hidden>#more</p>

## About Tesseract OCR - تعريف بالأداة

[wikipedia](https://en.wikipedia.org/wiki/Tesseract_(software))  
[Github](https://github.com/tesseract-ocr/tesseract)  

## install - التنصيب

- Fedora

>pre-installed on Fedora 36

```sh
sudo dnf install tesseract
```

- Ubuntu

```sh
sudo apt install tesseract-ocr
```

- Arch

```sh
sudo pacman -Sy
sudo pacman -S tesseract-data-eng
```

## Arabic Support - دعم اللغة العربية

- Fedora

```sh
sudo dnf install tesseract-langpack-ara
```

- Ubuntu

```sh
sudo apt install tesseract-ocr-ara
```

- Arch

```sh
sudo pacman -S tesseract-data-ara
```

## Usage - الاستخدام

- Arabic only - اللغة العربية فقط

```sh
tesseract -l ara image.png text
```

- English only - اللغة الإنجليزية فقط

```sh
tesseract -l eng image.png text
```

- Arabic and English - العربية والإنجليزية معًا

```sh
tesseract -l ara+eng image.png text
```

> More Examples - مزيد من الأمثلة: [here - هنا](https://tesseract-ocr.github.io/tessdoc/Command-Line-Usage.html)
