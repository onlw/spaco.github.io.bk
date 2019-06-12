---
title: hexo-on-travis-CI
permalink: hexo-on-travis-CI
categories:
- notes
tags: 
- hexo
comments: true
date: 2018-09-01
updated: 2018-09-01
---


## Quick Start

### travis-ci 是什么

一个使用yaml格式配置用于持续集成完成自动化测试部署的开源项目
官网：<https://travis-ci.com/>

### 为什么使用 travis-ci 部署 Hexo blog

- 避免换电脑需要安装环境
- 可以随时更改 blog 内容
- 不喜欢每次 hexo g  + hexo d

### 原理

github 设置两个分支，master develop,

travis-ci 检测 develop 的改动，编译之后将需要的数据提交到 master

### 涉及的主要内容

- GitHub-Pages

- Travis-CI 账号

- [github token](https://github.com/settings/tokens)

  coding 同样

### Create GitHub token

1. 进入 [github token](https://github.com/settings/tokens)，点击 Generate new token

2. 完善 Token description ，Select scopes

3. save

   ![image.png](https://upload-images.jianshu.io/upload_images/3625649-e8ace54949737ec2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/640)

### 同步github repository

![image.png](https://upload-images.jianshu.io/upload_images/3625649-27986c1344ef0e31.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/740)

1. 登陆 travis-ci

2. https://travis-ci.com/account/repositories

3. 同步 repository

   点击 `manage repositories on Github`

   ![image.png](https://upload-images.jianshu.io/upload_images/3625649-be37009a1c05a232.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/540)

4. 点击已同步 repository 右边的 setting

5. 配置 Environment Variables ：会在部署脚本中使用到

   ![image.png](https://upload-images.jianshu.io/upload_images/3625649-acfd05be85246188.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/640)

   GH_REF : github.com/spaco/spaco.github.io.git

   GH_TOKEN : 上个步骤获得的 github token

### Github  repository setting

`以github一个空仓库为示范,但是已实现本地 blog 预览`

1. create develop branch
2. set default branch = develop

### Run travis-ci

运行 travis-ci，需要配置 .travis.yml ,默认检测此文件，详情见下方 Extra

将 develop 编译之后的内容 提交到 master，现有如下几种方式

- copy ssh key
- github token : master 永远只有一次 commit log
- github token : master commit log 保存 （推荐）

git push to develop , travis-ci run

![image.png](https://upload-images.jianshu.io/upload_images/3625649-a020d050be405391.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/740)

### QAQ

- Q : 通过git clone theme,但是通过 travis-ci 自动部署的时候，无法提交 theme文件，因为theme所属另外一个 GitHub-repository

  A : 使用 git submodule clone theme 

  ```bash
  [submodule "themes/inside"]
  	path = themes/inside
  	url = https://github.com/spaco/hexo-theme-inside.git
  ```

### Extra

```bash
language: node_js
node_js: stable

# S: Build Lifecycle
install:
- npm install

#before_script:
# - npm install -g gulp

script:
  - hexo g

after_script:
  - git clone https://${GH_REF} .deploy_git  # GH_REF是最下面配置的仓库地址
  - cd .deploy_git
  - git checkout master
  - cd ../
  - mv .deploy_git/.git/ ./public/   # 这一步之前的操作是为了保留master分支的提交记录，不然每次git init的话只有1条commit
  - cd ./public
  - git config user.name "spaco"
  - git config user.email "she.ct@outlook.com"
  - git add .
  - git commit -m "Travis automatically updates the document"
  - git push --force --quiet "https://${GH_TOKEN}@${GH_REF}" master:master
# E: Build LifeCycle

branches:
  only:
    - develop
```



