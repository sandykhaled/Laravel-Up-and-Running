# Blade System

**Echo** <br/>
{{ $variable }} or {! $variable !} <br/>
**What if I want to print {{name}} ??** <br/>
I will use <br/>
@{{ name }} <br/>
__________________
**Control Structures**
1. <br/>
@if($condition) <br/>
@elseif($condition) <br/>
@else <br/>
@endif <br/>
<br/>
2. <br/>
@unless($condition) <br/>
@endunless <br/>
**Note** <br/>
instead of if(!$condition) and t doesn’t have a direct equivalent in
PHP <br/>
_________________
@forelse($invoices as $invoice)
    {{$invoice}}
@empty
    Not Found
    Not Found
@endforelse
**Instead of**
@foreach($invoices as $invoice)
@endforeach
_________
**Template Inheritance**
@yield() <br/>
@section() <br/>
@show <br/>
**Example-1** <br/>
<html> <br/>
 <head> <br/>
 <title>My Site | @yield('title', 'Home Page')</title> <br/>
 </head> <br/>
 <body> <br/>
 <div class="container"> <br/>
 @yield('content') <br/>
 </div> <br/>
 @section('footerScripts') <br/>
 <script src="app.js"></script> <br/>
 @show <br/>
 </body> <br/>
</html> <br/>

**Example-2**
@extends('layouts.master')<br/>
@section('title', 'Dashboard')<br/>
@section('content')<br/>
 Welcome to your application dashboard!<br/>
@endsection<br/>
@section('footerScripts')<br/>
 @parent<br/>
 <script src="dashboard.js"></script><br/>
@endsection<br/>

**Note**
Use @show when you’re defining the place for a section, in the par‐
ent template. Use @endsection when you’re defining the content
for a template in a child template 
**Note**
use <br/>
@section('title','Home Page') <br/>
Instead of <br/>
@section('title') <br/>
Home Page <br/>
@endsection <br/>
**Note**
In Parent Page <br/>
@yield('home') <br/>
@section('title') <br/>
@show <br/>
<br/>
In Class Page <br/>
@extends('home') <br/>
@section('title') <br/>
@endsection (Or @stop) <br/>
**Very Important Note** <br/>
**In Parent Page** <br/>
@section('sidebar') <br/>
   This is the master sidebar. <br>
@show <br>
**In Child Page**<br/>
@section('sidebar') <br/>
@parent // Instead of write the body of parent again <br/>
this is child's body <br/>
@endsection <br/>
