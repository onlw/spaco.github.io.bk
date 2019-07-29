---
title: laravel Facades
permalink: laravel-facades
categories:
- program
tags: 
- laravel
comments: true
date: 2019-07-26
updated: 2019-07-26
---
Laravel Facade 执行流程分析

## Introduction

Facades 为应用程序提供了一系列的静态方法，提供简洁，富有表现力的语法，同时保持比传统静态方法更多的可测试性和灵活性。

###  Demo Used in code

```php
// cache
\Illuminate\Support\Facades\Cache::get('cacheKey');
// redis
\Illuminate\Support\Facades\Redis::get('redisKey');
// auth
\Illuminate\Support\Facades\Auth::id();
// config
\Illuminate\Support\Facades\Config::get('configKey');
// log
\Illuminate\Support\Facades\Log::info('logKey');
// request
\Illuminate\Support\Facades\Request::get('requestKey');
```



## How Facades Work (以Cache为例)

首先是`Cache Facade`文件

```php
namespace Illuminate\Support\Facades;

/* 
 * @see \Illuminate\Cache\CacheManager
 * @see \Illuminate\Cache\Repository
 */
class Cache extends Facade
{
    /**
     * Get the registered name of the component.
     *
     * @return string
     */
    protected static function getFacadeAccessor()
    {
        return 'cache';
    }
}
```

代码量很少，只有一个`getFacadeAccessor`方法，那么它是如何可以调用其他方法的呢，主要是`Cache Facade` 继承的`Facade`类，`Facade`主要代码如下所示:

```php
namespace Illuminate\Support\Facades;

abstract class Facade
{
  
      /**
     * The application instance being facaded.
     *
     * @var \Illuminate\Contracts\Foundation\Application
     */
    protected static $app;

    /**
     * The resolved object instances.
     *
     * @var array
     */
    protected static $resolvedInstance;
  
      /**
     * Handle dynamic, static calls to the object.
     *
     * @param  string  $method
     * @param  array   $args
     * @return mixed
     *
     * @throws \RuntimeException
     */
    public static function __callStatic($method, $args)
    {
        $instance = static::getFacadeRoot();

        if (! $instance) {
            throw new RuntimeException('A facade root has not been set.');
        }

        return $instance->$method(...$args);
    }
  
      /**
     * Get the root object behind the facade.
     *
     * @return mixed
     */
    public static function getFacadeRoot()
    {
        return static::resolveFacadeInstance(static::getFacadeAccessor());
    }
  
      /**
     * Resolve the facade root instance from the container.
     *
     * @param  object|string  $name
     * @return mixed
     */
    protected static function resolveFacadeInstance($name)
    {
        if (is_object($name)) {
            return $name;
        }

        if (isset(static::$resolvedInstance[$name])) {
            return static::$resolvedInstance[$name];
        }

        return static::$resolvedInstance[$name] = static::$app[$name];
    }
  
      /**
     * Get the registered name of the component.
     *
     * @return string
     *
     * @throws \RuntimeException
     */
    protected static function getFacadeAccessor()
    {
        throw new RuntimeException('Facade does not implement getFacadeAccessor method.');
    }
  
}
```



那么   `cache` 在何处定义的呢？是由框架内部的`CacheServiceProvider`注册，若想更深一步了解`Service Providers`可以参考[Service Providers](https://laravel.com/docs/master/providers)

```php
namespace Illuminate\Cache;

class CacheServiceProvider extends ServiceProvider implements DeferrableProvider
{
      /**
     * Register the service provider.
     *
     * @return void
     */
    public function register()
    {
        $this->app->singleton('cache', function ($app) {
            return new CacheManager($app);
        });
    }

    /**
     * Get the services provided by the provider.
     *
     * @return array
     */
    public function provides()
    {
        return [
            'cache'
        ];
    }
}
```

那么就很明显了，`Cache::get()` 实际上是调用`CacheManager::get()`，主要是代码是在`CacheManager`中，`Cache Facade`提供了一种便捷使用方法。



##Find Laravel Predefined Facades

`search`

```php
$this->app->singleton('cache'
$this->app->singleton('redis'
$this->app->singleton('log'                      
```



