# [link](https://www.w3schools.com/php/default.asp)
## Redirect 
```php
header("Location: user_panel.php");
```
 

# Set Cookie
```php
setcooike("is_log", "true", time() +  (7 * 24 * 60 * 60), "/")
```

# Connecting MySql in PHP

```php
$db_servername = "localhost"; // Replace with your MySQL server hostname or IP address
$db_username = "rebel"; // Replace with your MySQL username
$db_password = "1234"; // Replace with your MySQL password
$db_database = "test"; // Replace with the name of your MySQL database
$backupPath = "/var/www/html/admin/backups/";

// Create a connection
$conn = new mysqli($db_servername, $db_username, $db_password, $db_database);
```

# include 
```php
include 'db.php';
```