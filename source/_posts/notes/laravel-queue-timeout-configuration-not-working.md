---
title: laravel-queue-timeout-configuration-not-working
permalink: laravel-queue-timeout-configuration-not-working
categories:
- program
tags: 
- laravel-queue
comments: true
date: 2019-07-04
updated: 2019-07-04
---

laravel 设置队列超时时间，但是并没有生效

## Development environments 

laravel : v5.5   

```bash
# https://laravel.com/docs/5.5/installation
laravel5.5 server requirements : PHP >= 7.0.0
```

php : v7.1.30



## code

### Controller

```php
<?php

namespace App\Http\Controllers;

use App\Jobs\Test;

class QueueController extends Controller
{
    public function index()
    {
        echo 'queue start';
        Test::dispatch()->onQueue('testQueue');
        echo 'queue end';
    }
}
```

### Job

```php
<?php

namespace App\Jobs;

use App\User;
use Illuminate\Bus\Queueable;
use Illuminate\Queue\SerializesModels;
use Illuminate\Queue\InteractsWithQueue;
use Illuminate\Contracts\Queue\ShouldQueue;
use Illuminate\Foundation\Bus\Dispatchable;
use Illuminate\Support\Facades\Log;

class Test implements ShouldQueue
{
    use Dispatchable, InteractsWithQueue, Queueable, SerializesModels;

    public $tries = 5;

    public $timeout = 5;

    public function __construct()
    {
    }
    public function handle()
    {
        Log::error('queue start');
        sleep(10);
        Log::error('queue end');
    }
}
```

调用控制器，会发现，队列设置的超时时间无效

```bash
[2019-07-04 03:16:38][42] Processing: App\Jobs\Test
[2019-07-04 03:16:48][42] Processed:  App\Jobs\Test
```

日志同样会被打印

```bash
local.ERROR: queue start 
local.ERROR: queue end
```

这是为什么呢

 [Timeout settings not kill job in queue Laravel 5.4](https://github.com/laravel/framework/issues/19584)

```bash
Timeouts also require PHP 7.1 to work.

Ref: Worker::registerTimeoutHandler
Ref: Worker::supportsAsyncSignals
```

Laravel Queue Worker

```php
# Illuminate\Queue\Worker

    /**
     * Register the worker timeout handler (PHP 7.1+).
     *
     * @param  \Illuminate\Contracts\Queue\Job|null  $job
     * @param  WorkerOptions  $options
     * @return void
     */
    protected function registerTimeoutHandler($job, WorkerOptions $options)
    {
        if ($this->supportsAsyncSignals()) {
            // We will register a signal handler for the alarm signal so that we can kill this
            // process if it is running too long because it has frozen. This uses the async
            // signals supported in recent versions of PHP to accomplish it conveniently.
            pcntl_signal(SIGALRM, function () {
                $this->kill(1);
            });
            $timeout = $this->timeoutForJob($job, $options);
            pcntl_alarm($timeout > 0 ? $timeout + $options->sleep : 0);
        }
    }

    /**
     * Determine if "async" signals are supported.
     *
     * @return bool
     */
    protected function supportsAsyncSignals()
    {
        return version_compare(PHP_VERSION, '7.1.0') >= 0 &&
               extension_loaded('pcntl');
    }
```

从上述代码可以看出，`registerTimeoutHandler` 超时处理需要两个条件

-  php 版本 >= 7.1
- 已安装 pcntl 扩展

命令行执行以下命令查看 pcntl 扩展情况

```bash
php -m | grep pcntl
```



## Solution

- 安装 pcntl 扩展

- php 版本 >= 7.1

  

## References

- [Timeout settings not kill job in queue Laravel 5.4](https://github.com/laravel/framework/issues/19584)

- [pcntl extension](https://www.php.net/manual/en/intro.pcntl.php)

  





