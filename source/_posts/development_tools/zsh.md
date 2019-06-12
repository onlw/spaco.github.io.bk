---
title: zsh
permalink: zsh
categories: 
- notes
tags: 
- software
comments: true
date: 2018-12-05
updated: 2018-12-05
---

##Install zsh(oh-my-zsh) && plugins && themes

### zsh (oh-my-zsh)

#### Why zsh

样式不错 插件多 

#### Install

[zsh-github](https://github.com/robbyrussell/oh-my-zsh)

```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

### Plugins

#### Enabling Plugins

Once you spot a plugin (or several) that you'd like to use with Oh My Zsh, you'll need to enable them in the .zshrc file. You'll find the zshrc file in your $HOME directory. Open it with your favorite text editor and you'll see a spot to list all the plugins you want to load.

```bash
vi ~/.zshrc
# For example, this might begin to look like this:
plugins=(git autojump zsh-autosuggestions zsh-syntax-highlighting)
```



###Plugin recommendation

#### git

Bring your own

####zsh-syntax-highlighting

> [Github](https://github.com/zsh-users/zsh-syntax-highlighting)

```bash
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting

# update ~/.zshrc plugins
plugins=( [plugins...] zsh-syntax-highlighting) 

# refresh ~/.zshrc 
source ~/.zshrc
```

#### zsh-autosuggestion

> [Github](https://github.com/zsh-users/zsh-autosuggestions)

```bash
git clone git://github.com/zsh-users/zsh-autosuggestions $ZSH_CUSTOM/plugins/zsh-autosuggestions

# update ~/.zshrc plugins
plugins=( [plugins...] zsh-autosuggestions) 

# refresh ~/.zshrc 
source ~/.zshrc
```

#### Git-open

```bash
git clone https://github.com/paulirish/git-open.git $ZSH_CUSTOM/plugins/git-open

# update ~/.zshrc plugins
plugins=( [plugins...] git-open) 

# refresh ~/.zshrc 
source ~/.zshrc
```



### Themes

#### ys



pyenv

brew

 npm cnpm



https://github.com/pyenv/pyenv#basic-github-checkout