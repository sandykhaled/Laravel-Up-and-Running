# Laravel-Up-and-Running
## chapter 2
the mian function of web application to take request from user and response (using Http for example) .. without routes you have little to no ability to interact with the end user. <br/>
**What is MVC?**
1. **Model** : deals with database
2. **View** : deals with User
3. **Controller** : like cop traffic that take request from user search in database , validate user input then retuen response to user <br/> <br/>
   **The user will first interact with the controller by sending the request , then controller will pull data from model , then return data (response) to user in view section (display in browser)**
___________________________
1. HTTPS verbs
   
   1. GET
   2. POST
   3. PUT
   4. DELETE
   5. HEAD
   6. OPTIONS (Ask server which verbs are allowed at this url)
   7. PATCH
   8. TRACE
   9. CONNECT
       
 **Note** The last two are pretty much never used in
normal web development <br/>
____________________________
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
**url() & route()**
```
<a href="{{route('')}}">
<a href="{{route('',['name'=>'Ali'])}}"> //if it has parameters
<a href="{{url('')}}">
```
**Prefer to use route() instead of url()**
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
use Illuminate\Support\Facades\URL;

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
**Note** For more details [signed Route](https://www.youtube.com/watch?v=tR9VHoj_kJM)  [Laravel Documatation](https://laravel.com/docs/10.x/urls#validating-signed-route-requests)
it's time for talking about View <br/>
**there are 3 ways to use view**
 1. view(); // it is popular
 2. View::make();
 3. inject your code Illuminate\View\ViewFactory

```
Route::get('show',function(){
return view('showUsers');
});
```

with variables

```
use Model/User;
Route::get('showUser',function(){
return view('showUsers')->with('users',User::all());
});
```
Instead of using 
```
Route::get('/',function (){
return view('welcome');
});
```
```
Route::view('/','welcome');
```
Or
```
Route::view('/', 'welcome', ['User' => 'Michael']);

```
May be you want to share the same data in all views in this case you can use in AppServiceProvider
```
class AppServiceProvider extends ServiceProvider
{
public function boot(){
View::share(['key','value'])
}
}
```
Create method in Controller
```
public function create(Request $request){
Task::create($request->only(['title','description']));
// only() method to pull just the title and description fields the user
submitted.
}
```
**typehint**
 - means putting the name of a class or interface in front of a variable in a method signature
```
public function store(Request $request){
}
```
**Note** php artisan route:list <br/>
//you’ll get a listing of all of the available routes <br/>
Make controller with API the same of the original resource controller without create , edit methods
```
php artisan make:controller ApiResourceController --api
```
in routes/web.php
```
Route::apiResource('Resources','ApiResourceController');
```
when a controller should only service a single
route
```
//inside TaskController
public function __invoke(){
// 
}
```
in routes/web.php
```
Route::post('update/{id}','TaskController')
```
**Route Model Binding**
1. Implicit
```
public function show($id){
$task=Task::where('id',$id);
return view('Tasks.show',compact('task'));
}
```
Instead of the previous code "for shorter code"
```
public function show(Task $task){
return view('Tasks.show',compact('task'));
}
```
**Note**  
[Laravel Daily Route Model Binding](https://www.youtube.com/watch?v=6dEfxGLgevM)
by default in url we search about id what if i search for name for example?
<br/>
->in this case ..  we use column key , and add this method to your model
```
public function RouteKeyName(){
return 'column_name';
}
```
or in routes/web.php
```
Route::get('tasks/{task:column_name}/update',['TaskController','update']);
```
**VIN**
what if column_name not found ? this will return page not found in this case , we can use 
The missing method accepts a closure that will be invoked if an implicitly bound model can not be found:
```
Route::get('tasks/{task:column_name}/update',['TaskController','update'])->missing(function(){
return view('notfound');
});

```
**Route Cache**
Using the route cache will drastically decrease the amount of time it takes to register all of your application's routes. if you add any new routes you will need to generate a fresh route cache. Because of this, you should only run the route:cache command during your project's deployment.
<br/>
```
php artisan route:cache
```
you can delete cache
```
php artisan route:clear
```
**VIN**prefer not to use the previous command
<br/>
**Method spoofing**
In laravel ,the method's form has only 2 verbs (Get , Post) what if i want to use another method like delete,put,patch?<br/>
In this case<br/>
```
<form method ="post" action="#">
@method('put') //recommanded
//or
<input type="hidden" value="put" name="_method">
</form>
```
**CSRF**
```
<form method ="post" action="#">
@csrf //recommanded
//or
<input type="hidden" value="<?= {{csrf_token()}}?>" name="_token">
//or before 5.6
{{csrf_filed}}
</form>
```
In javascript ,there is another code for csrf
```
<meta name="csrf-token" id="token" value="<?php csrf_token()?>">
```
```
// In jQuery:
$.ajaxSetup({
 headers: {
 'X-CSRF-TOKEN': $('meta[name="csrf-token"]').attr('content')
 }
});
// With Axios:
window.axios.defaults.headers.common['X-CSRF-TOKEN'] =
 document.head.querySelector('meta[name="csrf-token"]');
```
Redirect
```
Route::get('visit-link',function(){
    return redirect('login');
});
Route::get('visit-link',function(){
    return redirect()->to('login');
});
Route::get('visit-link',function(){
Route::redirect('login');
});
Route::redirect('visit-link','login',{3rd parameter is optional is status code});
```
```
return redirect()->route('con.index',['con'=>1]); // Nearly the same of redirect()->to() 
```
```
return redirect()->back();
//Or
return back();
```
```
Redirect::away("https://www.google.com");
return redirect()->away('https://www.google.com')

// Redirect to the "home" named route
return Redirect::route('home');

// Redirect to the same page (refresh the page)
return Redirect::refresh();
```
```

```
**Abort**
```
abort(status_code); <br/>
abort_if(condition,'status_code'); => inside the parathness must assign to true <br/>
abort_unless(condition,'status_code'); => inside the parathness mustn't assign to true <br/>
```
**response**
```
return response()->file($pathToFile);
 
return response()->file($pathToFile, $headers);
```
**Post Testing**
```
public function test_post_creates_new_assignment(){
$this->post('/assignments',[
'title'=>'assignment']);
$this->assertDatabaseHas('/assignments',['title','assignment']);
}
```
**Get Testing**
```
public function test_get_show_all_assignments(){
Assignmnet::create(['title'=>'assignmnet']);
$this->get('assignment')->assertSee('greate assignmnet');
}
```

**Reference Videos**
(Laravel Route Grouping-LaravelDaily)[https://www.youtube.com/watch?v=I6kyfSmPhn8]
