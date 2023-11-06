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
1. **integer(ColName) , tinyInteger(ColName) , smaillInteger(ColName) , mediumInteger , bigInteger** <br/>
2. **Enum(ColName,[choice1,choice2,..])**
```
Schema::create('my_table', function (Blueprint $table) {
            $table->enum('status', ['Active', 'Inactive', 'Pending']); // Example ENUM with choices
        });
```
3. **Boolean or Tinyint** <br/>

      For two genders (male and female) with no future additions, use **tinyint(1)** or **Boolean()** with 0 for female and 1 for male. When expansion is 
      needed, modify the schema to an **ENUM**.

3. **string(colName, optional length)**
4. **char(colName,optional length)**
5. **binary(colName)**

     This column will store binary data, such as **images**, **files**, or any **binary data** you need to store in your database.

6. **decimal(colName,precision,scale)** <br/>
   ```
   decimal("rate",5,2)
   ``` 
   
    **precision** specifies the total number of digits a DECIMAL column can hold. It controls the maximum digits, including those after the decimal 
       point. For example, it allows values like **123.45** or **12.345**.
    **scale** specifies the number of digits to the right of the decimal point. For example, it allows values like **123.45**, not 12.345.   

7. **double(colName, total digits, digits after decimal)**
8. **datetime(colName)**
9. **float(colName, precision, scale)**
