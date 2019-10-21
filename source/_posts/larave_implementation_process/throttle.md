---
title: laravel-throttle
permalink: laravel-throttle
categories:
- program
tags: 
- laravel
comments: true
date: 2019-10-10
updated: 2019-10-10
thumbnail: 'banners/ZhUwdBwIDiIefFYJ.jpg'
---

## Throttle 

Throttle : 风门 喉咙 喉

结论：以每分钟请求数为单位，但是间隔可以为小数
60,1 一分钟请求60次
6,0.1  6秒请求6次

### Kernel.php

```php
# App\Http\Kernel.php

protected $routeMiddleware = [
  'auth' => \App\Http\Middleware\Authenticate::class,
  'auth.basic' => \Illuminate\Auth\Middleware\AuthenticateWithBasicAuth::class,
  'bindings' => \Illuminate\Routing\Middleware\SubstituteBindings::class,
  'cache.headers' => \Illuminate\Http\Middleware\SetCacheHeaders::class,
  'can' => \Illuminate\Auth\Middleware\Authorize::class,
  'guest' => \App\Http\Middleware\RedirectIfAuthenticated::class,
  'signed' => \Illuminate\Routing\Middleware\ValidateSignature::class,
  'throttle' => \Illuminate\Routing\Middleware\ThrottleRequests::class,
  'verified' => \Illuminate\Auth\Middleware\EnsureEmailIsVerified::class,
];
```

可以看到 `throttle`  对应 `\Illuminate\Routing\Middleware\ThrottleRequests::class`

### ThrottleRequests.php

```php
# Illuminate\Routing\Middleware\ThrottleRequests

public function handle($request, Closure $next, $maxAttempts = 60, $decayMinutes = 1)
{
  # 根据当前 默认guard登陆用户 或 当前domain+ip 生成签名
  $key = $this->resolveRequestSignature($request);
  
  # 根据 中间件的参数获取最大接口请求数量
  # "60,1" => 1分钟内最多请求60次
  # "30|60,1" => 1分钟内，已登录用户请求上线60次，未登录用户请求上线30次
  # "guard user property|1" => 若设置的是已登录用户的可访问属性，则返回对应属性guard user property的值
  $maxAttempts = $this->resolveMaxAttempts($request, $maxAttempts);

  # 从cache中获取签名对应的请求数，若大于最大请求数，抛出异常 & 返回响应Header数据
  if ($this->limiter->tooManyAttempts($key, $maxAttempts)) {
    throw $this->buildException($key, $maxAttempts);
  }

  # 根据签名，设置具体请求缓存，缓存时间为 请求时间段 decayMinutes * 60，1分钟请求限制，缓存时间为60秒
  # 设置 签名+'：timer'缓存，值为当前时间戳+60秒
	# 设置请求缓存数+1
  $this->limiter->hit($key, $decayMinutes * 60);

  $response = $next($request);
 
  # 返回响应Header && 接口响应用户
  # Header内容：['X-RateLimit-Limit','X-RateLimit-Remaining']
  # 依次为 最大请求数，剩余可请求数
  return $this->addHeaders(
    $response, $maxAttempts,
    $this->calculateRemainingAttempts($key, $maxAttempts)
  );
  
}
```

