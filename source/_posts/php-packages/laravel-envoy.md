---
title: laravel-envoy
permalink: laravel-envoy
categories:
- program
tags: 
- laravel-envoy
comments: true
date: 2019-07-02
updated: 2019-07-02
---

## Introduction

### What is laravel-envoy

Suppose you hava a samll project of your own,put it on a remote server. Every time you develop a function for this 
small project, you have to go online. The general operation will be the following.

1. connect to server ` ssh username@ip`
2. enter the project  `cd workspace/my project`
3. update local code to the latest  `git pull`

Doing this thing manually for too long,It feels bad ～

[Laravel Envoy](https://github.com/laravel/envoy) provides a clean, minimal syntax for defining common tasks you run on
 your remote servers. Using Blade style syntax, you can easily setup tasks for deployment, Artisan commands, and more. 
 Currently, Envoy only supports the Mac and Linux operating systems.

Allows you to do all of the above with a minimum of configuration,just by executing the following line of commands from the local command line.

```bash
envoy run deploy
```



### Install

First, install Envoy using the Composer `global require` command:

```bash
composer global require laravel/envoy
```

Since global Composer libraries can sometimes cause package version conflicts, you may wish to consider using `cgr`, which is a drop-in replacement for the `composer global require`command. The `cgr` library's installation instructions can be [found on GitHub](https://github.com/consolidation-org/cgr).

After installed,you can see it in the `~/.composer/vendor/bin` directory

`Make sure to place the `~/.composer/vendor/bin` directory in your PATH so the `envoy` executable is found when running the `envoy` command in your terminal.`

executing the following line of commands from the local command line.

 ```bash
export PATH="$PATH:$HOME/.composer/vendor/bin"
 ```

or 

```bash
# i am using zsh, update .bash_profile is also useful

vim ~/.bash_profile
# add this line
export PATH="$PATH:$HOME/.composer/vendor/bin"
# refresh .bash_profile
source ~/.bash_profile
```

In order to test whether the installation was successful

```bash
envoy --version

# show like this
Laravel Envoy 1.5.0
```

#### Updating Envoy

```bash
composer global update
```

To view a list of all available Envoy commands, you may use the `list` command:

```bash
envoy list
```



## Writing Tasks

First, in your project root directory , execute the following command to initialize

```bash
envoy init root@127.0.0.1

# show like this
Envoy file created!
```

`Envoy.blade.php` is created

```bash
@servers(['web' => 'root@127.0.0.1'])

@task('deploy')
    cd /path/to/site
    git pull origin master
@endtask
```



### Variables

If needed, you may pass option values into Envoy tasks using the command line:

```bash
envoy run deploy --branch=master
```



```php
@servers(['web' => '192.168.1.1'])

@task('deploy', ['on' => 'web'])
    cd site

    @if ($branch)
        git pull origin {{ $branch }}
    @endif

    php artisan migrate
@endtask
```



### Stories

Stories group a set of tasks under a single, convenient name, allowing you to group small, focused tasks into large tasks. For instance, a `deploy` story may run the `git` and `composer`tasks by listing the task names within its definition:

```php
@servers(['web' => '192.168.1.1'])

@story('deploy')
    git
    composer
@endstory

@task('git')
    git pull origin master
@endtask

@task('composer')
    composer install
@endtask
```

Once the story has been written, you may run it just like a typical task:

```bash
envoy run deploy
```

Attention: 

```php
@servers(['web' => '192.168.1.1'])

@story('deploy')
    task1
    task2
@endstory

@task('task1')
  	cd ~/code
    pwd
@endtask

@task('task2')
    composer install
@endtask
```

在story里，只能使用tasks, 并且第一个 task `cd` 进入 code 目录，不代表第二个 task 在 code 目录中 

```php
@story('deploy')
  	cd /workspace/project   // error!
    task1
    task2
@endstory
```

```bash
# about pwd

# task1
/root/code

# task2
/root
```



### Multiple Servers

Envoy allows you to easily run a task across multiple servers. First, add additional servers to your `@servers` declaration. Each server should be assigned a unique name. Once you have defined your additional servers, list each of the servers in the task's `on` array:

```php
@servers(['web-1' => '192.168.1.1', 'web-2' => '192.168.1.2'])

@task('deploy', ['on' => ['web-1', 'web-2']])
    cd site
    git pull origin {{ $branch }}
    php artisan migrate
@endtask
```

### Parallel Execution

默认情况下，将在每个服务器上串行执行任务。换句话说，任务将在继续在第二台服务器上执行之前在第一台服务器上完成运行。如果要并行运行多个服务器上的任务，请将该`parallel`选项添加到任务声明中：

```php
@servers(['web-1' => '192.168.1.1', 'web-2' => '192.168.1.2'])

@task('deploy', ['on' => ['web-1', 'web-2'], 'parallel' => true])
    cd site
    git pull origin {{ $branch }}
    php artisan migrate
@endtask
```



## Running Tasks

```bash
envoy run deploy
```



### Confirming Task Execution

如果要在服务器上运行给定任务之前提示您进行确认，则应将该`confirm`指令添加到任务声明中。此选项对于破坏性操作特别有用：

```php
@task('deploy', ['on' => 'web', 'confirm' => true])
    cd site
    git pull origin {{ $branch }}
    php artisan migrate
@endtask
```



## Notifications

to be continued