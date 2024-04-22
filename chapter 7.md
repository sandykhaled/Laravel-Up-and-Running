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
_____
**why i use isValid even i use validation?** <br/>
provides an extra layer of security and validation checks
_______
**you can specify the Eloquent model instead of the table name**
```
'name' => 'exists:App\Models\Contact,name',
'phone' => 'unique:App\Models\Contact,phone',
```
_______
**use Validator facade**
```
Validator::make($request->all(), [
        'title' => 'required|max:125',
        'body' => 'required'
    ]);
```
_________
```
if ($validator->fails()) {
 return redirect('recipes/create')
 ->withErrors($validator)
 ->withInput();
 }
```
_______
**to show all error messages**
```
->withErrors($validator)
```
________
**to save old value**
```
->withInput();
then in view page
<input type="text" name="title" value="{{ old('title') }}">
<input type="text" name="body" value="{{ old('body') }}">
```
____________
