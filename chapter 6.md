## Vite
1. difference between **npm run dev** and **npm run build(npm run prod)**? <br/><br/>

**npm run dev** is suitable for development, offering fast compilation times and development-friendly features, <br/>
while **npm run prod (or npm run build)** is essential for production deployments, ensuring optimized assets for improved performance and efficiency in production environments. <br/>
It's crucial to use the appropriate command based on your current stage of development and deployment requirements. <br/>
________
**Note : Ensure to run "npm run build" during deployment as Laravel's default .gitignore ignores the public/build folder.**
_______
**Note : In JavaScript templates, Vite will process and version links to relative static assets, while ignoring links to absolute static assets. 
This means that images referenced in JavaScript templates will be treated differently based on whether they are relative or absolute links.**

```
<!-- Ignored by Vite -->
<img src="/resources/images/soccer.jpg">
<!-- Processed by Vite -->
<img src="../resources/images/soccer.jpg">
```

__________
**Note : To have Vite handle static assets in Blade templates, use the Vite::asset facade call to link your assets.**
```
<img src="{{ Vite::asset('resources/images/soccer.jpg') }}">
```
_____________
## Pagination

```
public function index()
{
 return view('posts.index', ['posts' => DB::table('posts')->paginate(20)]);
}
```

**To display results in view pages**

```
{{ $posts->links() }}
```
**Note : The paginator uses TailwindCSS for its default styling. 
If you want to use Bootstrap styles, call Paginator::useBootstrap()
in your AppServiceProvider**

```
use Illuminate\Pagination\Paginator;
public function boot(): void
{
 Paginator::useBootstrap();
}
```
