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
_-
