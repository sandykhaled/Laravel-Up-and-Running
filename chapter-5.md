## DataBase and Elqouent
```
php artisan migrate:rollback --step=3 
```
```
php artisan make:migration create_posts_table
php artisan make:migration create_posts_table --create=posts
php artisan make:migration add_title_posts_table --table=posts
```
_____________
**1-Note**
**--create = posts** When Create New Table <br/>
**--table = posts** When Modify Table <br/>
____________
**Columns Data Type**
1. **integer(ColName) , tinyInteger(ColName) , smaillInteger(ColName) , mediumInteger , bigInteger**
2. **increments(colName) and bigIncrements(colName)**
   
   Add an unsigned incrementing INTEGER or BIG INTEGER primary key ID

3. **Enum(ColName,[choice1,choice2,..])**
```
Schema::create('my_table', function (Blueprint $table) {
            $table->enum('status', ['Active', 'Inactive', 'Pending']); // Example ENUM with choices
        });
```
4. **Boolean or Tinyint** 

      For two genders (male and female) with no future additions, use **tinyint(1)** or **Boolean()** with 0 for female and 1 for male. When expansion is 
      needed, modify the schema to an **ENUM**.

5. **string(colName, optional length)**
6. **text(colName) , mediumText(colName) ,longText(colName)**

      **text** is for shorter text, **mediumText** for medium-length text, and **longText** for large blocks of text or content.
   
7. **char(colName,optional length)**
8. **binary(colName)**

     This column will store binary data, such as **images**, **files**, or any **binary data** you need to store in your database.

9. **decimal(colName,precision,scale)** <br/>
   ```
   decimal("rate",5,2)
   ``` 
   
    **precision** specifies the total number of digits a DECIMAL column can hold. It controls the maximum digits, including those after the decimal 
       point. For example, it allows values like **123.45** or **12.345**. <br/>
    **scale** specifies the number of digits to the right of the decimal point. For example, it allows values like **123.45**, not 12.345.   

10. **double(colName, total digits, digits after decimal)**
12. **float(colName, precision, scale)**
13. **datetime(colName)**
14. **time(colName)**
15. **timestamp(colName)**
    used to store date and time information.
```
$table->timestamp('email_verified_at')->nullable();
```
16. **timestamps() and nullableTimestamps()**

     method is used to add two timestamp columns to a table: **created_at** and **updated_at.**
17. **json(colName) and jsonb(colName)**
18. **morphs(colName)**
    
     For a provided colName, adds an integer colName_id and a string colName_type
```
morphs('tag') ; //means tag_id, tag_type used in polymorphic relationships
```  
19. **softDeletes()**
20. **rememberToken()**
     method adds a VARCHAR(100) column named remember_token to the specified database table. This column is used to store a token value for each user.
21. **uuid(colName)**
    are unique identifiers that are useful in scenarios where you need to ensure that each record in a table has a globally unique identifier. They are 
     commonly used in distributed systems and applications where uniqueness is critical. (CHAR(36) in MySQL)
__________________________
**More Properties after creation of the column**
1. **nullable()**
2. **default(value)**
3. **after(colName)**
4. **first()**
   ```
    public function up(): void
    {
        Schema::create('comments', function (Blueprint $table) {
            $table->id();
            $table->string('title');
            $table->timestamps();
        });

        // Use first() to place 'title' column as the first column
        Schema::table('comments', function (Blueprint $table) {
            $table->text('description')->first();
        });
    }
   ```
   **Note** if I add column description at the first schema will return error  .. **Try it**
5. **unique()**
```
$table->string('email')->unique();
```
6. **unsigned()**
   (not negative or positive, but just an integer)<br/>
   ensure that the column will not accept negative values, which is suitable for fields like quantities, counts, or identifiers that should not 
   have negative values.
7. **primary()**
   ```
   $table->primary('ssn');
   //or
   $table->integer('ssn')->primary();
   ```   
8. **index()**
   
   method can improve the speed of SELECT queries but might slightly slow down INSERT, UPDATE, or DELETE operations because the index needs to be updated.  
