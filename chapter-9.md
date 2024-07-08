## onceUsingId() & loginUsingId()
**onceUsingId()** logs a user into the application once using their ID without starting a persistent session. Upon logout, the user cannot log in again by default.
<br/>
**loginUsingId()** logs a user into the application, starting a persistent session. The user remains logged in and can access the application consistently.


_________________
## logoutOtherDevices()
[Follow this tutorial](https://programmingfields.com/logout-multiple-login-sessions-from-other-browsers-in-laravel-9)

___________
## Middleware

**Main Difference between auth and auth.basic**
```
Route::middleware('auth.basic')->group(function () {
    Route::get('/profile', [ProfileController::class, 'edit'])->name('profile.edit');
}
```
in Case of auth.basic show a **popup** asking for username(Email) and password.
_____________
## Email Verification
```
class User extends Authenticatable implements MustVerifyEmail
{}
```
```
in web.php
Auth::routes(['verify' => true]);
```
__________
**Note : @auth('guard_name') in Blade templates checks if the user is authenticated using a specific guard ('trainees' in this case) instead of the default 'web' guard.**
______________
## Guards
``` in config/auth.php ```
**The driver has just two choices: session or token. However, the provider deals exclusively with users. To expand this, you could create a new migration, perhaps named "admins."**
<br/> <br/>
**By Default auth() deals with guard('web'), if you want to change use**
```
$apiUser = auth()->guard('api')->user();
```
[Video](https://youtu.be/thMtoYW2oJg?si=ju3MhMzCmff6QclQ)
____________________________
## Gates

```
// in AuthServiceProvider
public function boot(){
Gate::define('add-post',function(User $user,Post $post)){
return $user->id == $post->user_id;
}
```
Or you can **Policy** instead of clourse function
```
php artisan make:policy PostPolicy --model=Post
```
```
in App/policies/PostPolicy
public function update(User $user, Post $post): bool
    {
        return $user->id == $post->user_id;
    }
```
```
Gate::define('update-post',[PostPolicy::class,'update'])
```
[Gate](https://youtu.be/D1KoBL0dEcs?si=TYBOv3TIcgooKrwc)
```
if(Gate::allow('add-post',$post)){}
//OR
Gate::authorize('add-user',$post) return authorizationExpection
```
by default, we use gate:user, if i want to user for example
```
if(Gate::forUser($user)->allows('add-post')){}
```
_________
```
Gate::resource('post','App\Policies\PostPolicy')
//or
Gate::define('photos.view', 'App\Policies\PhotoPolicy@view');
Gate::define('photos.create', 'App\Policies\PhotoPolicy@create');
Gate::define('photos.update', 'App\Policies\PhotoPolicy@update');
Gate::define('photos.delete', 'App\Policies\PhotoPolicy@delete');
```
