---
title: hexo-on-github-pages
permalink: hexo-on-github-pages
categories:
- notes
tags: 
- hexo
comments: true
date: 2018-09-01
updated: 2018-09-01
---

Welcome to [Hexo](https://hexo.io/)! This is your very first post. Check [documentation](https://hexo.io/docs/) for more info. If you get any problems when using Hexo, you can find the answer in [troubleshooting](https://hexo.io/docs/troubleshooting.html) or you can ask me on [GitHub](https://github.com/hexojs/hexo/issues).

## Quick Start

### Precondition

- git : deploy to github
- node.js : Hexo 基于 node.js开发的

### Environment

- git 

  ```bash
  # version
  git --version
  git version 2.17.2 (Apple Git-113)
  ```

- node.js

  ```bash
  # version
  node -v
  v7.2.1
  ```

### Install Hexo

```bash
npm install -g hexo-cli
```

### Init Hexo Folder

```bash
mkdir spaco
cd spaco
hexo init
npm install
```

### Run server

```bash
hexo server
Hexo is running at http://localhost:4000 . Press Ctrl+C to stop.
```

### Create a new post

``` bash
hexo new "My New Post"
```

More info: [Writing](https://hexo.io/docs/writing.html)

### Run server

``` bash
hexo server
Hexo is running at http://localhost:4000 . Press Ctrl+C to stop.
```

More info: [Server](https://hexo.io/docs/server.html)

### Replace theme

```bash
cd spaco/themes
git clone https://github.com/elmorec/hexo-theme-inside.git inside

cd ../
vim _config.yml
# replace default theme to inside
theme: landscape => theme: inside

hexo server
# theme has changed
```

### Deploy to GitHub

`need ssh-key already registered in GitHub`

1. Create repostory

   your_github_name.github.io

2. Deployment config

   ```bash
   vim _config.yml
   # like this
   
   # Deployment
   ## Docs: https://hexo.io/docs/deployment.html
   deploy:
     type: git
     repo: git@github.com:spaco/spaco.github.io.git
     branch: master
   ```

### Clean static files

```bash
hexo clean
hexo c
```

### Generate static files

``` bash
hexo generate 
# hexo g
```

More info: [Generating](https://hexo.io/docs/generating.html)

### Deploy to remote sites

``` bash
hexo deploy
# hexo d
```

More info: [Deployment](https://hexo.io/docs/deployment.html)

### Effective

open `your-page.github.io` in the browser

### Replace domain name



### References

[github+Hexo搭建blog](https://www.jianshu.com/p/863f3f2d1733)

[浅析 Hexo 搭建博客的原理](https://juejin.im/post/598eeaff5188257d592e55bb)