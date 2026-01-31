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


## The 4 pillars of object oriented programming
In PHP OOP, the 4 pillars are basically the rules that make your code organized, reusable, and not a nightmare later.

The 4 pillars of OOP are:
- Encapsulation
- Abstraction
- Inheritance
- Polymorphism

### Encapsulation
**What it meanse**
Encapsulation is about wrapping data (variables) and methods together in a class and controlling access to them.
- In simple words:
- “You can’t touch my data directly. Use my methods.”

****Why it’s important
Protects data from being changed accidentally
- Makes code more secure
- Easier to debug and maintain
- 
**How PHP does this**
Using access modifiers:
- public → accessible everywhere
- private → accessible only inside the class
- protected → accessible inside class + child classes
```
class BankAccount {
    private $balance = 0;

    public function deposit($amount) {
        if ($amount > 0) {
            $this->balance += $amount;
        }
    }

    public function getBalance() {
        return $this->balance;
    }
}

$account = new BankAccount();
$account->deposit(100);

// Error: Cannot access private property
// echo $account->balance;

echo $account->getBalance(); 
// You hide the data and only allow access through methods
```
### Abstraction
**What it means**
Abstraction means showing only what is necessary and hiding the internal details.
“You don’t need to know how it works, just how to use it.”
- An abstract class is like a blueprint.
- It defines rules, but doesn’t finish the job.
**The parent class says what must be done,**
**The child class decides how to do it**
  Key points

##### An abstract class must have at least one abstract method
An abstract method:
- Has a name
- Has no body (no code inside)
- Child classes MUST implement all abstract methods
- You cannot create an object of an abstract class

```
abstract class Animal {
    abstract public function makeSound();
}
```
here:
- Animal is abstract
- makeSound() has no implementation
- Every child must define **makeSound()**
  
```
class Dog extends Animal {
    public function makeSound() {
        echo "Bark";
    }
}

class Cat extends Animal {
    public function makeSound() {
        echo "Meow";
    }
}

```
**using this**

```
$dog = new Dog();
$dog->makeSound(); // Bark

$cat = new Cat();
$cat->makeSound(); // Meow

```
Error
```
$animal = new Animal(); // Error cant call the abstract class

```

**Why abstract classes are useful**
- Force structure and rules
- Avoid missing methods in child classes
- Useful in large projects
- Helps with abstraction (one of the OOP pillars)
**An abstract class is a class that cannot be instantiated and is used to force child classes to implement abstract methods.**

###### Real-World Example: Payment System(abstraction) 
Imagine an app that supports multiple payment methods:
- Credit Card
- PayPal
- Google Pay
All payments must follow one rule:
Every payment method must have a pay() function

```
abstract class Payment {
    abstract public function pay($amount);
}
```
**This says:**
- Every payment must have pay().
- Parent doesn’t care how it’s done.
  
```
// Credit Card Payment
class CreditCard extends Payment {
    public function pay($amount) {
        echo "Paid $amount using Credit Card";
    }
}

// PayPal Payment
class Paypal extends Payment {
    public function pay($amount) {
        echo "Paid $amount using PayPal";
    }
}
// Using the System
$payments = [
    new CreditCard(),
    new Paypal()
];

foreach ($payments as $payment) {
    $payment->pay(500);
}
// Output
// Paid 500 using Credit Card
// Paid 500 using PayPal

```
**Why this is REAL-WORLD useful**
- You can add new payment methods later (Apple Pay, UPI, etc.)
- No need to change existing code
- Enforces consistency
- Clean, scalable design
Real-Life Analogy 

**Think of an ATM:**
- ATM says: “You must enter a PIN”
- It doesn’t care how the bank verifies it
- Each bank handles it differently

### interface
##### What is an Interface? (PHP OOP)?
An interface is like a pure rule book.
- It only says what methods a class must have.
- It does not say how they work.
So when a class implements an interface, it must write code for every method inside it.
**Key points (easy)**
- Interface has only method declarations
- No method body
- All methods are public
- A class can implement multiple interfaces
- Defined using the interface keyword
- Used with the implements keyword
```
interface Animal {
    public function makeSound();
}


class Dog implements Animal {
    public function makeSound() {
        echo "Bark";
    }
}
$dog = new Dog();
$dog->makeSound(); // Bark
```

**Abstract Class vs Interface (Super Simple)**
🔹 Abstract Class
- Can have rules + some working code
- Can have properties (variables)
- A class can extend only ONE abstract class
- Used when classes are closely related

