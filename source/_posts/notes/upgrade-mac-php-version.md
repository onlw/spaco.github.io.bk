---
title: upgrade-mac-php-version.md
permalink: upgrade-mac-php-version.md
categories:
- program
tags: 
- php-version
comments: true
date: 2019-07-04
updated: 2019-07-04
---

mac 本地 php 版本较低，升级至 7.2

## Upgrade



### update brew

如未安装 `brew`，参考 https://brew.sh/ 安装

```bash
brew update

brew search php
```

```bash
$ brew search php
==> Formulae
brew-php-switcher         php-cs-fixer              phplint                   phpstan
php                       php@7.1                   phpmd                     phpunit
php-code-sniffer          php@7.2                   phpmyadmin
```



### Install

```bash
brew install php@7.2
```

安装完成之后，执行`php -v` 查看PHP版本，显示仍然是老版本



### 修改本地PHP版本

```bash
brew link php@7.2
```

出现以下提示

`因为mac用的是zsh，所以会将PATH 写入 ~/.zshrc,个人根据提示执行命令即可`

```bash
$ brew link php@7.2
Warning: php@7.2 is keg-only and must be linked with --force

If you need to have this software first in your PATH instead consider running:
  echo 'export PATH="/usr/local/opt/php@7.2/bin:$PATH"' >> ~/.zshrc
  echo 'export PATH="/usr/local/opt/php@7.2/sbin:$PATH"' >> ~/.zshrc
```

执行完之后，刷新配置

```bash
source ~/.zshrc
```

执行`php -v` 查看PHP版本

```bash
php -v

PHP 7.2.19 (cli) (built: Jun 17 2019 09:03:55) ( NTS )
```

Success