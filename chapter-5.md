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
 At first create **new migration** as rename_name_column
``` public function up()
    {
        Schema::table('posts', function (Blueprint $table) {
            $table->renameColumn('name', 'to'); // Correct usage to rename 'name' to 'to'
        });
    }

    public function down()
    {
        Schema::table('posts', function (Blueprint $table) {
            $table->renameColumn('to', 'name'); // Reverse the column name change
        });
    }

```    
