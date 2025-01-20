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
