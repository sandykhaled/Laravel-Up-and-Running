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
