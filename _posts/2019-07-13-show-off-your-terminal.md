---
layout: post
title: Show Off Your Terminal
date: 2019-07-13
---

## Introduction
Have you seen some of those beautiful terminal setups in the presentations. Do you want to change your bash to this
![New Bash](/assets/bash/new_bash.png)
from this
![Old Bash](/assets/bash/old_bash.png)

This blog shall cover beautification of bash on Ubuntu 19

## Installation
Step 1: Install Oh-My-ZSH 
``` shell
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

Step 2: Install a Powerline Font
Follow the instructions from here

``` shell
https://powerline.readthedocs.io/en/latest/installation/linux.html#fonts-installation
```
Execute these commands to install the fonts
``` shell
wget https://github.com/powerline/powerline/raw/develop/font/PowerlineSymbols.otf
wget https://github.com/powerline/powerline/raw/develop/font/10-powerline-symbols.conf
mkdir ~/.local/share/fonts/
mv PowerlineSymbols.otf ~/.local/share/fonts/
fc-cache -vf ~/.local/share/fonts/
mkdir -p ~/.config/fontconfig/conf.d/
mv 10-powerline-symbols.conf ~/.config/fontconfig/conf.d/
```

Next, we need to download a bunch of additional fonts

``` shell
> git clone https://github.com/gabrielelana/awesome-terminal-fonts.git
> git clone https://github.com/zsh-users/zsh-autosuggestions.git
> exec zsh
> git clone https://github.com/bhilburn/powerlevel9k.git ~/.oh-my-zsh/custom/themes/powerlevel9k
```

Backup the existing ~/.bashrc and copy the version from here
https://gist.github.com/hariinfo/df486e2f321372646d449fcb875c88b9

1.) Download a Nerd Font from here https://nerdfonts.com/, for my version of bashrc file I am using RobotoMon font from here
https://github.com/ryanoasis/nerd-fonts/releases/download/v2.0.0/RobotoMono.zip

2.) Unzip and copy to ~/.fonts
3) Download to ~/.fonts
https://github.com/spencerhakim/zsh-snarf/blob/master/Knack%20Regular%20Nerd%20Font%20Complete.otf

3.) Run the command fc-cache -fv to manually rebuild the font cache

If everything went as expected, you should be able to see the new terminal display. 

