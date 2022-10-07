---
template: blog_post.html
title: ZSH
description: ZSH | الطرفية بشكل أفضل وإضافات متميزة وتخصيص غير محدود
date: 2021-08-02
---

# ZSH

![type:video](https://www.youtube.com/embed/fDuGKsQ3bV4)

<div dir="rtl">
شرح zsh وتنصيب وتخصيص كامل، للحصول على طرفية ذات "واجهة" جذابة، وإضافات مهمة تيسر وتسهل استخدام الطرفية في مختلف المهام. الشرح مفيد سواء كنت مستخدمًا محترفًا أو مبتدئ على حد سواء.
</div>

<div dir="rtl">
واجهة Linux الرسومية تطورت كثيرًا خلال السنوات الأخيرة، بإمكانك استخدام معظم التوزيعات بدون الحاجة إلى التعامل مع الـ Terminal أو الطرفية!
</div>

<div dir="rtl">
لكن تظل الـ Terminal أحد أهم عناصر Linux، ومهما كان الـ GUI أو الواجهة متطورة ستظل الطرفية أسرع وأسهل - نعم أسهل - الطرق لتنفيذ عديد من المهام على جهازك!
</div>

<div dir="rtl">
لكن واجهة الطرفية - لو صح التعبير - ليست جذابة بواجهتها التقليدية، لذلك اليوم سوف نغير ذلك تمامًا، شكل جذاب وتخصيص كامل للطرفية.
</div>

<p hidden>#more</p>

## Update your system

* Ubuntu

```bash
sudo apt update && sudo apt upgrade -y
```

```bash
sudo apt autoremove && sudo apt autoclean
```

* Fedora

```bash
sudo dnf upgrade
```

## Install Packages

* Ubuntu

```bash
sudo apt install zsh ranger git wget
```

* Fedora

```bash
sudo dnf install zsh ranger git wget util-linux-user
```

## Create the Folders

```bash
mkdir -p ~/.config/zsh/plugins && mkdir -p ~/.cache/zsh
```

## Move '.zshrc' file to '$HOME/.config/zsh'

* Ubuntu

```bash
echo -e '\n\n# zsh\nexport ZDOTDIR="$HOME/.config/zsh"' >> ~/.profile
```

* Fedora

DO NOTHING

## Download Fonts

```bash
wget https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Regular.ttf https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Bold.ttf https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Italic.ttf https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Bold%20Italic.ttf -P ./MesloLGS-NF
```

## Install Fonts

```bash
sudo mv MesloLGS-NF /usr/share/fonts
```

## Set the shell font

* Ubuntu

Set the shell font to MesloLGS NF Regular

* Fedora

> Close and reopen the terminal

Set the shell font to MesloLGS NF Regular

## LOGOUT and LOGIN

## Run 	`zsh` and configure it

```bash
zsh
```

## Download Plugins

```bash
git clone https://github.com/zsh-users/zsh-autosuggestions.git ~/.config/zsh/plugins/autosuggestions && git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ~/.config/zsh/plugins/syntax-highlighting && git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ~/.config/zsh/plugins/powerlevel10k
```

## Install Plugins

* Ubuntu

```bash
echo -e '\n\n# Plugins\nsource ~/.config/zsh/plugins/powerlevel10k/powerlevel10k.zsh-theme\nsource ~/.config/zsh/plugins/autosuggestions/zsh-autosuggestions.zsh\nsource ~/.config/zsh/plugins/syntax-highlighting/zsh-syntax-highlighting.zsh' >> ~/.config/zsh/.zshrc
```

* Fedora

```bash
echo -e '\n\n# Plugins\nsource ~/.config/zsh/plugins/powerlevel10k/powerlevel10k.zsh-theme\nsource ~/.config/zsh/plugins/autosuggestions/zsh-autosuggestions.zsh\nsource ~/.config/zsh/plugins/syntax-highlighting/zsh-syntax-highlighting.zsh' >> ~/.zshrc
```

## add ranger to zsh

* Ubuntu

```bash
echo -e '\n\n# ranger-cd\nfunction ranger-cd {\n    tempfile="$(mktemp -t tmp.XXXXXX)"\n    /usr/bin/ranger --choosedir="$tempfile" "${@:-$(pwd)}"\n    test -f "$tempfile" &&\n    if [ "$(cat -- "$tempfile")" != "$(echo -n `pwd`)" ]; then\n        cd -- "$(cat "$tempfile")"\n    fi  \n    rm -f -- "$tempfile"\n}\n\n#ranger-cd will run by alt+r\nbindkey -s "^\\er" "ranger-cd\\n"' >> ~/.config/zsh/.zshrc
```

* Fedora

```bash
echo -e '\n\n# ranger-cd\nfunction ranger-cd {\n    tempfile="$(mktemp -t tmp.XXXXXX)"\n    /usr/bin/ranger --choosedir="$tempfile" "${@:-$(pwd)}"\n    test -f "$tempfile" &&\n    if [ "$(cat -- "$tempfile")" != "$(echo -n `pwd`)" ]; then\n        cd -- "$(cat "$tempfile")"\n    fi  \n    rm -f -- "$tempfile"\n}\n\n#ranger-cd will run by alt+r\nbindkey -s "^\\er" "ranger-cd\\n"' >> ~/.zshrc
```

## ZSH as the default shell 

* for `root`

```bash
sudo -s
```

```bash
chsh -s /bin/zsh root
```

```bash
exit
```

* for `user`

```bash
chsh -s /bin/zsh $USER
```

## LOGOUT and LOGIN

## Open the Terminal and Configure it
