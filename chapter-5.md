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
[https://stackoverflow.com/questions/64636625/laravel-generate-multiple-models-using-factory-hasmany-relationship](j)

**
