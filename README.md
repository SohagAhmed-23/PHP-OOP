# PHP-OOP


## What is OOP?
Object-Oriented programming is faster and easier to execute.

Procedural programming is about writing procedures or functions that perform operations on the data, 
while object-oriented programming is about creating objects that contain both data and functions.
Object-oriented programming has several advantages over procedural programming:

- **Better structure:** OOP organizes code into classes and objects, which makes big programs way easier to understand.
- **DRY code:** You can reuse classes instead of rewriting the same logic over and over.
- **Easier maintenance:** Changes usually happen in one place, not everywhere.
- **Scalability:** OOP is great for large, complex, or long-term projects.
- **Reusability:** Classes can be reused across different projects, saving time and effort.
###### Note:
OOP is not automatically faster in execution. Sometimes procedural code can actually run faster. The real advantage of OOP is **readability**, **organization**, and **maintainability**, not raw speed.
## What are Classes and Objects?
Classes and objects are the two main aspects of object-oriented programming.
### Class
A class is like a blueprint or template.
It describes what something is and what it can do, but it’s not a real thing yet.

Think of it as the idea of something. :)

### What is an Object?
An object is the real thing made from the class.
You can create many objects from the same class, and each one can have different details.

### Real-life Example: Car
#### Class: Car
- Describes what every car has (brand, color, speed)
- Describes what every car can do (drive, stop)

#### Object:
- A black Toyota
- A red Ferrari

```
class Car {
// Properties: Variables inside a class (e.g., $brand, $color).
    public $brand;
    public $color;

// Methods: Functions inside a class (e.g., drive()).
    public function drive() {
        echo "Driving a $this->color $this->brand";
    }
}
// $this refers to the current object

$car1 = new Car(); // make a object
$car1->drive(); // for that object call drive
```


$this operator: $this, as the '$' sign suggest, is an object. 
$this represents the current object of a class. It is used to access non-static members of a class(public, private, protected, etc., except the static member)



### The __construct() Function


Think of __construct() like a starter kit for your object.
Whenever you make a new object, PHP automatically runs __construct() to set it up. You don’t have to call anything extra.

```

<?php
class Fruit {
    public $name;
    public $color;

    // This runs automatically when we make a new Fruit
    public function __construct($name, $color) {
        $this->name = $name;
        $this->color = $color;
    }
}

// Make a new Fruit – constructor runs automatically
$banana = new Fruit("Banana", "Yellow");

echo "Fruit: $banana->name, Color: $banana->color";
?>



```


### PHP - The __destruct() Function

The PHP __destruct() function is a special method within a class that is automatically called when an object is destroyed or when the script finishes execution.

The PHP __destruct() function is the opposite of the PHP __construct() function.

If you create a __destruct() function, PHP will automatically call this function at the end of the script.

`Notice that the __destruct() function starts with two underscores (__)!
`


##### Real situation:

Your app connects to a database.
If you don’t close it → server gets overloaded.




```

<?php
class Database {
    private $conn;

    public function __construct() {
        $this->conn = "DB Connected";
        echo "Database opened \n";
    }

    public function __destruct() {
        $this->conn = null; // close connection
        echo "Database closed \n";
    }
}

$db = new Database();
echo "Running queries...\n";

for($i= 0;$i<=5;$i++) {
    echo $i;
    echo PHP_EOL;
}
?>
// output be like 
Database opened // first construction will call 
Running queries... // Then just run the whole scrpit 
0
1
2
3
4
5
Database closed // After the script is end it will called the destruction. Even if you forget to close the DB manually, PHP does it for you at the end.
````


### Why destructor helps:
Prevents:
- file corruption
- memory leaks
- locked files






### PHP - Access Modifiers

Properties and methods can have access modifiers (or visibility keywords) that control where they can be accessed.

- **public** - the property or method can be accessed from everywhere. This is default
- **protected**- the property or method can be accessed within the class and by classes derived from that class.
- **private**- the property or method can ONLY be accessed within the same class where they are defined.



**Note: If no acces modifier is specified, it will be set to public.**


### Public Access Modifier

- The public access modifier allows class properties or methods to be accessed from everywhere.
- In the following example, the $conn property and the get_details() method are accessible from outside the class.



```
<?php
class Database {
    public $conn;
    public $host;

    public function __construct() {
        $this->conn = "DB Connected\n";
        echo "Database opened \n";
    }
   public function getdetails() {
     $this->host = '127.0.0.1:8080';
     echo "Database Host is : " . $this->host . "\n";
    }

    public function __destruct() {
        $this->conn = null; // close connection
        echo "Database closed \n";
    }
}

$db = new Database();
echo $db->conn;
$db->getdetails();

echo "Running queries...\n";

// output

Database opened 
DB Connected
Database Host is : 127.0.0.1:8080 // public function, but we can access outsite class.
Running queries...
Database closed 

```

### Private Access Modifier
The private access modifier allows class properties or methods ONLY to be accessed within the same class where they are defined.
In the following example, the $conn property is private and cannot be accessed directly.

```
<?php
class Database {
    private $conn;
    private $host;

    public function __construct() {
        $this->conn = "DB Connected\n";
        echo "Database opened \n";
    }
   public function getdetails() {
     $this->host = '127.0.0.1:8080';
     echo "Database Host is : " . $this->host . "\n";
    }

    public function __destruct() {
        $this->conn = null; // close connection
        echo "Database closed \n";
    }
}

$db = new Database();
echo $db->conn; // got an error **caught Error: Cannot access private property Database::$conn **
$db->getdetails();

echo "Running queries...\n";



```


 ### **protected means:**

- Accessible inside the same class
- Accessible in child (derived) classes
- NOT accessible outside the class
```
<?php
class Database {
    protected $host;

    public function __construct() {
        $this->host = "127.0.0.1:8080";
        echo "Database created\n";
    }
}

class MySQL extends Database {
    public function showHost() {
        echo "Database Host: " . $this->host . "\n";
    }
}

$db = new MySQL();
$db->showHost();      //  allowed (child class accessing protected property)
echo $db->host;       // ERROR: Cannot access protected property
?>

```
### Static Methods
- Static methods and properties in PHP belong to the class rather than an instance of the class.
- This means they can be accessed without creating an object of the class.
- A static method is declared with the static keyword and can be invoked directly using the class name followed by the scope resolution operator.
- Similarly, a static property is also defined with the static keyword, but cannot be accessed directly, even from within the class methods - they must be accessed through static methods.
```
class MyClass {
    static $myStaticProperty = "Hello, world";

    static function myStaticMethod() { 
        return self::$myStaticProperty; 
    }
}

echo MyClass::myStaticMethod();
```

A class can have both static and non-static methods. 
A static method can be accessed from a method in the same class using the self keyword and double colon (::):
```
<?php
class greeting {
  public static function welcome() {
    echo "Hello World!";
  }

  public function __construct() {
    self::welcome();
  }
}

new greeting();
?>

```



