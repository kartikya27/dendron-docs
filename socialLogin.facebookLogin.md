---
id: sw0708q1bbv2m9lgtsoeo0u
title: Facebook Login
desc: ""
updated: 1675504680938
created: 1675491348383
---

### Socialite Login in laravel

```composer
composer require laravel/socialite
```

---

**Testing Credintial**

```.env
FACEBOOK_CLIENT_ID="500362448836300"
FACEBOOK_CLIENT_SECRET="fff835a12545757a21d039e99c0fe2ce"
FACEBOOK_REDIRECT_URI="https://127.0.0.1:8000/auth/facebook/login/callback"
```

These credentials should be placed in your application's `config/services.php`

```php
    'facebook' => [
        'client_id' => env('FACEBOOK_CLIENT_ID'),
        'client_secret' => env('FACEBOOK_CLIENT_SECRET'),
        'redirect' => env('FACEBOOK_REDIRECT_URI')
    ],
```

In `routes/web.php`

```php
Route::prefix('auth')->group(function () {
    ## // * For Facebook Login API
    Route::prefix('facebook')->name('facebook.')->group(function () {
        Route::get('login', [FacebookController::class, 'loginUsingFacebook']);
        Route::get('login/callback', [FacebookController::class, 'callbackFromFacebook']);
    });
});
```

In Facebook Controller

```php
namespace App\Http\Controllers;

use App\Exceptions\ErrorException;
use App\Models\User;
use Illuminate\Support\Facades\Auth;
use Illuminate\Support\Facades\Hash;
use Laravel\Socialite\Facades\Socialite;

class FacebookController extends Controller
{
    public function loginUsingFacebook()
    {
        return Socialite::driver('facebook')->redirect();
    }

    public function callbackFromFacebook()
    {
        try {

            $user = Socialite::driver('facebook')->user();

            $saveUser = User::updateOrCreate([
                'email' => $user->getEmail(),
            ], [
                'name' => $user->getName(),
                'type' => "facebook",
                'role' => "user",
                'email' => $user->getEmail(),
                'password' => Hash::make($user->getName() . '@' . $user->getId())
            ]);

            Auth::loginUsingId($saveUser->id);
            return success("User Logged In")->response();
        } catch (ErrorException $e) {
            dd($e->getMessage());
        }
    }
}

```

**_Note:-_** For Google Login Change `google` driver name insted of `facebook`

```php
 $user = Socialite::driver('google')->user();
```

### Login Buttons

**_`Facebook`_**

```html
<fb:login-button config_id="{config_id}" onlogin="checkLoginState();">
</fb:login-button>
```

**_`Google`_**

```html
<div class="g-signin2" data-onsuccess="onSignIn"></div>
```

**Include in html header file**

```html
<script src="https://apis.google.com/js/platform.js" async defer></script>
```