__________________
**Modifying columns**
1. **change()**
Now .. I have this column and I want to change datatype should I **refresh** my migration? of course **No**
```
Schema::create('users', function (Blueprint $table) {
            $table->text('name');
        });

        Schema::table('users', function (Blueprint $table) {
            $table->string('name',200)->change();
        });
```
**Example-2**
```
        Schema::create('users', function (Blueprint $table) {
            $table->string('name');
        });

        Schema::table('users', function (Blueprint $table) {
            $table->text('name')->default('ali')->change();
        });
```

2. **removeColumn()**
   ```
    Schema::create('users', function (Blueprint $table) {
            $table->string('name');
            $table->removeColumn('name');


        });
   ```
 3. **renameColumn()**
    <br/>
    At first In create migration **create_posts_migration**
    ```
    Schema::create('posts', function (Blueprint $table) {
            $table->id();
            $table->string('name');
            $table->timestamps();
        });
    ```
    <br/>
  then create **new migration** as **rename_name_column_in_posts_table**
``` public function up()
    {
        Schema::table('posts', function (Blueprint $table) {
            $table->renameColumn('name', 'names'); // Correct usage to rename 'name' to 'names'
        });
    }

    public function down()
    {
        Schema::table('posts', function (Blueprint $table) {
            $table->renameColumn('names', 'name'); // Reverse the column name change
        });
    }

```
4. **dropColumn()**
   create **new migration** as **remove_name_column_from_posts_table**
```
   Schema::table('contacts', function (Blueprint $table)
{
 $table->dropColumn('votes');
});
```
**Note If you try to drop or modify multiple columns within a single
migration closure , you can use one  Schema::table() within up() instead of creating one for each drop or modify**
```
public function up()
{
 Schema::table('contacts', function (Blueprint $table)
 {
 $table->dropColumn('is_promoted');
 });
 Schema::table('contacts', function (Blueprint $table)
 {
 $table->dropColumn('alternate_email');
 });
}
```
______________________________
**Squashing migrations**
<br/>
If you have too many migrations, you can merge them all <br/>
into a single SQL file that Laravel will run before it runs any future migration <br/>
```
php artisan schema:dump
```
**Note make Schema Dir with file has all migration tables**
```
php artisan schema:dump --prune
```
**Note the same of prev command but delete migration dir**
______________________________
**Note : If you use schema dumps, you can’t use in-memory SQLite; it only works on
MySQL, PostgreSQL, and local file SQLite.**
______________________________
**unique() & primary() & index()**
```
$table->primary('primary_id'); // Primary key; **unnecessary if used increments() or bigIncrements() or id()**
$table->primary(['first_name', 'last_name']); // Composite keys
$table->unique('email'); // Unique index
$table->unique('email','unique_email'); // rename Unique column
$table->index('amount'); // Basic index
```
**dropUnique() & dropPrimary() & dropIndex()**
create new migration **drop_primary_from_posts_table**
```
 public function up(): void
    {
        Schema::table('posts', function (Blueprint $table) {
            $table->dropPrimary('name');

        });
    }
```
**Note : put column in [] in dropUnique() & dropIndex()**
```
 $table->dropUnique(['name']);
```
```
 $table->dropIndex(['name']);
```
**Or**
```
 $table->dropUnique('posts_name_unique']);
```
```
 $table->dropIndex('posts_name_index']);
```
______________
**foreign key**
```
$table->foreign('user_id')->references('id')->on('users');
```
Or 
```
$table->foreignId('user_id')->constrained();
```
**Drop foreign key**
```
 $table->dropForeign('posts_user_id_foreign'); //posts == table name
```
or **put foreign key in []**
```
$table->dropForeign(['user_id']);
```
___________________
**Migration Commands**
1. **php artisan migrate**
2. **php artisan migarte:reset** just run migration without tables -tables are pending-
3. **php artisan migrate:refresh** the same as **migarte:reset** put run migration again (rollback then create again)
4. **php artisan migrate:fresh** the same as **migrate:fresh** but doesn’t bother with the “down” migrations
5. **php artisan migrate:status**
6. **php artisan migrate:rollback**
7. **php artisan migrate:install** // you can ignore it
 _____________
