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
**Note - 3** 

_____________________________
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
    /**
     * Transform the resource into an array.
     *
     * @return array<string, mixed>
     */
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
