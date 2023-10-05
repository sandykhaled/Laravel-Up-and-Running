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
```
Or Using Functions
```
Route::get('posts/{id}',['as','posts.show',function(){
//code
}]);
```
Grouping Routes<br/>
share a particular characteristic—a certain authentication
requirement, a path prefix, or perhaps a controller namespace
```
Route::group(finction(){
Route::get('hello',function(){
return 'hello';
});
Route::get('world',function(){
return 'world';
});
});
```
Use Middleware in group routes
```
Route::middleware('auth')->group(function(){
Route::get('dashboard',function(){
return view('dashboard');
});
});
```
before laravel 5.2 
```
Route::group(['middleware','auth'],function(){
Route:get('dashboard',function(){
return view('dashboard');
});
});
```
It is clearer to use middleware in your controller instead of using in route , you can use it in constructor method of controller <br\>
**Note** use $this->middleware because it is used in controller
```
public function __construct(){
$this->middleware('auth');
$this->middleware('auth')->only('view');
$this->middleware('auth')->expect('view');
}
```
If you want to limit number of times that user accessing any give route(s)  it is knowon as **Rate limiting** you can use throttle that takes 2 parameters <br/> the first is the number of tries a user is permitted 
<br/> the second is the number of minutes to wait before resetting the
attempt count
```
Route::middleware(['auth','throttle:60,1'])->group(function(){
Route::get('profile',function(){
});
});
```
**Dymanic Rate Limiting**
first parameter will pull the tries count instead of tries count as the first parameter , we will pass the name of attribute of elqouent 
<br/>like this
```
Route::middleware(['auth','throttle:plan_rate_limit,1'])->group(function(){
Route::get('profile',function(){
});
});
```
**Note** plan_rate_limit is an attribute in your model<br/>
**prefix group**
if your route share a segment of path <br/>
```
Route::prefix('dashboard')->group(function(){
Route::get('users',function(){
//dashboard/users
});
Route::get('/',function(){
//dashboard/
});
});
```
**Fallback routes**
use this when all routes don't match the incoming requests {eg. return page 404 }**Note** this route must be the end of your application
```
Route::fallback(function(){
});
```
before laravel 5.6 
```
Route::any('{anything}','CatchAllController')->where('anything','*');
```
**Subdomain Routes**
is the same like prefix routes there are 2 primary uses for this , first when you want present different sections of the application
```
Route::domain('api/example.com')->group(function(){
Route::get('/',function(){
});
});
```
second when you have parameter in your routes
```
Route::domain('{account}/example.com')->group(function(){
Route::get('/',function($account){
});
Route::get('/users',function($account,$id){
});
});
```
**Note** parameter must be the first parmeter in method <br/>
May be controller share the same namespace in this case , we can use namespace route
```
Route::namespace('Dashboard')->group(function(){
Route::get('users',['UserController','index'])
//app/Http/Dashboard/controllers/UserController
});
```
Route Prefix name
```
Route::name('users.')->prefix('users')->group(function(){
Route::name('admins')->prefix('admins')->group(function(){
Route::get('{id}',function(){
//users.admins.{id}
})->name('show');
});
});
```
**Signed Route**
allows Laravel to verify that the URL has not been modified since it was created.<br/> Signed URLs are especially useful for routes that are publicly accessible yet need a layer of protection against URL manipulation.
**How to create this**
1. at first make:controller SignatureController
```
class SignatureController extends Controller
{
public function send(){
//return URL::signedRoute('unsubscribe',['subscriber'=>1]);
return URL::temporySignedRoute('unsubscribe',now()->addSeconds(15),['subscriber'=>1]); //work for one time
}
public function unsubscribe($subscriber)
{
  if(!request->hasValidSignatrue){
    abort('403');
   }
  return User::find($subscriber)->name;
 //return $subscriber;
}
}
```
2. in routes/web.php
```
Route::get('send-mails',['SignatureController','send']); // this return unique url can't be modify
Route::get('unsubscribe/{subscriber}/now',['SignatureController','unsubscribe'])->name('unsubscribe')->middleware('signed');
```




