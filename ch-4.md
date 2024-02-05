## Ch.4

1. stdClass => refer to **standard class** . <br/>
   It is an empty class that can be used to create objects on the fly without defining a specific class structure<br/>
   used to create generic objects .<br/><br/>
   **Note** <br/>
   **foreach()** and **forelse()** provide a special variable **$loop** which returns a **stdClass object**.<br/>
Example-1 
```
//$users = ['ahmed','ali','sara','sama']
@foreach($users as $user)
{{$user->name}} -
{{$loop->iteration}} <br/>
@endforeach
```
**Output**
```
ahmed - 1
ali - 2
sara - 3
sama - 4 
```
________________
Example-2
```
@foreach($users as $user)
{{$user->name}} -
{{$loop->first}} <br/>
@endforeach
```
**Output**
```
ahmed - 1
ali - 
sara -
sama -
```
___________________
Example - 3
```
@foreach($users as $user)
{{$user->name}} -
{{$loop->last}} <br/>
@endforeach
```
**Output**
```
ahmed -
ali -
sara -
sama - 1
```
___________________
Example - 4
```
@foreach($users as $user)
{{$user->name}} -
{{$loop->remaining}} <br/>
@endforeach
```
**Output**
```
ahmed - 3
ali - 2
sara - 1
sama - 0
```
___________________
Example - 5
```
@foreach($users as $user)
    {{$user->name}} -
    {{$loop->odd ? 'Yes' : 'No'}}  <br/>
@endforeach
```
**Output**
```
ahmed - Yes
ali - No
sara - Yes
sama - No
```
_________________
Example - 6
```
@foreach($users as $user)
    {{$user->name}} -
    {{$loop->even ? 'Yes' : 'No'}}  <br/>
@endforeach
```
**Output**
```
ahmed - No
ali - Yes
sara - No
sama - Yes
```
_______________
Example - 7
```
@foreach($users as $user)
    {{$user->name}} -
    {{$loop->depth}}  <br/>
@foreach($users as $user)
{{$user->email}}
{{$loop->depth}}
@endforeach
@endforeach
```
**Note first foreach return 1 , second foreach return 2**
________________
Example - 8
```
@foreach($users as $user)
    {{$user->name}} - <br/>
    {{$loop->count}} <br/>

@endforeach
```
**Output**
```
ahmed - 4
ali - 4
sara - 4
sama - 4
```
________________
Example - 9
```
@foreach($users as $user)
    {{$user->name}} -
    {{$loop->index}} <br/>
@endforeach
```
**Output**
```
ahmed - 0
ali - 1
sara - 2
sama - 3
```
______________
Example - 10
```
@php
    $categories = [
        'Electronics' => ['Laptop', 'Smartphone', 'Tablet'],
        'Clothing' => ['Shirt', 'Pants', 'Jacket'],
        'Books' => ['Fiction', 'Non-Fiction', 'Science'],
    ];
@endphp

@foreach ($categories as $category => $items)
    <h2>{{ $category }}</h2>

    <ul>
        @foreach ($items as $item)
            <li>
                {{ $item }}
                @if ($loop->parent)
                    {{-- Accessing the parent loop's category --}}
                    (Parent: {{ $loop->parent->iteration }})
                @endif
            </li>
        @endforeach
    </ul>
@endforeach

```
**Output**
```
Electronics

Laptop (Parent: 1)
Smartphone (Parent: 1)
Tablet (Parent: 1)

Clothing

Shirt (Parent: 2)
Pants (Parent: 2)
Jacket (Parent: 2)

Books

Fiction (Parent: 3)
Non-Fiction (Parent: 3)
Science (Parent: 3)
```
**Note parent written in nested loop**
**look the differnce**
```
@php
    $categories = [
        'Electronics' => ['Laptop', 'Smartphone', 'Tablet'],
        'Clothing' => ['Shirt', 'Pants', 'Jacket'],
        'Books' => ['Fiction', 'Non-Fiction', 'Science'],
    ];
@endphp

@foreach ($categories as $category => $items)
    <h2>{{ $category }}</h2>

    <ul>
        @foreach ($items as $item)
            <li>
                {{ $item }}
                @if ($loop->parent)
                    {{-- Accessing the parent loop's category --}}
                    (Parent: {{ $loop->iteration }})
                @endif
            </li>
        @endforeach
    </ul>
@endforeach

```
**Output**
```
Electronics

Laptop (Parent: 1)
Smartphone (Parent: 2)
Tablet (Parent: 3)

Clothing

Shirt (Parent: 1)
Pants (Parent: 2)
Jacket (Parent: 3)

Books

Fiction (Parent: 1)
Non-Fiction (Parent: 2)
Science (Parent: 3)
```
**Look the difference between $loop->parent->iteration and $loop->iteration**
_______________________________________________________
