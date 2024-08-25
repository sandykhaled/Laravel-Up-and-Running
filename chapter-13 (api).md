**Note - 1** 
```
dd(response()->json(Dog::all()));  //return Json //Illuminate\Http\JsonResponse
dd(Dog::all()); //return Collection   //Illuminate\Database\Eloquent\Collection
```

<br/>
**Note - 2**
```
php artisan make:model Dog --all
```
**--all flag make resource controller - (create-update)Requset - Model - Migration - Factory - Seeder**
