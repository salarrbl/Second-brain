## Install php
```shell
apt install php
pacman -S php
```
- if install should run `php -v`

## Basic Syntax
```php
<?php
// PHP code goes here
?>
```
- The default file extension for PHP files is "`.php`".
- create file `test.php`
```php
<!DOCTYPE html>
<html>
<body>

<h1>My first PHP page</h1>

<?php
echo "Hello World!";
?>

</body>
</html>
```
- <span style="color:rgb(192, 0, 0)">I</span><span style="color:rgb(192, 0, 0)">n PHP, keywords (e.g. `if`, `else`, `while`, `echo`, etc.), classes, functions, and user-defined functions are not case-sensitive.</span>
- ### variable
- In PHP, a variable starts with the <span style="color:rgb(192, 0, 0)">`$`</span> sign, followed by the name of the variable:
- 
```php
<?php
$x = 5;
$y = "John";
$color = "red";
echo "My boat is " . $coLOR . $y;
?>
```
- #### Output Variables
```php
$txt = "salar";
echo "I love $txt!";
//-----------
$txt = "salar";
echo "I love . $txt. "!";
$x = 5;
$y = 4;
echo $x + $y;
```
### Variable Types
- To get the data type of a variable, use the `var_dump()` function.
```php
$salar = "sa";
var_dump($salar);
var_dump(5);
var_dump("John");
var_dump(3.14);
var_dump(true);
var_dump([2, 3, 56]);
var_dump(NULL);
```
#### Assign Multiple Values
- You can assign the same value to multiple variables in one line:
```php
$salar = $x = $y = "salar";
```
- #### Comment
```php
// This is a single-line comment

# This is also a single-line comment

/* This is a
multi-line comment */
```
## PHP echo and print Statements
- They are both used to output data to the screen.
- #### The PHP echo Statement
- can not use () in `echo`
- can contain html tags
```php
echo "Hello";
//same as:
echo("Hello");
//--------------
echo "<h2>PHP is Fun!</h2>";
echo "Hello world!<br>";
echo "I'm about to learn PHP!<br>";
echo "This ", "string ", "was ", "made ", "with multiple parameters.";
```
- ##### Display Variables
```php
$txt1 = "Learn PHP";
$txt2 = "W3Schools.com";

echo "<h2>$txt1</h2>";
echo "<p>Study PHP at $txt2</p>";
```
- #### print 
- can not use () in `print` like `echo`
```php
print "Hello";
//same as:
print("Hello");
//--------------
print "<h2>PHP is Fun!</h2>";
print "Hello world!<br>";
print "I'm about to learn PHP!<br>";
print "This ", "string ", "was ", "made ", "with multiple parameters.";
$txt1 = "Learn PHP";
$txt2 = "W3Schools.com";

print "<h2>$txt1</h2>";
print "<p>Study PHP at $txt2</p>";
```
## PHP Data Types
- php supports these data types:
- String
- Integer
- Float (floating point numbers - also called double)
- Boolean
- Array
- Object
- NULL
- Resource
- ##### PHP Array
```php
$cars = array("Volvo","BMW","Toyota");
var_dump($cars);
```
- ##### PHP Object

- Classes and objects are the two main aspects of object-oriented programming.
- A class is a template for objects, and an object is an instance of a class.
- When the individual objects are created, they inherit all the properties and behaviors from the class, but each object will have different values for the properties.
- Let's assume we have a class named `Car` that can have properties like model, color, etc. We can define variables like `$model`, `$color`, and so on, to hold the values of these properties.
- When the individual objects (Volvo, BMW, Toyota, etc.) are created, they inherit all the properties and behaviors from the class, but each object will have different values for the properties.
- If you create a `__construct()` function, PHP will automatically call this function when you create an object from a class.

