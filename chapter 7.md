## Request 

```
return $request->filled('name');
// will return 1 if input is filled ,else return nothing
```

```
 return $request->whenHas('name', function ($name) {
            return $name; });
```
```
 return $request->whenFilled('name', function ($name) {
            return $name;
        });
```
```
 if($request->missing('name')){
            return 'ok';
        }
```
```
 return $request->mergeIfMissing(['name'=>'username']);
```
```
if($request->isMethod('get')){
    return 'ok';
}
```
```
return $request->method();
```