8. **php artisan migrate --seed** 
equal to : <br/>
**php artisan db:seed** and **php artisan migrate**
_______________
**php artisan db:monitor** => show database connection <br/>
**php artisan db:table** => show all tables with all data <br/>
**php artisan db:show** => show all tabled only <br/>
___________________
## Seeder <br/>
**to create seeder**
```
php artisan make:seeder UserSeeder
```
**In database/seeder/UserSeeder**
```
 public function run(): void
    {
        User::create([
            'name'=>'aly',
            'email'=>'ali@aly.com',
            'password'=>Hash::make(12345678)
        ]);
    }
```
**To run seeder**
```
php artisan db:seed --class=UserSeeder
```
What if I have **multi** seeders?
**In database/seeder/DatabaseSeeder**
```
$this->call(UserSeeder::class,PostSeeder::class);
```
**To run seeder**
**Note you must run command after every migrate:f or refresh**
```
php artisan db:seed
```
**To avoid error with use command more than a time(duplicate data)**
```
 public function run(): void
    {
        User::truncate();
        User::create([
            'name'=>'aly',
            'email'=>'aly@aly.com',
            'password'=>Hash::make(12345678)
        ]);
    }
```
**To run a seeder along with a migration**
```
php artisan migrate --seed
```
```
php artisan migrate:refresh --seed
```
______________________
**Factory**
```
php artisan make:factory BookFactory --model=book
```
then in BookFactory
```
 public function definition(): array
    {
        return [
            'title'=>$this->faker->title,
            'author_name'=>$this->faker->name
        ];
    }
```
in model **Book**
```
protected static function newFactory()
{
 return \Database\Factories\BookFactory::new();
}
```
**this is not mandatory this must be used when you don't follow convention name of laravel** <br/>
in this case you must **define model in factory** <br/>

```
protected $model = "Books";
```

then in  **tinker**


```
App\Models\Book::factory()->create()  

```


when you want to write **multiple instances**

```
App\Models\Book::factory(10)->create()  
App\Models\Book::factory()->count(10)->create()  
   ```               