```php
class Car {
  public $color;
  public $model;
  public function __construct($color, $model) {
    $this->color = $color;
    $this->model = $model;
  }
  public function message() {
    return "My car is a " . $this->color . " " . $this->model . "!";
  }
}

$myCar = new Car("red", "Volvo");
var_dump($myCar);
```
## String
-  String Length  `strlen()`
```php
echo strlen("Hello world!");
```
-  Word Count --> str_word_count()
```php
echo str_word_count("Hello world!");
```
-  Search For Text Within a String   `strpos()`
```php
echo strpos("Hello world!", "world");
```
-  Upper Case   `stroupper()`
```php
$x = "Hello World!";
echo strtoupper($x);
```
-  Lower Case   `strtolower()`
```php
$x = "Hello World!";
echo strtolower($x);
```
-  Replace String   `str_replace()`
```php
$x = "Hello World!";
echo str_replace("World", "Dolly", $x);
```
-  Reverse a String    `strrev()`
```php
$x = "Hello World!";
echo strrev($x);
```
-  Remove Whitespace `trim()`
```php
$x = " Hello World! ";
echo trim($x);
```
-  Convert String into Array
- The first parameter of the `explode()` function represents the "separator". The "separator" specifies where to split the string.
```php
$x = "Hello World!";
$y = explode(" ", $x);
//Use the print_r() function to display the result:
print_r($y);
echo "$y[1]";   // World!
/*
Result:
Array ( [0] => Hello [1] => World! )
*/
```
- ## Slicing
```php
$x = "Hello World!";
echo substr($x, 6, 5);
``` 
- Slice to the End
```php
$x = "Hello World!";
echo substr($x, 6);
```
## php number
- `is_int()`
- `is_float()`
- etc
## Change Data Type
Casting in PHP is done with these statements:
- `(string)` - Converts to data type String
- `(int)` - Converts to data type Integer
- `(float)` - Converts to data type Float
- `(bool)` - Converts to data type Boolean
- `(array)` - Converts to data type Array
- `(object)` - Converts to data type Object
- `(unset)` - Converts to data type NULL
```php
$c = "25 kilometers"; // String
$c = (string) $c;
$c = (int) $c;
$c = (float) $c;
$c = (bool) $c;
$c = (array) $c;
$c = (object) $c;
$c = (unset) $c;
```
## PHP Constants
- `define()`
```php
define(name, value);
```
- `cast()`
```php
const MYCAR = "Volvo";
```
## php [operators](https://www.w3schools.com/php/php_operators.asp)

# PHP if Statements
- ## PHP Conditional Statements
- ### Syntax
```php
if (condition) {
  // code to be executed if condition is true;
}
```
- example
```php
$t = 14;

if ($t < 20) {
  echo "Have a good day!";
}
```
```php
$a = 5;

if ($a == 2 || $a == 3 || $a == 4 || $a == 5 || $a == 6 || $a == 7) {
  echo "$a is a number between 2 and 7";
}
```
- ## else
	- #### Syntax
```php
if (condition) {
  // code to be executed if condition is true;
} else {
  // code to be executed if condition is false;
}
```

```php
$t = date("H");

if ($t < "20") {
  echo "Have a good day!";
} else {
  echo "Have a good night!";
}
```
- ## if...elseif...else
	- #### syntax
```php
if (condition) {
  code to be executed if this condition is true;
} elseif (condition) {
  // code to be executed if first condition is false and this condition is true;
} else {
  // code to be executed if all conditions are false;
}
```
## PHP switch Statement
- ### Syntax
```php
switch (expression) {
  case label1:
    //code block
    break;
  case label2:
    //code block;
    break;
  case label3:
    //code block
    break;
  default:
    //code block
}
```
- example
```php
$favcolor = "red";

switch ($favcolor) {
  case "red":
    echo "Your favorite color is red!";
    break;
  case "blue":
    echo "Your favorite color is blue!";
    break;
  case "green":
    echo "Your favorite color is green!";
    break;
  default:
    echo "Your favorite color is neither red, blue, nor green!";
}
```
- #### Common Code Blocks
```php
$d = 3;

switch ($d) {
  case 1:
  case 2:
  case 3:
  case 4:
  case 5:  
    echo "The weeks feels so long!";
    break;
  case 6:
  case 0:
    echo "Weekends are the best!";
    break;
  default:
    echo "Something went wrong";
}
```
# PHP while Loop
- `while`
```php
$i = 1;
while ($i < 6) {
  echo $i;
  $i++;
}
```
- `break`
```php
$i = 1;
while ($i < 6) {
  if ($i == 3) break;
  echo $i;
  $i++;
}
```
- `continue`
```php
$i = 0;
while ($i < 6) {
  $i++;
  if ($i == 3) continue;
  echo $i;
}
```
# PHP [do while](https://www.w3schools.com/php/php_looping_do_while.asp)Loop
# for
- ### Syntax
```php
for (expression1, expression2, expression3) {
  // code block
}
```
- example
```php
for ($x = 0; $x <= 10; $x++) {
  if ($x == 3) continue;
  if ($x == 9) break;
  echo "The number is: $x <br>";
}
```

## foreach
- The `foreach` loop - Loops through a block of code for each element in an array or each property in an object.
```php
$colors = array("red", "green", "blue", "yellow");

foreach ($colors as $x) {
  if ($x == "red")  break;
  if ($x == "blue") continue;
  echo "$x <br>";
}
foreach ($color as $x )
```
- key and value
```php
$members = array("Peter"=>"35", "Ben"=>"37", "Joe"=>"43");

foreach ($members as $x => $y) {
  echo "$x : $y <br>";
}
```
- #### object
```php
class Car {
  public $color;
  public $model;
  public function __construct($color, $model) {
    $this->color = $color;
    $this->model = $model;
  }
}

$myCar = new Car("red", "Volvo");

foreach ($myCar as $x => $y) {
  echo "$x: $y <br>";
}
```
- change one item array
```php
$colors = array("red", "green", "blue", "yellow");

foreach ($colors as $x) {
  if ($x == "blue") $x = "pink";
}

var_dump($colors);
```
## Function
```php
function myMessage($fname, $year, $city = "tabriz") {
  echo "Hello world!";
  echo "$fname Refsnes.<br>";
}
myMessage("salar", "2006");
myMessage("salar", "2006");   // will use the default value of 50
```

