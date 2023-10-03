# Laravel-Up-and-Running
## chapter 2
**Notes**
1. HTTPS verbs
   
   1. GET
   2. POST
   3. PUT
   4. DELETE
   5. HEAD
   6. OPTIONS
   7. PATCH
   8. TRACE
   9. CONNECT
       
 **Note** the last two are pretty much never used in
normal web development <br/>
2. **What’s a Closure?** <br/>
Closures are PHP’s version of anonymous functions. A closure is a function that you
can pass around as an object, assign to a variable, pass as a parameter to other func‐
tions and methods, or even serialize.
<br/>
<br/>
3. **why I use return instead of echo ?**
<br/>
there are a lot of wrappers around Laravel’s request and response cycle,
including something called middleware. When your route closure
or controller method is done, it’s not time to send the output to the
browser yet; returning the content allows it to continue flowing
through the response stack and the middleware before it is
returned back to the user   
<br/>
4.  **to avoid facades**
```
$router->get('/', function () {
 return 'Hello, World!';
});
```
Instead of
```
use Illuminate\Support\Facades\Route;

Route::get('/', function () {
 return 'Hello, World!';
});

```
To Handle get or post

```
Route::match(['get','post'],function(){
});
```
Route Parameter 
```
Route::get('posts/{id}/edit',function($id){
});
```
your parameter can be optional by including a question mark (?) , in this case you must provide a default value for the route’s corresponding variable.
```
Route::get('posts/{id?}',function($id=1233){
});
```
Using Regular Expression
```
Route::get('post/{id}',function($id){
})->where('id','[0-9]+');
```
```
Route::get('users/{user}',function($user){
})->where('user','[a-zA-Z]+');
```
```
Route::get('posts/{id}/{slug}', function ($id, $slug) {
})->where(['id' => '[0-9]+', 'slug' => '[A-Za-z]+']);
```
Named Routes
```
Route::get('posts/{id}','PostController@show')->name('posts.show');
```
Another Way
```
Route::get('posts/{id}',['as'=>'posts.show','uses'=>'PostController@show']);
Or Using Functions
```
Route::get('posts/{id}',['as','posts.show',function(){
//code
}]);
```
