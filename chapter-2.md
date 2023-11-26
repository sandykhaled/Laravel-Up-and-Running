# Laravel-Up-and-Running
## chapter 2
**Notes**
1. tokenizer php extension =>It allows you to break down PHP code into individual tokens 

**functions**  
1. token_get_all(); => conevert string code into array of tokens.
2. token_name(); => return name of token.
<br\>


2. ctype php extension => check character type of string
   
**functions**
1. ctype_alnum($text) //alphanumeric [letter-digit]     
2. ctype_alpha($text) //alphabetic [letter]
3. ctype_digit($text) //digit
4. ctype_lower($text) //lower character
5. ctype_upper($text) //upper character
6. ctype_space($text) //whitespace characters (e.g., spaces, tabs, newline characters)
7. ctype_punct($text) //punctuation characters
8. ctype_graph($text) //printable characters, excluding spaces.
9. ctype_print($text) //printable characters, including spaces.
10. ctype_cntrl($text) //control characters
11. ctype_xdigit($text) //hexadecimal digits


3. json php extension

**functions**
1. json_encode($data,$options=0;$depth=512) => $options = JSON_PRETTY_PRINT
2. json_decode($data,$assocative = false,$opstions=0;$depth=512) => $options = [JSON_OBJECT_AS_ARRAY,JSON_BIGINT_AS_STRING]
3. json_last_error();
4. json_last_error_msg();

4. BCMath => Binary Calculate Math

**functions**

1. bcadd($num1,$num2,$scale)
2. bccomp($num1$num2,$scale)
2. bcmode($num2,$num2,$scale)

5. Composer is a dependency manager for PHP
   
6.composer.json VS composer.lock <br/>
Configuration files for Composer; composer.json is user-editable and com‐
poser.lock is not. These files share some basic information about the project and
also define its PHP dependencies   

7.phpunit.xml <br/>
A configuration file for PHPUnit, the tool Laravel uses for testing out of the box.

**Note**
```
// config/services.php
return [
 'bugsnag' => [
 'key' => env('BUGSNAG_API_KEY'), // Read the value from the .env file
 ],
];
```
After defining the configuration item, you can easily reference it anywhere within your application <br/>
code by using the config() function. For example, in a controller or other parts of your app,<br/>
you can access the Bugsnag API key as follows:
```
$bugsnag = new Bugsnag(config('services.bugsnag.key'));

```
Now we are in .env file <br/>
APP_KEY if it is an empty <br/>
use this command
```
 php artisan key:generate
```
APP_DEBUG <br/>
A Boolean determining whether the users of this instance of your application <br/>
should see debug errors—great for local and staging environments, terrible for <br/>
production. <br/>
by default : 
```
APP_DEBUG=true

```

**Laravel offers five tool for local development**
1. artisan serve => easy to remember.
2. Sail => use Dockor
3. Valet => for mac
4. Laravel Herd => it removes the need to work with Homebrew, Docker, or any other dependency managers
5. HomeStead => tool for managing virtual machines

**Create New Project**
```
composer create-project laravel/laravel _project
```
Or a simpler way
```
laravel new project
```

