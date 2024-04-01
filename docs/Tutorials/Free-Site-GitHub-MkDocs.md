---
template: blog_post.html
title: Free Site with GitHub and MkDocs
description: خطوات إنشاء موقع مجاني بالكامل
date: 2024-04-01
---

# <div dir="rtl">خطوات إنشاء موقع مجاني بالكامل</div>

![type:video](https://www.youtube.com/embed/f5FsE6Tf3PE)

<div dir="rtl">
شرح إنشاء موقع مجاني باستضافة مجانية على GitHab Pages، وإدارة التعديل والإضافة على الموقع من خلال جهازك الشخصي، ونشر الموقع يتم بشكل تلقائي باستخدام GitHub Actions. محتوى الموقع يتم كتابته باستخدام MarkDown، وتطبيق أو أداة MkDocs تتولى عنك انشاء ملفات الموقع النهائية من HTML و CSS و javascript.
</div>

<p hidden>#more</p>

## أهم الأوامر

* <div dir="rtl">نسخ الموقع من على</div>

```sh
git clone git@github.com:<userName>/<repoName>.git
```

* <div dir="rtl">إنشاء ملفات الموقع الأساسية</div>

```sh
mkdocs new <repoName>
```

* <div dir="rtl">تجربة الموقع محليًا</div>

```sh
mkdocs serve
```

* <div dir="rtl">إنشاء مجلد `.github/workflows/`</div>

```sh
mkdir -p .github/workflows/
```

* <div dir="rtl">إنشاء ملف `.github/workflows/gh-deploy.yml`</div>

```sh
nano .github/workflows/gh-deploy.yml
```

<div dir="rtl">بالمحتوى التالي:</div>

```yml
name: gh-deploy

on:
  push:
    branches:
      - main

jobs:
  build:
    name: MkDocs Github Pages automatic deployment
    runs-on: ubuntu-latest
    steps:
      - name: Checkout main
        uses: actions/checkout@v2

      - name: Set up Python 3.9
        uses: actions/setup-python@v2
        with:
          python-version: '3.9'

      - name: Install requirements
        run: |
          python -m pip install --upgrade pip
          pip install -r requirements.txt
      - name: MkDocs gh-deploy
        run: |
          git pull
          mkdocs gh-deploy
```

* <div dir="rtl">إنشاء ملف `requirements.txt`</div>

```sh
nano requirements.txt
```

<div dir="rtl">بالمحتوى التالي:</div>

```ini
mkdocs>=1.2.2
```

* <div dir="rtl">إرسال ملفات الموقع إلى GitHub</div>

```sh
git add .
git commit -m 'init'
git push
mkdocs gh-deploy
rm -r site
```

* <div dir="rtl">استخدم الأوامر التالية بالتريب بعد إجراء أي تعديل على الموقع:</div>

```sh
git add .
git commit -m 'TEST01'
git push
```

## <div dir="rtl">مراجع</div>

- [MkDocs](https://www.mkdocs.org/)