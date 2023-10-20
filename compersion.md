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
**@foreach Vs @foreach & @include Vs @each**
```
@foreach($users as $user)
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
