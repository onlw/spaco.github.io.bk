---
title: composer-package-build
permalink: composer-package-build
categories:
- program
tags: 
- composer
comments: true
date: 2019-02-14
updated: 2019-02-14
---

## Build composer package

#### composer


#### build your composer package

1. create package floder

   ```bash
   mkdir package-name
   cd package-name
   ```

2. init

   ```bash
    ashe@ashedeMacBook-Pro  ~/code/composer-package/express   master  composer init
   
   
     Welcome to the Composer config generator
   
   
   
   This command will guide you through creating your composer.json config.
   
   Package name (<vendor>/<name>) [ashe/express]: spaco/express
   Description []: get express php
   Author [spaco <she.ct@outlook.com>, n to skip]:
   Minimum Stability []: dev
   Package Type (e.g. library, project, metapackage, composer-plugin) []: library
   License []: MIT
   
   Define your dependencies.
   
   Would you like to define your dependencies (require) interactively [yes]?
   Search for a package:
   Would you like to define your dev dependencies (require-dev) interactively [yes]?
   Search for a package:
   
   {
       "name": "spaco/express",
       "description": "get express php",
       "type": "library",
       "license": "MIT",
       "authors": [
           {
               "name": "spaco",
               "email": "she.ct@outlook.com"
           }
       ],
       "minimum-stability": "dev",
       "require": {}
   }
   
   Do you confirm generation [yes]?
   ```

   `此时composer.json生成完毕`
