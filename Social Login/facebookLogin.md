### Socialite Login in laravel ###

```composer 
composer require laravel/socialite
```

These credentials should be placed in your application's ``config/services.php``
```php 
    'facebook' => [
        'client_id' => env('FACEBOOK_CLIENT_ID'),
        'client_secret' => env('FACEBOOK_CLIENT_SECRET'),
        'redirect' => env('FACEBOOK_REDIRECT_URI')
    ],
```