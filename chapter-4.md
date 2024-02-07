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
**Template Inheritance** <br/>
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

**Example-2** <br/>
@extends('layouts.master')<br/>
@section('title', 'Dashboard')<br/>
@section('content')<br/>
 Welcome to your application dashboard!<br/>
@endsection<br/>
@section('footerScripts')<br/>
 @parent<br/>
 <script src="dashboard.js"></script><br/>
@endsection<br/>

**Note** <br>
Use @show when you’re defining the place for a section, in the par‐
ent template. Use @endsection when you’re defining the content
for a template in a child template 
_____________________
**Note** <br/>
use <br/>
@section('title','Home Page') <br/>
Instead of <br/>
@section('title') <br/>
Home Page <br/>
@endsection <br/>
_____________________
**Note**
**In Parent Page** <br/>
@yield('home') <br/>
@section('title') <br/>
@show <br/>
<br/>
**In Class Page** <br/>
@extends('home') <br/>
@section('title') <br/>
@endsection (Or @stop) <br/>
**Very Important Note** <br/>

**In Parent Page** <br/>
```
@section('sidebar') <br/>
   This is the master sidebar. <br>
@show <br>
```
**In Child Page**<br/>
```
@section('sidebar') <br/>
@parent // Instead of write the body of parent again <br/>
this is child's body <br/>
@endsection <br/>
```
**Note** <br/>
@include <br/>
**In Home Page**
```
@include('sign-up-button', ['text' => 'See just how great it is'])

```
**In sign-up-button page**
```
{{$text}}
```
```
{{-- Include a view if it exists --}}
@includeIf('sidebars.admin', ['some' => 'data'])
{{-- Include a view if a passed variable is truth-y --}}
@includeWhen($user->isAdmin(), 'sidebars.admin', ['some' => 'data'])
{{-- Include the first view that exists from a given array of views --}}
@includeFirst(['customs.header', 'header'], ['some' => 'data'])

```
what if i want to loop @include for 5 times ? <br/>
in this case I will write @include() 5 times , but it's not a clean code <br/> 
So we prefer to use @each
```
@each('page-name',$array-name,'var-name','optional-view-if-it-is-empty');
```
Forexample
```
@include('view')
@foreach($projects as $project)
{{$project}}
@endforeach
```
```
@each('view',$projects,'project','welcome');
```
**Note  <br/> @each is not equal to @foreach**
<br/> Example-2 <br/>
**In jobController**
```
public function getJobs(){
jobs=[
['job1'=>'title1'],
['job2'=>'title2']
];
return view('jobs.job',compact('jobs'));
}
```
**In views/jobs/job.blade.php**
```
@each('jobs.getJob',$jobs,'job','jobs.notFind')
```
**In view/jobs/getJob.blade.php**
```
<div>
{{$job['title']}}
</div>
```
**Output**
```
title1
title2
```
______________
**$slot**<br/>
Example-1
```
// in components/alert.blade.php
<div>
This is title from main page
{{$slot}}
</div>
```
```
<x-alert>
this is title form secondary page
</x-alert>
```
 Example-2
```
// in components/alert.blade.php
<div>
This is title from main page
<span style="color:red">{{$title}}<span>
<span style="color:green">{{$content}}</span>
</div>
```
```
<x-alert>
<x-slot:title>
title 1
</x-slot>
<x-slot:content>
this content span
</x-slot>
</x-alert>
```
___________________________
## Method Components

1- Make component class
```
php artisan make:component Alert
```
in **app/View/Component/Alert.php**
```
public function showMessage($msg){
return $msg;
}
```
in **resources/views/components/alert.blade.php**
```
{{$showMessage('this is msg')}}
```
in your view page
```
<x-alert/>
```
_________________________________
## View Composer 
  **ِApp/providers/appservicesprovider.php**<bdi> في</bdi>share view<bdi>كل مرة هستخدم</bdi>compact<bdi> بدل ما امرر </bdi>view <bdi>في كذا </bdi>variable <bdi> ليه انا ممكن استخدمه ؟ لو عندي </bdi>

























