---
title: laravel-login
permalink: laravel-login
categories:
- program
tags: 
- laravel
comments: true
date: 2019-06-14
updated: 2019-06-14
---


## Quick Start

```php
composer create-project --prefer-dist laravel/laravel principle && cd principle
php artisan make:auth
# database configuration
vim .env
php artisan db:seed --class=UsersTableSeeder
php artisan serve
```



## Login Route

`Auth::routes();`

```php
# config/app.php
'Auth' => Illuminate\Support\Facades\Auth::class
```

```php
		# Illuminate\Support\Facades\Auth
    public static function routes(array $options = [])
    {
        static::$app->make('router')->auth($options);
    }
```

```php
    # Illuminate\Routing\RoutingServiceProvider
    protected function registerRouter()
    {
        $this->app->singleton('router', function ($app) {
            return new Router($app['events'], $app);
        });
    }
```

```php
    # Illuminate\Routing\Router
    public function auth(array $options = [])
    {
        // Authentication Routes...
        $this->get('login', 'Auth\LoginController@showLoginForm')->name('login');
        $this->post('login', 'Auth\LoginController@login');
        $this->post('logout', 'Auth\LoginController@logout')->name('logout');

        // Registration Routes...
        if ($options['register'] ?? true) {
            $this->get('register', 'Auth\RegisterController@showRegistrationForm')->name('register');
            $this->post('register', 'Auth\RegisterController@register');
        }

        // Password Reset Routes...
        if ($options['reset'] ?? true) {
            $this->resetPassword();
        }

        // Email Verification Routes...
        if ($options['verify'] ?? false) {
            $this->emailVerification();
        }
    }
```



## Login Action Explain

### resource code

according to `Auth::routes()`, source code => `Auth\LoginController@login`

```php
# App\Http\Controllers\Auth\LoginController

namespace App\Http\Controllers\Auth;

use App\Http\Controllers\Controller;
use Illuminate\Foundation\Auth\AuthenticatesUsers;

class LoginController extends Controller
{
    use AuthenticatesUsers;
}
```

In fact, the main code implementation is in `AuthenticatesUsers` Trait

```php
# Illuminate\Foundation\Auth\
    public function login(Request $request)
    {
        $this->validateLogin($request);

        // If the class is using the ThrottlesLogins trait, we can automatically throttle
        // the login attempts for this application. We'll key this by the username and
        // the IP address of the client making these requests into this application.
        if (method_exists($this, 'hasTooManyLoginAttempts') &&
            $this->hasTooManyLoginAttempts($request)) {
            $this->fireLockoutEvent($request);

            return $this->sendLockoutResponse($request);
        }

        if ($this->attemptLogin($request)) {
            return $this->sendLoginResponse($request);
        }

        // If the login attempt was unsuccessful we will increment the number of attempts
        // to login and redirect the user back to the login form. Of course, when this
        // user surpasses their maximum number of attempts they will get locked out.
        $this->incrementLoginAttempts($request);

        return $this->sendFailedLoginResponse($request);
    }
```

### explain

1. validate login form
2. rate limiting
3. attempt login && return success response
4. increment login attempts count for rate limiting
5. return fail response 

> More important is attempt login

### attempt login

```php
    if ($this->attemptLogin($request)) {
        return $this->sendLoginResponse($request);
    }

    protected function attemptLogin(Request $request)
    {
        return $this->guard()->attempt(
            $this->credentials($request), $request->filled('remember')
        );
    }
```

In fact, the main code implementation is in `Illuminate\Auth\SessionGuard`  Class

> `SessionGuard`  `attempt` method

```php 
    public function attempt(array $credentials = [], $remember = false)
    {
        $this->fireAttemptEvent($credentials, $remember);

        $this->lastAttempted = $user = $this->provider->retrieveByCredentials($credentials);

        // If an implementation of UserInterface was returned, we'll ask the provider
        // to validate the user against the given credentials, and if they are in
        // fact valid we'll log the users into the application and return true.
        if ($this->hasValidCredentials($user, $credentials)) {
            $this->login($user, $remember);

            return true;
        }

        // If the authentication attempt fails we will fire an event so that the user
        // may be notified of any suspicious attempts to access their account from
        // an unrecognized user. A developer may listen to this event as needed.
        $this->fireFailedEvent($user, $credentials);

        return false;
    }
```

> The essence is to save a session

```php
    # Illuminate\Auth\SessionGuard
		protected function updateSession($id)
    {
        $this->session->put($this->getName(), $id);

        $this->session->migrate(true);
    }
```

```php
    # Illuminate\Auth\SessionGuard
 		public function getName()
    {
        return 'login_'.$this->name.'_'.sha1(static::class);
    }
```

 