# php [Array's](https://www.w3schools.com/php/php_arrays.asp) 

# PHP Global Variables - Superglobals
- [read](https://www.w3schools.com/php/php_superglobals.asp)
- `[$GLOBALS]
- `[$_SERVER]
- `[$_REQUEST]
- `[$_POST]
- `[$_GET]
- `$_FILES`
- `$_ENV`
- `[$_COOKIE]
- `[$_SESSION]
- ### $GLOBALS
- `$GLOBALS` is an array that contains all global variables.
- accessed from any scope.

### $_SERVER
```php
echo $_SERVER['PHP_SELF'];
echo $_SERVER['SERVER_NAME'];
echo $_SERVER['HTTP_HOST'];
echo $_SERVER['HTTP_REFERER'];
echo $_SERVER['HTTP_USER_AGENT'];
echo $_SERVER['SCRIPT_NAME'];
```
The following table lists the most important elements that can go inside `$_SERVER`:

| Element/Code                    | Description                                                                                                                                                                                                            |
| ------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| $_SERVER['PHP_SELF']            | Returns the filename of the currently executing script                                                                                                                                                                 |
| $_SERVER['GATEWAY_INTERFACE']   | Returns the version of the Common Gateway Interface (CGI) the server is using                                                                                                                                          |
| $_SERVER['SERVER_ADDR']         | Returns the IP address of the host server                                                                                                                                                                              |
| $_SERVER['SERVER_NAME']         | Returns the name of the host server (such as www.w3schools.com)                                                                                                                                                        |
| $_SERVER['SERVER_SOFTWARE']     | Returns the server identification string (such as Apache/2.2.24)                                                                                                                                                       |
| $_SERVER['SERVER_PROTOCOL']     | Returns the name and revision of the information protocol (such as HTTP/1.1)                                                                                                                                           |
| $_SERVER['REQUEST_METHOD']      | Returns the request method used to access the page (such as POST)                                                                                                                                                      |
| $_SERVER['REQUEST_TIME']        | Returns the timestamp of the start of the request (such as 1377687496)                                                                                                                                                 |
| $_SERVER['QUERY_STRING']        | Returns the query string if the page is accessed via a query string                                                                                                                                                    |
| $_SERVER['HTTP_ACCEPT']         | Returns the Accept header from the current request                                                                                                                                                                     |
| $_SERVER['HTTP_ACCEPT_CHARSET'] | Returns the Accept_Charset header from the current request (such as utf-8,ISO-8859-1)                                                                                                                                  |
| $_SERVER['HTTP_HOST']           | Returns the Host header from the current request                                                                                                                                                                       |
| $_SERVER['HTTP_REFERER']        | Returns the complete URL of the current page (not reliable because not all user-agents support it)                                                                                                                     |
| $_SERVER['HTTPS']               | Is the script queried through a secure HTTP protocol                                                                                                                                                                   |
| $_SERVER['REMOTE_ADDR']         | Returns the IP address from where the user is viewing the current page                                                                                                                                                 |
| $_SERVER['REMOTE_HOST']         | Returns the Host name from where the user is viewing the current page                                                                                                                                                  |
| $_SERVER['REMOTE_PORT']         | Returns the port being used on the user's machine to communicate with the web server                                                                                                                                   |
| $_SERVER['SCRIPT_FILENAME']     | Returns the absolute pathname of the currently executing script                                                                                                                                                        |
| $_SERVER['SERVER_ADMIN']        | Returns the value given to the SERVER_ADMIN directive in the web server configuration file (if your script runs on a virtual host, it will be the value defined for that virtual host) (such as someone@w3schools.com) |
| $_SERVER['SERVER_PORT']         | Returns the port on the server machine being used by the web server for communication (such as 80)                                                                                                                     |
| $_SERVER['SERVER_SIGNATURE']    | Returns the server version and virtual host name which are added to server-generated pages                                                                                                                             |
| $_SERVER['PATH_TRANSLATED']     | Returns the file system based path to the current script                                                                                                                                                               |
| $_SERVER['SCRIPT_NAME']         | Returns the path of the current script                                                                                                                                                                                 |
| $_SERVER['SCRIPT_URI']          | Returns the URI of the current page                                                                                                                                                                                    |
# $_REQUEST
```php
<html>
<body>

<form method="post" action="<?php echo $_SERVER['PHP_SELF'];?>">
  Name: <input type="text" name="fname">
  <input type="submit">
</form>

<?php
if ($_SERVER["REQUEST_METHOD"] == "POST") {
  $name = htmlspecialchars($_REQUEST['fname']);
  if (empty($name)) {
    echo "Name is empty";
  } else {
    echo $name;
  }
}
?>

</body>
</html>
```

# $_POST
## $_POST in JavaScript HTTP Requests