**first statement is shorthand of second statement**<br/>
**we can run one of two methods on it: make() or create()**
______________________
**I want to create 20 users and each user has 2 books**<br/>
At first make relationship between user and book models
```
//in user Model
public function books(){
return $this->hasMany(Book::class);
}
```
```
//in book Model
public function user(){
return $this->belongsTo(User::class);
}
```
**Don't forget to create userseeder, bookseeder and complete migration with relationship**
```
App\Models\User::factory()->count(20)->has(App\Models\Book::factory()->count(2))->create()
```
**Note has() function must Write relationships with 2 models in model** 
[has function](https://stackoverflow.com/questions/64636625/laravel-generate-multiple-models-using-factory-hasmany-relationship)

_____________________
**Example 5-11. Using values from other parameters in a factory**
```
//in contacts table

$table->unsignedBigInteger('company_size')->constrained('companies', 'size');
```
```
//in compaines table

$table->unsignedBigInteger('size');
```
```
// in CompanyFactory

 return[
        'name' => fake()->company,
        'size' => fake()->randomFloat(2, 1, 100), 
        ];
```
```
//ContactFactory

return [
            'name'=>'sandy khaled',
            'email'=>'sandy@gmail.com',
            'company_id'=>Company::factory(),
            'company_size'=>function (array $attributes) {
                return Company::find($attributes['company_id'])->size;
            }
        ];
```
___________________
**for() and has()** <br/>
**for() define that the instance we’re creating belongsTo()** <br/>
**has()  define that the instance we’re creating hasMany()** <br/>
```
// create 10 books for 1 user

Book::factory()->create(10)->for(User::factory)->create()
```
```
//create 10 users each user has 2 books
User::factory()->count(10)->has(Book::factory()->count(2))->create();
```
**Note: This code may lead to an error.**

```
Book::factory()->count(10)->for(User::factory()->count(2))->create();
```
___________
**Sequence will determine only the attributes that you specify:**
```
User::factory()->count(10)->has(Book::factory()->count(2)->sequence(
            ['title'=>'mrssss'],['title'=>'mr...']
        ))->create();
```
_____________
```
 User::factory()->count(10)->has(Book::factory()->count(2)->state(function (array $attributes, User $user) {
            return ['title' => $user->name ];
        }))->create();
```
**this means title of books equal to name of users table**
____________
```
User::factory()
            ->count(3)
            ->has(Book::factory()->sequence(['title'=>'Company Title']))->create();
```
**The sequence method must have variables on the nearer model.**
_______________
**state is used for dynamic customization of attributes based on the parent model's state,
while sequence is used for generating sequential values for a specific attribute.** <br/>
_________________
```
 $users = User::factory()
            ->count(10)
            ->sequence(fn (Sequence $sequence) => ['name' => 'Name '.$sequence->index])
            ->create();
```
**output sequence refers to id if id = 1 , name=Name 0**  
________________________________
## Query Builder

**Fluent Vs non-Fluent**
<br/>
**//Fluent**
<br/> A fluent interface allows the usage of multiple sequential methods, where each method call typically returns an instance of the object it belongs to, enabling a chain of consecutive method calls.<br/>
```
class Car {
    private $color;

    public function setColor($color) {
        $this->color = $color;
        return $this;  // Returning $this allows for method chaining
    }

    public function setBrand($brand) {
        // Set brand logic
        return $this;
    }

    public function drive() {
        // Drive logic
    }
}

// Using the fluent interface
$myCar = new Car();
$myCar->setColor('blue')->setBrand('Toyota')->drive();
```
**//non-Fluent**<br/>
"A non-fluent interface requires separate and distinct method calls for each operation, where each method does not necessarily return the instance of the object, necessitating individual calls for different functionalities."
```
class Car {
    private $color;

    public function setColor($color) {
        $this->color = $color;
    }

    public function setBrand($brand) {
        // Set brand logic
    }

    public function drive() {
        // Drive logic
    }
}

// Using a non-fluent interface
$myCar = new Car();
$myCar->setColor('blue');
$myCar->setBrand('Toyota');
$myCar->drive();
```
_______________________
**Raw SQL**

```
DB::statement("drop from users table");
DB::select("SELECT * FROM users");
DB::table('users')->get();
```

**delete(),update(),select(),insert()**
```
$users = DB::table('users')->select('name', 'email')->get();
DB::table('users')->insert([
    'name' => 'John Doe',
    'email' => 'john@example.com',
]);
$affectedRows = DB::table('users')->where('id', 1)->update(['name' => 'UpdatedName']);
$affectedRows = DB::table('users')->where('id', 1)->delete();
```
```
$user= DB::select("SELECT * FROM users WHERE id = ? AND name = 'ali'",[$id]);
$user= DB::select("SELECT * FROM users WHERE id = :id AND name = 'ali'",['id',$userId]);
```
**insert()**
```
$user= DB::insert("insert into users(name,email,password)VALUES (?,?,?)",['hany','hany@hany.com','12345678']);
```
**delete statement**
```
$user= DB::delete("delete from users WHERE name = ? ",['Ali']);
```
**update()**
```
$user= DB::update("update users set name = 'ali' where name = ? ",['hany']);
```
____________________
**select & addSelect()**
```
DB::table('users')->select('email','password as passUser')->get();
//Or
DB::table('users')->select('email')->addSelect('password ass passUser')->get();
```
_______________________
**where()**
```
$user = DB::table('users')->where('created_at','>',now()->subDay())->get();
// will return any date after 24 hours ago ...
```
**where([[],[]])**
```
$user = DB::table('users')->where([['created_at','>',now()->subDay()],['id','<','9']])->get();
// multiple where conditional can be written as where([[],[]])
```
**orWhere()**
```
$user = DB::table('users')->where('created_at','>',now()->subDay())->orWhere('id','<','6')->get();
//return any condition that matches
```
**complex orWhere()**
```
$user = DB::table('users')->where('created_at','>',now()->subDay())->orWhere(function ($query){
            $query->where('id','>','6')->where('name','salem');
        })->get();
```
```
$user = DB::table('users')->where('created_at','>',now()->subDay())->orWhere('id','>',
'6')->where('name','salem')->get();
//"select * from `users` where `created_at` > ? or `id` > ? and `name` = ?"
```
**whereBetween()**
```
$user = DB::table('users')->whereBetween('id',[1,9])->get();
```
**whereIn()**
```
$user = DB::table('users')->whereIn('id',[1,9])->get();
```
**whereNotIn()**
<br/>
**whereNotNull() & whereNull()**
```
$user = DB::table('users')->whereNotNull('created_at')->get();
```
```
$user = DB::table('users')->whereNull('created_at')->get();
```
**whereRaw()**
```
$name="sandy";
$user = DB::table('users')->whereRaw('name = ?',$name)->get();
//Or
$user = DB::table('users')->whereRaw("name = 'salem'")->get();
```
__________
**distinct()** 
```
$user = DB::table('users')->select('name')->distinct()->get();
// return only unique name form table users
```
**orderBy()**
```
$user = DB::table('users')->select('name')->distinct()->orderBy('name','Desc')->get();
// return unique name order by desc
```
___________
**skip(),take()**
```
$user = DB::table('users')->skip(2)->take(4)->get();
```
________
**latest() Vs oldest()**
```
$user = DB::table('users')->oldest()->take(3)->get();
//ascending
$user = DB::table('users')->latest()->take(3)->get();
//descending
```
___________
**groupBy()**
```
 $book = DB::table('books')
            ->select('user_id',DB::raw('COUNT(*) as count'))
            ->groupBy('user_id')
            ->having('count','>','1')
            ->get();
```
```
 $book = DB::table('books')
            ->select('user_id',DB::raw('COUNT(*) as count'))
            ->groupBy('user_id')
            ->havingRaw('count > 1')
            ->get();
```
**having() & havingRaw()**
```
havingRaw($sql)
having($column,$operator,$value)
```
________
**inRandomOrder()**
```
$user = DB::table('users')->inRandomOrder()->take(3)->get(); //change each refresh
```
_________
**count()** 
```
 $book = DB::table('books')->where('user_id','=','1')->count();
// return count of user_id = 1
```
________
**find()**
```
 $book = DB::table('books')
           ->find(1,['user_id','name']);
// find($id,$columns[optional]) return only user_id,name from query 
```
_________
**get()**
```
$book = DB::table('books')->where('user_id',1)->get(); //return all data that match condition user_id=1
$book = DB::table('books')->get(); //return all data 
```
**Note: there is no ->all() in DB**
________
**first()**
```
$book = DB::table('books')->orderBy('user_id','desc')->first();
// return only first column match the condition
```
```
$book = Book::where('user_id','2')->first(); // if there is no user_id = 1 , return empty page
$book = Book::where('user_id','2')->firstOrFail(); //return not found
```
**Note findOrFail() & firstOrFail() don't use in DB** <br/>
**first() fails silently if there are no results, Vs
firstOrFail() will throw an exception**
______________
**value()**
```
$book = DB::table('books')->orderBy('user_id')->value('name');
//value($column)
//return only name column from the matching condition
```
__________
**min() & max()**
```
$book = DB::table('books')->min('user_id');
$book = DB::table('books')->max('user_id');
```
**sum() & avg()**
```
$book = DB::table('books')->sum('user_id');
$book = DB::table('books')->avg('user_id');
```
__________
**dd()**
```
$user = DB::table('users')->where('name', 'Wilbur Powery')->dd();
// SELECT * FROM users WHERE name = `Wilbur Powery`
```
**dump()**
```
dump(DB::table('books')->where('options->isAdmin','false'));
```
________
**explain()**
```
$user = DB::table('users')->explain()->dd();
// return array has table name , no. of rows ,and other data
```
___________
**DB::raw()**
```
//Custom Calculations
$book= DB::table('books')->select(DB::raw(' user_id * 10 as NID '))->get();
//Using Aggregation Functions (MIN(),MAX(),COUNT())
$book = DB::table('books')->select('user_id',DB::raw('COUNT(*) as count'))->groupBy('user_id')->get();
```
________
**join**
```
//inner join
 $book = DB::table('books')->
        join('users','user_id','=','users.id')->select('books.*','users.name','users.email')->get();
```
**Complex join**
```
$book = DB::table('books')->
        join('users',function ($join){
            $join->on('user_id','=','users.id')
            ->orOn('user_id','=','users.name');
        })->get();
```
________
**union()**
```
 $first = DB::table('books')->whereNull('name');
        $book = DB::table('books')->whereNull('user_id')->
        union($first)->get();
```
**insert() , insertGetId()**
```
DB::table('books')->insertGetId(['name'=>'book10','user_id'=>'2']);
insertGetId // when you want to return id
DB::table('books')->insert(['name'=>'book10','user_id'=>'2']);
```
___________
**update()**
```
DB::table('books')->where('name','=','book10')->update(['name'=>'book200','user_id'=>'6']);
```
_______________
**increment() & decrement()**
```
DB::table('books')->increment('id','100');
DB::table('books')->decrement('id');
```
_________________
**delete() & truncate()**
```
DB::table('books')->where('name','=','10book10')->delete();
DB::table('books')->truncate(); // delete all rows from DB
```
_______
for **json()**
```
//in Migration
$table->json('options')->nullable();
```
```
//in Factory
'options'=>json_encode(['isAdmin'=>true])
```
```
//to get
DB::table('books')->where('isAdmin','true')->get();
//to update
DB::table('books')->update(['isAdmin'=>false]);
```
________________
**DataBase Transaction**
```
DB::transaction(function () {
            $user = User::create([
                'name' => 'yasser',
                'email' => 'yassr@yahoo.com',
                'password' => Hash::make('12345678')
            ]);
            $new = User::where('id','4')->update(['name'=>'sara']);
        });
```
Or use **global variable**
```
$name= 'merna';
        DB::transaction(function () use ($name)  {
            $user = User::create([
                'name' => 'yasser',
                'email' => 'yaso@yahoo.com',
                'password' => Hash::make('12345678')
            ]);
            $new = User::where('id','4')->update(['name'=>$name]);
        });
```
**Manual Transaction**
```
        DB::beginTransaction();
        $user = User::create([
            'name'=>'yasser',
            'email'=>'yasser@gail.com',
            'password'=>Hash::make('12345678')
        ]);
        $new = User::find(3);
        if(!$new)
        DB::rollBack();
        DB::commit();
```
__________________
**where()&first() Vs firstWhere()**
```
$user=User::firstWhere('id','3');
$user=User::where('id','3')->first();
```
____________________
**all() & get()**
```
User::where('id',3)->get();
User::all(); // without where()
```
**So, even though all() is very common, I’d recommend using get() for everything and ignoring the fact that all() even exists.**
____________________
**o process a large amount (thousands or more) of
records at a time**
```
User::chunk(20,function ($users){
            foreach ($users as $user){
                echo $user->name."<br/>";
            }
        });
```
**it will return all data but in chunks, each chunk size is 20**
<br/>
**Instead of**
```
 $users = User::all();
            foreach ($users as $user){
                echo $user->name."<br/>";
            }
```
_____________________
**Aggregates**
```
$users = User::whereIn('id',[3,4,6,7])->count();
$users = User::sum('id'); 
$users = User::avg('id');

```
_____________
**Insert**
```
//First Way
User::make([
         'name'=>'ali',
         'email'=>'ali@ali.com',
         'password'=>Hash::make('12345678')
     ])->save();
```
```
// Second Way
 $user = new User([
         'name'=>'sandy',
         'email'=>'sandy@sandy.com',
         'password'=>Hash::make('12345678')
     ]);
     $user->save();
```
```
//Third Way
 $user = new User();
     $user->name = 'salma';
     $user->email = 'salma@salma.com';
     $user->password = Hash::make('12345678');
     $user->save();
```
```
//Forth and Popular Way
User::create([
            'name'=>'ahmed',
            'email'=>'ahmed@ahmed.com',
            'password'=>Hash::make('12345678')
        ]);
```
**In the make() method, you must use save(), while in the create() method, there is no need to use save().**
______________
**update()**
```
//first way
$id = 3;
User::findOrFail($id)->update([
  'name'=>'mhmd',
  'email'=>'mhmd@mhmd.com',
  'password'=>Hash::make('12345678')
        ]);
```
```
$id = 3;
$user = User::find($id);
$user->name = 'sara';
$user->email = 'sara@sara.com';
$user->save();
```
**Note in prev example, you can't use where() instead of find()** <br/>
**if you want to use where(), in this case you must use first() with where()**
```
$id = 3;
$user = User::where('id', $id)->first();
$user->name = 'sara';
$user->email = 'sara@sara.com';
$user->save();
```
**Very Importmant Note : if you use find() with first() it will return first row in db**
```
$id = 3;
$user = User::find($id)->first();
$user->name = 'sara';
$user->email = 'sannnn@sara.com';
$user->save();
```
**the prev code will update user with id = 1 not user with id = 3** 
______________________
**$fillable & $guard**
```
protected $fillable = ['name','email','password']; //determine which fields can be edited/created
protected $guard = ['id','updated_at','created_at']; // determine which fields cannot be edited/created
```
________________
**$request->all() & $request->only()**
```
 $contact->update($request->all());
 $contact->update($request->only(['name','email']));
```
________________
**firstOrCreate() & firstOrNew()**
```
$user = User::firstOrNew([
      'name'=>'ali3',
      'email'=>'ali3@ali.com',
      'password'=>Hash::make('12345678')
        ])->save();
```
```
$user = User::firstOrCreate([
          'name'=>'ali3',
          'email'=>'ali3@ali.com',
          'password'=>Hash::make('12345678')
        ]);
```
**Note firstOrNew() must have save() to save on database but in firstOrCreate() No need to use save()** <br/>
**firstOrNew() eill return 1 if it's found, but firstOrCreate will return data of user** <br/>
____________________
**delete() & destory()**
```
User::find(3)->delete(); // delete() doesn't have values
```
```
User::destroy(3);
```
**if you have condition, use delete()**
```
 User::where('name','ali')->delete();
```
___________
**softDeletes()**
//in Model
```
use softDeletes;
```
in Migration
```
$table->softDeletes();
```
Then in ur controllers
```
User::withTrashed()->get(); //return all rows
User::withoutTrashed()->get(); //only row without softDelete
User::onlyTrashed()->get(); //only rows with softDeletes
```
**trashed()** // use for rows that already in trash
```
$users = User::onlyTrashed()->get();
foreach ($users as $user){
if ($user->trashed()) {
$user->forceDelete();
return $user->trashed();
   }
}
```
**forceDelete()**
```
//delete data perment from database
$user->forceDelete();
User::onlyTrashed()->forceDelete();
```
**restore()**
```
User::onlyTrashed()->restore();
$user = User::onlyTrashed()->get();
$user->restore();
```
______________
**Local Scopes**
```
//in User Model
public function scopeActive(Builder $builder,$id){
return $builder->where('id',$id);
}
```
```
//in User controller
User::active(7)->update(['name'=>'sarah']);
```
**Using 2 local Scopes with orWhere() condition**
```
$user = User::active(5)->orWhere(function($query){
$query->name('farah');
})->get();
```
**Using 2 or more local scopes**
```
User::active(5)->name('farah')->age(25)->get();
```
________________
**Global Scopes**
```
public static function booted()
     {
         static::addGlobalScope('first',function (Builder $builder){
             $builder->where('name','sarah');
         });

     }
```
**Or create class for global scope**
```
php artisan make:scope ActiveScope
```
```
class ActiveScope implements Scope
{
 public function apply(Builder $builder, Model $model): void
 {
 $builder->where('active', true);
 }
}
```
```
//in Model
public static function booted(){
static::addGlobalScope(new ActiveScope())
}
```
**withoutGlobalScope()**
```
User::withoutGlobalScope('acive')->get();
```
**if you want to except same global scopes**
```
User::withoutGlobalScopes()->get(); //except all global scopes
User::withoutGlobalScopes(['active','name'])->get(); //expect only active,name global scopes
```
**Accessors & Mutators** <br/>
**get , set** <br/>
**Accessors**
```
//Accessors
protected function name() : Attribute{
return Attribute::make(fn string($value) => strtoupper($value)
);
}
// in Controller
$user = User::find(1);
return $user->name;
```
```
protected function nameEmail() : Attribute
    {
        return Attribute::make(
             fn() => $this->name . '.' . $this->email
        );
    }
//in controller
$user = User::find(1);
return $user->nameEmail;
```
___________
**Mutators**
```
protected function amount() : Attribute{
return Attribute::make(
 set : fn(string($value)) => $value > 10 ? "Good Amount" : "Bad Amount" 
)
}
```
```
protected function workgroupName(): Attribute
 {
 return Attribute::make(
 set: fn (string $value) => [
        'email' => "{$value}@ourcompany.com",
     ],
    );
 }
```
____________
**Attribute casting**
```
//in Model User
 protected $casts = [
        'email_verified_at' => 'datetime',
        'password' => 'hashed',
    ];
```
___________
**make ur own cast**
```
php artisan make:cast UpperString
```
in **App/casts/UpperString**
```
public function get(Model $model, string $key, mixed $value, array $attributes): mixed
    {
        return strtoupper($value);
    }

    public function set(Model $model, string $key, mixed $value, array $attributes): mixed
    {
        return strtoupper($value);
    }
```
**in Model User**
```
protected $cast = ['amount'=> \App\Casts\UpperString::class]
```
_____________
**notifications**
```
php artisan make:notification OrderCreatedNotification
```
**In OrderCreatedNotification**
```
public function via($notifiable){
return ['mail','database','broadcast']; //refer to channel , you can define multi channels
}
```
**notifiable is an object of class , in this case (UserModel)** <br/>
**if your channel is mail, you will define method named isMail and the same for database and broadcast** <br/>
```
public function ToMail($notifiable){
}
```
**notify() VS notifyNow()**
```
$user->notify(New NotificationName);
```
**notifyNow if you put notifications in queue**
_____________
if you have multi users
```
$users = User::where('store_id',$order->store_id);
//first way
foreach($users as $user){
$user->notify(New NotificationClass);
}
// second way
Notification::send(,$users,New NotificationClass);
```
____________
**Collections**
Example 1.1
```
 $user = collect([1,2,3]);
        $collection = $user->reject(function ($item){
           return $item % 2 == 0;
        });
return $collection;
```
**output**
```
{
    "0": 1,
    "2": 3
}
```
Example 1.2
```
  $user = collect([1,2,3]);
        $collection = $user->map(function ($item){
           return $item % 2 == 0;
        });
        return $collection;
```
**output**
```
[
    false,
    true,
    false
]
```
Example 1.3
```
$user = collect([1,2,3]);
        $collection = $user->filter(function ($item){
           return $item % 2 == 0;
        });
        return $collection;
```
**output**
```
{
    "1": 2
}
```
**Note: reject X filter**
```
$user = collect([1,2,3,4]);
$collection = $user->filter(function ($item){
      return $item % 2 == 0;
      })->map(function ($item){
      return $item*10;
      })->sum();
return $collection;
```
**output**
```
60
```
**difference between collection and lazy collections**<br/>
Normal Collection **all()**
```
 $users = User::all()->filter(function($user){
            return $user->id % 2 == 0;
        });
return $users;
```
Lazy Collection **cursor()**
```
$users = User::cursor()->filter(function($user){
            return $user->id % 2 != 0;
        });
return $users;
```
_____
**To return all ids from db**
```
$users = User::all();
return $users->modelKeys();
```
___________
**custom collections** => make ur own collection in ur model 
1. **At first create collections folder in app, then create new class named OrderCollection for example**
2. in OrderCollection
 ```
use Illuminate\Database\Eloquent\Collection;

class OrderCollection extends Collection
{
   public function filterUsers()
   {
       return $this->filter(function ($user){
           return $user->id % 2 != 0;
       });
   }
}
```
3. in model
   ```
   public function newCollection(array $models = []) : Collection
  {
      return new OrderCollection($models);
  }
   ```
**don't forgot to custom Collection to method**
4. in Controller
```
$users = User::all();
return $users->filterusers();
```
(larvel docs)[https://laravel.com/docs/10.x/collections#creating-collections]
