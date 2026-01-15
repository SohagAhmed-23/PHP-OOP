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
