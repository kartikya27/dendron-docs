### Socialite Login in laravel ###

```composer 
composer require laravel/socialite
```
---

**Testing Credintial**
```.env
FACEBOOK_CLIENT_ID="586547156655534"
FACEBOOK_CLIENT_SECRET="b62ba606af4acea659213a56c59722fa"
FACEBOOK_REDIRECT_URI="https://127.0.0.1:8000/auth/facebook/login/callback"
```

These credentials should be placed in your application's ``config/services.php``
```php 
    'facebook' => [
        'client_id' => env('FACEBOOK_CLIENT_ID'),
        'client_secret' => env('FACEBOOK_CLIENT_SECRET'),
        'redirect' => env('FACEBOOK_REDIRECT_URI')
    ],
```
In ``routes/web.php``
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