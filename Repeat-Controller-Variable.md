## What if you repeat variable in view or compact?
**ForExample**
```
Route::get('/users',function(){
return view('users')->with('uers',User::all());
});
```
```
Route::get('/',function(){
return view('home')->with('uers',User::all());
});
```
**We Repeat with !!!**
//In App\Providers\AppServiceProviders@boot
```
public function boot(){
view()->share('users',User::all());
}
```
OR
//In App\Http\ViewComposers\RecentPostsComposer@cpmpose
```
public function compose(View $view)
 {
 $view->with('recentPosts', Post::recent());
 }
```
```
public function boot(){
view()->composer(['users','home'],\App\Http\ViewComposers\RecentPostsComposer::class)
}
//Or
 view()->composer(['user','home'],function($view){
      $view->with('users',User::all());
        });
```
(LaravelDaily)[https://www.youtube.com/watch?v=dX2tCE0g7AU]

