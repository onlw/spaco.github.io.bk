---
title: PHP Garbage Collection
categories: program
tags: php
comments: true
date: 2018-12-27
---
Explains Garbage Collection (also known as GC) of PHP

### Related concepts

- Garbage Collection

- zval

- refcount

- is_ref

- arithmetic

  PHP5.2 : [Reference Counting](http://php.net/manual/en/features.gc.refcounting-basics.php)

  PHP5.3 : [Concurrent Cycle Collection in Reference Counted Systems](http://researcher.watson.ibm.com/researcher/files/us-bacon/Bacon01Concurrent.pdf)

- function : [memory_get_usage](http://php.net/manual/en/function.memory-get-usage.php)

- function : xdebug_debug_zval() : need xdebug extension

### Reference Counting

Important : [Reference Counting Basics](http://php.net/manual/en/features.gc.refcounting-basics.php)

被舍弃的原因 ： 引用的值为变量自身，内存泄漏

### Concurrent Cycle Collection in Reference Counted Systems



