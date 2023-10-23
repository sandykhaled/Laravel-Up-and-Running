## compersion
**@if VS @auth Vs @guest**
```
@if(Auth::check())
{{ __("You're logged in!") }}
@else
{{__("Logged off")}}
@endif
```
```
@auth
{{ __("You're logged in!") }}
@else
{{__("Logged off")}}
@endauth
```
```
@guest
{{__('you are a guest')}}
@else
{{__("you are an admin")}}
@endguest
```
_______________________________

**@if & @include VS @icludewhen**
```
@if($user->name=='ali')
    @include('welcome')
 @endif
```
```
@includeWhen($user->name=='ali','welcome')
```
_______________________________

**@if & @foreach @else VS @forelse**
```
@if($users->count() > 0)
  @foreach($users as $user)
       {{$user->name}} -- {{$user->email}} <br/>
   @endforeach
@else
       {{__('Not Found Data')}}
@endif
```
```
@forelse($users as $user)
     {{$user->name}} -- {{$user->email}} <br/>
@empty
     {{__('Not Found Data')}}
 @endforelse
```
_______________________________
**@foreach Vs @foreach & @include Vs @each**
```
@foreach($users as $user)
  //move to views/users.blade.php 
  <div>
      {{$user->name}} -- {{$user->email}} <br/>
  </div>
@endforeach
```
```
@foreach($users as $user)
  @include('users')
@endforeach
```
```
@each('users',$users,'user')
```
_______________________________
Now you have multi script tag <script> in 'views/layouts/app.blade.php' But you want to add new script differs from one page to another
**@stack & @push Vs @stack & @pretend** 
```
//in resources/views/layouts/app.blade.php
<script type="text/javascript" src="main.js">
 </script>
@stack('script')
```
```
//in resources/view/users/user.blade.php
//push in the bottom of stack
@push('script')
 <script type="text/javascript" src="example.js">
 </script>
@endpush
```
```
//pust at the top of stack
@pretend('script')
 <script type="text/javascript" src="example-2.js">
 </script>
@endpretend
```
Output
```
<script type="text/javascript" src="main.js"></script>
<script type="text/javascript" src="example-2.js"></script>
<script type="text/javascript" src="example.js"></script>
```
_______________________________
**@json** 
this most often will be used in vuejs or other js frameworks
```
 @json($users)
```
________________________________
**@verbatim**
Often we need to write {{name}} 
```
@verbatim
{{name}}
@endverbatim
```
**Note 
everything included are written as it is**
OR -without @verbatim-
```
@{{name}}
```
________________________________
**@choice**
will choose between user,users depending on count of users 
```
{{$users->count()}} @choice('user|users',$users->count())
```
________________________________
**@inject**
At first .. create App/Repositories/UserRepository ,then create function getAll()
```
public function getAll(){
     $users=User::all();
     return $users;
 }
```
In UserController
```
public function index(){
return view('users')
}
```
In views/users
```
 @inject('users',"App\Repositories\UserRepository")
 @foreach(($users->getAll()) as $user)
      {{$user->name}}
 @endforeach
```
This is sconaroi instead of create $users=User::all() in UserController that may be useful in some cases <br/>
_________________________________
**@commponent & @slot**
At first create component 
```
php artisan make:component Alert
```
//In Resources/views/components/Alert.blade.php **Single Slot**
```
<div>
Alert Page
<p>{{$slot}}</p>
</div>
```
In Resources/views/home.blade.php
```
@commponent('commponents.alert')
@slot
<p>Green ALert in home page</p>
@endslot
@endcomponent
```
//In Resources/views/components/Alert.blade.php  **Multi Slots**
```
<div>
Alert Page for mlti slots
<div class="alert-title">{{$title}}</div>
<div class="alert alert-danger">
    {{ $slot }}
</div>
</div>
```
In Resources/views/home.blade.php
```
@commponent('commponents.alert')
@slot('title')
<p>Green ALert for title in home page</p>
@endslot
oops ! this is an alert
@endcomponent
```

[Source](https://www.youtube.com/@codingwithstef6225)
[Slot&Commponent](https://www.youtube.com/watch?v=B-DTsHGbbTk)
