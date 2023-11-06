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
**Boolean or Tinyint** <br/>

For two genders (male and female) with no future additions, use **tinyint(1)** or **Boolean()** with 0 for female and 1 for male. When expansion is needed, modify the schema to an **ENUM**.
