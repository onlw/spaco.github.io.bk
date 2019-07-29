---
title: laravel-reset-password
permalink: laravel-reset-password
categories:
- program
tags: 
- laravel
comments: true
date: 2019-05-20
updated: 2019-05-20
thumbnail: 'banners/ZhUwdBwIDiIefFYJ.jpg'
---

# Reset Password

## Environment

[TOC]

Larval5.8

> 首先是 ResetPasswordController 的 reset 方法，其实是由引入的 trait ResetsPasswords 内部实现的

```php
use Illuminate\Foundation\Auth\ResetsPasswords;

use ResetsPasswords;
```