🔹 Interface
- Has only rules, no working code
- No properties
- A class can implement MULTIPLE interfaces
- Used when classes are not related, but must follow the same rules.

### PHP Polymorphism
What is polymorphism?
Polymorphism is a Greek word that literally means many forms. In object-oriented programming, polymorphism is closely related to inheritance.
Polymorphism allows objects of different classes to respond differently based on the same message.
Polymorphism helps you create a generic framework that takes the different object types that share the same interface.

**PHP polymorphism using an abstract class **

First, define an abstract class named Person that has an abstract method called greet().
```
<?php

abstract class Person
{
	abstract public function greet();
}

```

Second, define the English, German, and French classes that extend the Person class. The greet() method of each class returns a different greeting message.
```
<?php

class English extends Person
{
	public function greet()
	{
		return 'Hello!';
	}
}

class German extends Person
{
	public function greet()
	{
		return 'Hallo!';
	}
}

class French extends Person
{
	public function greet()
	{
		return 'Bonjour!';
	}
}

```
Third, define a function called greeting() that accepts an array of Person objects and calls the greet() method of each object:

```
<?php
function greeting($people)
{
	foreach ($people as $person) {
		echo $person->greet() . '<br>';
	}
}
$people = [
	new English(),
	new German(),
	new French()
];

greeting($people);
```




## What is Inheritance? (Simple Definition)

Inheritance means:
A child class gets properties and methods from a parent class.
- Child reuses parent code
- Child can add new features
- A child can change a parent's behavior

**Real-Life Example (Very Easy)**

**Think of a Family**
- Parent has: surname, house, habits
- Child automatically gets them
- A child can also have their own things
- That’s inheritance.
  
```
class ParentClass {
    // parent code
}

class ChildClass extends ParentClass {
    // child code
}

```


```
class Animal {
    public function eat() {
        echo "Animal is eating<br>";
    }
}
class Dog extends Animal {
    public function bark() {
        echo "Dog is barking<br>";
    }
}
//Dog inherits eat()

// Dog has its own method bark()

$dog = new Dog();
$dog->eat();   // inherited
$dog->bark();  // own method
```


**Method Overriding**
```
class Animal {
    public function sound() {
        echo "Some sound<br>";
    }
}

class Cat extends Animal {
    public function sound() {
        echo "Meow<br>";
    }
}

$cat = new Cat();
$cat->sound(); // Meow

```

Inheritance allows a class to acquire properties and methods of another class, promoting code reuse and hierarchy.


**Types of Inheritance (Conceptually)**

In OOP, inheritance is commonly explained in 5 types:
- Single Inheritance
- Multilevel Inheritance
- Hierarchical Inheritance
- Multiple Inheritance
- Hybrid Inheritance



**Note: Important PHP note:**
- PHP supports Single, Multilevel, Hierarchical
- PHP does NOT support multiple inheritance with classes
- PHP uses interfaces & traits to handle that

  

**Single Inheritance**
- One child inherits from one parent.
- Dog → Animal

``` 
class Animal {
    public function eat() {
        echo "Eating<br>";
    }
}

class Dog extends Animal {
    public function bark() {
        echo "Bark<br>";
    }
}

// Dog inherits Animal
// One parent → one child
```
**Multilevel Inheritance**
- A class inherits from a class that already inherited another class.
- Animal → Mammal → Dog

```
class Animal {
    public function eat() {
        echo "Eating<br>";
    }
}

class Mammal extends Animal {
    public function walk() {
        echo "Walking<br>";
    }
}

class Dog extends Mammal {
    public function bark() {
        echo "Bark<br>";
    }
}
// Chain inheritance
// Grandparent → parent → child

```

**Hierarchical Inheritance**
- Multiple child classes inherit from one parent.
- Animal → Dog
- Animal → Cat
- Animal → Bird

```
class Animal {
    public function eat() {
        echo "Eating<br>";
    }
}

class Dog extends Animal {
    public function bark() {
        echo "Bark<br>";
    }
}

class Cat extends Animal {
    public function meow() {
        echo "Meow<br>";
    }
}

```
**Multiple Inheritance**
- One child inherits from multiple parents.

```
interface A {
    public function methodA();
}

interface B {
    public function methodB();
}


class C implements A, B {
    public function methodA() {}
    public function methodB() {}
}

```

**Hybrid Inheritance**
- Combination of more than one inheritance type.
- Hierarchical + Multilevel






