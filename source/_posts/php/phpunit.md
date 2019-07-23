---
title: phpunit
permalink: phpunit
categories:
- program
tags: 
- phpunit
comments: true
date: 2019-07-04
updated: 2019-07-04
---

[PHPUnit](https://phpunit.de)是一个面向PHP程序员的测试框架，这是一个xUnit的体系结构的单元测试框架


## Install

我们用一个 [PHP Archive (PHAR)](http://php.net/phar) 来包含你需要使用的PHPUnit，可以从这里[下载](http://phar.phpunit.cn/phpunit.phar)它，使其可执行，并把它放到你的 `$PATH` 里, 如:

```bash
wget http://phar.phpunit.cn/phpunit.phar
chmod +x phpunit.phar
sudo mv phpunit.phar /usr/local/bin/phpunit
phpunit --version
```



## Used in phpstorm

Languages & Frameworks  ->  PHP  -> Debug  -> Test Frameworks

Path to phpunit.phar. -> /usr/local/bin/phpunit
