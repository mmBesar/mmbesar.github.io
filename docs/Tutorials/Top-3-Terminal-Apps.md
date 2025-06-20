---
template: blog_post.html
title: Top 3 Terminal Tools
description: اكتشف أفضل 3 تطبيقات في طرفية لينكس لزيادة إنتاجيتك اليومية
date: 2025-06-20
---

# <div dir="rtl">اكتشف أفضل 3 تطبيقات في طرفية لينكس لزيادة إنتاجيتك اليومية</div>

![type:video](https://www.youtube.com/embed/eVfYJ1AspMg)

<div dir="rtl">
أشارك معكم 3 أدوات وتطبيقات خفيفة ومهمة لأفضل تجربة استخدام على الطرفية في لينكس، سواء كنت تستخدم Bash أو Zsh، الأدوات مهمة وفعالة.
</div>

<p hidden>#more</p>

## Tools

```sh
sudo apt update && sudo apt install gpg git curl
```

## NerdFont

```sh
mkdir -p ~/.local/share/fonts \
&& cd ~/.local/share/fonts \
&& curl -OL https://github.com/ryanoasis/nerd-fonts/releases/latest/download/JetBrainsMono.tar.xz \
&& bash -c 'mkdir "${1%.tar.xz}" && tar -xf "$1" -C "${1%.tar.xz}"' _ JetBrainsMono.tar.xz \
&& rm JetBrainsMono.tar.xz \
&& fc-cache -fv
```

## Starship

1. Install
```sh
curl -sS https://starship.rs/install.sh | sh
```

2. Add it to `bashrc`
```sh
echo 'eval "$(starship init bash)"' >> ~/.bashrc
```

3. Add it to `zshrc`
```sh
echo 'eval "$(starship init zsh)"' >> ~/.zshrc
```

## fzf

1. Use the official script
```sh
git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf
~/.fzf/install
```

2. And add the following line to your shell configuration file

=== "bash"

    ``` sh
    # Set up fzf key bindings and fuzzy completion
    eval "$(fzf --bash)"
    ```

=== "zsh"

    ``` sh
    # Set up fzf key bindings and fuzzy completion
    source <(fzf --zsh)
    ```

## eza

1. Install eza via:
```sh
sudo mkdir -p /etc/apt/keyrings \
&& wget -qO- https://raw.githubusercontent.com/eza-community/eza/main/deb.asc | sudo gpg --dearmor -o /etc/apt/keyrings/gierens.gpg \
&& echo "deb [arch=amd64 signed-by=/etc/apt/keyrings/gierens.gpg] http://deb.gierens.de stable main" | sudo tee /etc/apt/sources.list.d/gierens.list \
&& sudo chmod 644 /etc/apt/keyrings/gierens.gpg /etc/apt/sources.list.d/gierens.list \
&& sudo apt update \
&& sudo apt install -y eza
```

2. Add alias to `.bashrc`

```sh
nano ~/.bashrc
```

``` ini title="~/.bashrc"
alias ls='eza --color=auto --icons=auto'
alias eza='eza --color=auto --icons=auto'
alias ll='eza -l'
alias la='eza -lah'
alias lt='eza -T'
```

## <div dir="rtl">مراجع</div>

- [NerdFont](https://github.com/ryanoasis/nerd-fonts)
- [Starship](https://github.com/starship/starship)
- [fzf](https://github.com/junegunn/fzf)
- [eza](https://github.com/eza-community/eza)