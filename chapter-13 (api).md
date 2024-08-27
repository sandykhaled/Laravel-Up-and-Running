**Note - 1** 
```
dd(response()->json(Dog::all()));  //return Json //Illuminate\Http\JsonResponse
dd(Dog::all()); //return Collection   //Illuminate\Database\Eloquent\Collection
```

<br/>

**Note - 2**
______________________________

```
php artisan make:model Dog --all
```

<br/>
**--all flag make resource controller - (create-update)Requset - Model - Migration - Factory - Seeder**

<br/>

________________________________
**Note - 3** 
<br/>

**Default paginate()**

```
return Dog::paginate(2);
```

<br/>

**Customize paginate() to jsonData**
```
php artisan make:resource DogResource
```
**//in Resource**
```

namespace App\Http\Resources;

use Illuminate\Http\Request;
use Illuminate\Http\Resources\Json\ResourceCollection;

class DogResource extends ResourceCollection
{
    public function toArray(Request $request): array
    {
        $pagination = $this->resource->toArray($request);

        return [
            'data' => $this->collection->transform(function ($dog) {
                return [
                    'id' => $dog->id,
                    'type' => 'dog',
                    'attributes' => [
                        'name' => $dog->name,
                    ],
                ];
            }),
            'links' => [
                'first' => $pagination['first_page_url'],
                'last' => $pagination['last_page_url'],
                'next' => $pagination['next_page_url'],
                'prev' => $pagination['prev_page_url'],
            ],
            'meta' => [
                'total-pages' => $pagination['last_page'],
                'total-count' => $pagination['total'],
            ],
        ];
    }
}
```
**In Controller**
```
public function index()
{
     $dogs = Dog::paginate(2);
     return new DogResource($dogs);
}
```
**Note : A JSON API document must include either data, errors, or meta at the top level, but not data and errors together.**
```
try {
        $dog = Dog::create($validated);
        return response()->json([
            'data' => [
                'id' => $dog->id,
                'type' => 'dog',
                'attributes' => $dog->toArray(),
            ],
        ], 201);
    } catch (\Exception $e) {
        return response()->json([
            'errors' => [
                [
                    'status' => '500',
                    'title' => 'Internal Server Error',
                    'detail' => 'An error occurred while processing your request.'
                ]
            ]
        ], 500);
```
**meta: Includes additional metadata about pagination, such as the total number of pages and items, the current page, and items per page.**
______________________________
**Note - 4**

**Filter Data**
<br/>
1. only one
   ```
   Route::get('dogs', function () {
    $query = Dog::query();
    $query->when(request()->filled('filter'),function ($query){
            [$criteria, $value] = explode(':', \request('filter'));
        // $query->where($criteria,$value);  //filter by word
        $query->where($criteria, 'LIKE', "%$value%"); //filter from first char
    });
    return $query->paginate(20);
});

   ```
2. Multi-Data
```
Route::get('dogs',function (){
    $query = Dog::query();
    $query->when(\request()->filled('filter'),function ($query){
        $filters = explode(',',\request('filter'));
        foreach($filters as $filter) {
            [$key, $value] = explode(':', $filter);
            // $query->where($key, 'LIKE',"%$value%"); //filter by word
            $query->where($key, 'LIKE',"%$value%"); //filter from first char
        }
    });
    return $query->get();
});

```
_______________

**Note-5**

```
namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class Dog extends Model
{
    
    public function toArray()
    {
        $attributes = parent::toArray();
        $attributes['full_name'] = $this->name . ' the ' . $this->breed;
        
        $attributes['formatted_date'] = $this->created_at->format('Y-m-d H:i:s');

        unset($attributes['email']);

        return $attributes;
    }
}

```

