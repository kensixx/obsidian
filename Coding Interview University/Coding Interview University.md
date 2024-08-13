# SOLID
Resources:

| Link                                                                                                         | Description                                  |
| ------------------------------------------------------------------------------------------------------------ | -------------------------------------------- |
| https://www.oodesign.com/                                                                                    | OODesign.com                                 |
| https://github.com/jwasham/coding-interview-university?tab=readme-ov-file#additional-detail-on-some-subjects | Coding Interview University -> SOLID Section |
1. SOLID 
2. Design Patterns, 1 example for each category 
3. OOP Pillars
	- Abstraction
	- Encapsulation
	- Inheritance
	- Polymorphism
1. Best practices pag create restful api
2. Interface vs Abstract
3. concurrency
4. Stream
# Single Responsibility Principle
**There should never be more than one reason for a class to change.**
## Example
User Management in a Web Applications
![](attachments/Pasted%20image%2020240723102119.png)

Applying SRP, these functions should be separated into different classes:

- A **User** class solely for storing and managing user details.
- A **UserValidator** class for validating user data.
- A **UserReportGenerator** class for creating user activity reports.
- An **Authenticator** class for handling authentication.
- An **Authorizer** class for managing user permissions.

# Open-Closed Principle
Software entities like classes, modules and functions should be **open for extension** but **closed for modifications**.

The **Open Close Principle** states that the design and writing of the code should be done in a way that new functionality should be added with minimum changes in the existing code. 

The design should be done in a way to allow the adding of new functionality as new classes, keeping as much as possible existing code unchanged.
## Bad Example
Suppose you have a **ShapeDrawer** or **GraphicEditor** that draws shapes.

The following is a bad example:

![](attachments/Pasted%20image%2020240723185910.png)

```java
// Open-Close Principle - Bad example
 class GraphicEditor {
 
  public void drawShape(Shape s) {
    if (s.m_type==1)
      drawRectangle(s);
    else if (s.m_type==2)
      drawCircle(s);
  }

  public void drawCircle(Circle r) {
    ....
  }

  public void drawRectangle(Rectangle r) {
    ....
  }
}
 
class Shape {
  int m_type;
}
 
class Rectangle extends Shape {
  Rectangle() {
    super.m_type=1;
  }
}
 
class Circle extends Shape {
  Circle() {
    super.m_type=2;
  }
}
```

This is bad example because you always have to update `GraphicEditor` -> `drawShape()` method every time there is a new Shape.
## Good Example
![](attachments/Pasted%20image%2020240723190346.png)

```java
// Open-Close Principle - Good example
class GraphicEditor {
  public void drawShape(Shape s) {
    s.draw();
  }
}

class Shape {
  abstract void draw();
}

class Rectangle extends Shape  {
  public void draw() {
    // draw the rectangle
  }
}   
```
This is good example because: 
- The `drawShape()` method accepts a class that extends to `Shape`
- We make sure that all `Shapes` has to implement a method called `draw()` on it
- You just have to add a new class that `extends` to `Shape` 
- The `GraphicEditor` -> `drawShape()` method doesn't need to update every time there is a new `Shape`.
- So the one that has to adjust is the new class only, it has to implement the `draw()` method. 

# Liskov's Substitution Principle(LSP)
Description: 
- A child class should be able to be used in place of its parent class.
- If we substitute a parent class object reference with an object of *any* of its child classes, **the program should not break**.

Say we had a method that used a superclass object reference to do something:
```java
class ChildClass {
  
  void aMethod(ParentClass parentClass) {
    doSomething(parentClass);
  }
  
  // definition of doSomething() omitted
}
```

This should work as expected for _every possible subclass object of `ParentClass` that is passed to it_. 

If substituting a parent class object with a child class object changes the program behavior in unexpected ways, **the LSP is violated**.
## Example
![](attachments/Pasted%20image%2020240725210806.png)

# Interface Segregation Principle (ISP)
The **Interface Segregation Principle** states that clients should not be forced to implement interfaces they don't use.

Instead of one fat interface, many small interfaces are preferred based on groups of methods, each one serving one submodule.
## Example
![](attachments/Pasted%20image%2020240723192708.png)
# Dependency Inversion Principle (DIP)
Description: 
- High-level modules (handling complex logic) should not depend on low-level modules (performing basic operations). 
- Both should rely on abstractions.

## Principle - Visual
![](attachments/Pasted%20image%2020240725200942.png)
## Bad Example
```java
// Dependency Inversion Principle - Bad example
class Worker {
  public void work() {
    // ....working
  }
}

class Manager {
  Worker worker;

  public void setWorker(Worker w) {
    worker = w;
  }

  public void manage() {
    worker.work();
  }
}

class SuperWorker {
  public void work() {
    //.... working much more
  }
}
```

- We have the `Manager` class which is a high level class, and the low level class called `Worker`.
- `Manager` is using (and dependent) on the `Worker` class.
- When `Manager` is used using the `manage()` method, it uses the `Worker` to use the `work()` method.
- All is well and good until a new class is introduced called `SuperWorker`. The new requirement is that this `SuperWorker` should be able to use by the `Manager`.
- But since `Manager` only knows how to use `Worker`, we now have an issue.
### Issues
- We have to change the `Manager` class (remember it is a complex one and this will involve time and effort to make the changes).
- Some of the current functionality from the `Manager` class might be affected.
- The unit testing should be redone.
## Good Example
```java
// Dependency Inversion Principle - Good example
interface IWorker {
  public void work();
}

class Worker implements IWorker{
  public void work() {
    // ....working
  }
}

class SuperWorker  implements IWorker{
  public void work() {
    //.... working much more
  }
}

class Manager {
  IWorker worker;

  public void setWorker(IWorker w) {
    worker = w;
  }

  public void manage() {
    worker.work();
  }
}
```

- In this new design, **a new abstraction layer is added** through the `IWorker` Interface. 
- Now the problems from the above code are solved.
- `Manager` class doesn't require changes when adding `SuperWorkers`.
- Minimized risk to affect old functionality present in `Manager` class since we don't change it.
- No need to redo the unit testing for `Manager` class.
## Types of Dependency Injection 
### Constructor Injection
We can inject the depedency to the main component direcly in the constructor when the component is instantiated. 

In this case the dependency should be already instantiated when the constructor of the main component is invoked.

```java
public class UserService {
  private final Database database;

  public UserService(Database database) {
    this.database = database;
  }

  public User getUser(int id) {
    return database.query("SELECT * FROM users WHERE id = " + id);
  }
}
```

### Method Injection
Method injection is similar to Constructor Injection, but we inject the dependencies in methods. This concept is less likely used in practice, but we might have to use it in certain situations.

```java
public class UserController {
  public User getUser(int id, UserService userService) {
    return userService.getUser(id);
  }

  public static void main(String[] args) {

  UserService userService = new UserService();

  UserController userController = new UserController();

  User user = userController.getUser(123, userService);
    // Do something with the user...
  }
}
```

### Property Injection
This type of dependency injection consists in passing the dependency through a property. 

It allows a lazy loading initialization of the dependency, in case the dependency is not instantiated when main component is instantiated.

```java
public class OrderService {
  // Dependencies assigned directly
  private ProductService productService;

  public OrderService() {
    // ...
  }

  public void setProductService(ProductService productService) {
    this.productService = productService;
  }

  // ...
}
```
# Design Patterns
## Creational Patterns

### Singleton
Ensure that only one instance of a class is created and Provide a global access point to the object.

## Behavioral Patterns

# RESTful API - Best Practices

## Use noun instead of verbs

For example:
```
// Don't do this
- GET /get_customers
- POST /insert_customers
- PUT /modify_customers
- DELETE /delete_customers

// Do this
+ GET /customers
+ POST /customers
+ PUT /customers
+ DELETE /customers
```

## Endpoints name should be plural

For example:

```
// Don't do this
- GET /article
- GET /article/:id

// Do this
+ GET /articles
+ GET /articles/:id
```

## Support for Filter, Sort, and Pagination

For example:
- **Filter:** filter the customer with the following properties… the last name is Smith and the age is 30.  
    `GET /customers?last_name=Smith&age=30`
- **Pagination:** Return 20 rows starting from 0  
    `GET /customers?limit=20&offset=0`
- **Sort:** Return rows sorted by email in the ascendant.  
    `GET /customers?sort_by=asc(email)`

## Versioning
Keeping the different versions of your API will help you to track the changes and it will help you to restore the previous version in case if something goes wrong with the latest one.

For example:
```
GET /v1/customers
GET /v2/students
```
# Interface vs Abstract

## Interface
- they are "contracts"
- they will be overridden interface implementing classes, aka "impl classes"
- in **Java 8**, there is now an ability to **support static and default methods** in interfaces.
- Interfaces in **Java 9+** can have _private_ methods, which can be used to split lengthy default methods.

### When to Use an Interface
- Consider using the interface when our problem makes the statement **“A is capable of [doing this]”**
	- For example, “Clonable is capable of cloning an object”, “Drawable is capable of drawing a shape”, etc.
- When application functionalities have to be defined **as a contract**, 
	- but not concerned about who implements the behavior. i.e., third-party vendors need to implement it fully

### Example
```java
public interface Sender { 
	void send(File fileToBeSent); 
}
```

```java
public class ImageSender implements Sender {
    @Override
    public void send(File fileToBeSent) {
        // image sending implementation code.
    }
}
```

- Here, _Sender_ is an interface with a method _send()_. 
- Hence, **“Sender is capable of sending a file”** we implemented it as an interface. 
- _ImageSender_ implements the interface for sending an image to the target. 
- We can further use the above interface to implement _VideoSender_, _DocumentSender_ to accomplish various jobs.
## Abstract

### When to Use an Abstract
- **Consider using abstract classes and inheritance when our problem makes the evidence “A is a B”.** 
	- For example, “Dog is an Animal”, “Lamborghini is a Car”, etc.
- Trying to use the inheritance concept in code (**share code among many related classes**), by providing **common base class** methods that the subclasses override

### Example
```java
public abstract class Vehicle {
    protected abstract void start();
    protected abstract void stop();
    protected abstract void drive();
    protected abstract void changeGear();
    protected abstract void reverse();
    
    // standard getters and setters
}
```

```java
public class Car extends Vehicle {

    @Override
    protected void start() {
        // code implementation details on starting a car.
    }

    @Override
    protected void stop() {
        // code implementation details on stopping a car.
    }

    @Override
    protected void drive() {
        // code implementation details on start driving a car.
    }

    @Override
    protected void changeGear() {
        // code implementation details on changing the car gear.
    }

    @Override
    protected void reverse() {
        // code implementation details on reverse driving a car.
    }
}
```

- In the above code, the _Vehicle_ class has been defined as abstract along with other abstract methods. 
- It provides generic operations of any real-world vehicle and also has several common functionalities. 
- The _Car_ class, which extends the _Vehicle_ class, overrides all the methods by providing the car’s implementation details (“Car is a Vehicle”).

# OOP Pillars
# Abstraction
Abstraction is hiding complexities of implementation and exposing simpler interfaces.

In OOP, abstraction means hiding the complex implementation details of a program, exposing only the API required to use the implementation. 

In Java, we achieve abstraction by using interfaces and abstract classes.
# Encapsulation
**Encapsulation is hiding the state or internal representation of an object from the consumer of an API** and providing publicly accessible methods bound to the object for read-write access. 

For example, in Java, is making all data fields _private_ and only accessible by using the _public_ member methods:

```java
public class Car {
    private int speed;

    public int getSpeed() {
        return color;
    }

    public void setSpeed(int speed) {
        this.speed = speed;
    }
}
```
Here, the field _speed_ is encapsulated using the _private_ access modifier, and can only be accessed using the _public getSpeed()_ and _setSpeed()_ methods.
# Inheritance
**Inheritance is the mechanism that allows one class to acquire all the properties from another class by inheriting the class.**

In Java, we do this by extending the parent class. Thus, the child class gets all the properties from the parent:

```java
public class Car extends Vehicle { 
    //...
}
```

When we extend a class, we form an [IS-A relationship](https://www.baeldung.com/java-inheritance-composition). **The _Car_ IS-A _Vehicle_.** So, it has all the characteristics of a _Vehicle_.

While we inherit from the parent class, a developer could also override a method implementation from the parent. **This is known as [method overriding](https://www.baeldung.com/java-method-overload-override#method-overriding).**
# Polymorphism
[Polymorphism](https://www.baeldung.com/cs/polymorphism) is the ability of an OOP language to process data differently depending on their types of inputs. 

In Java, this can be the same method name having **different parameters** and performing different functions,

aka **method overloading**:

```java
public class TextFile extends GenericFile {
    public String read() {
        return this.getContent()
          .toString();
    }
 
    public String read(int limit) {
        return this.getContent()
          .toString()
          .substring(0, limit);
    }
 
    public String read(int start, int stop) {
        return this.getContent()
          .toString()
          .substring(start, stop);
    }
}
```


There is also runtime or **dynamic polymorphism, where the child class overrides the parent’s method**:
```java
public class GenericFile {
    private String name;
 
    public String getFileInfo() {
        return "Generic File Impl";
    }
}
```

A child class can extend the _GenericFile_ class and override the _getFileInfo()_ method:

```java
public class ImageFile extends GenericFile {
    private int height;
    private int width;
 
    //... getters and setters
     
    public String getFileInfo() {
        return "Image File Impl";
    }
}
```

# Java 8 Streams
Java Streams or Streams is a major new functionality in Java 8 – [_java.util.stream_](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/stream/package-summary.html).

The `java.util.stream` package consists of classes, interfaces, and many types **to allow for functional-style operations over elements.**

Key Definitions:
- From a developer point of view, it is a new concept that might just look like a Collection, but it is in fact much different from a Collection.
- A Stream **does not** hold any data. This is very important to keep that in mind and understand.
	- There is no data in a _Stream_, however, there is data held in a _Collection_.
- A Stream shouldn't Modify the Source

## Stream Creation
A _stream()_ default method is added to the _Collection_ interface and allows creating a `Stream<T>` using any collection as an element source:

```java
Stream<String> stream = list.stream();
```

### Multi-threading With Streams
Stream API also simplifies multithreading by providing the _parallelStream()_ method that runs operations over the stream’s elements in parallel mode.

The code below allows us to run method _doWork()_ in parallel for every element of the stream:

```java
list.parallelStream().forEach(element -> doWork(element));
```

## Stream Operations
> It’s also worth noting that operations on streams don’t change the source.

Here’s a quick example:

```java
long count = list.stream().distinct().count();
```

So, the _distinct()_ method represents an intermediate operation, which creates a new stream of unique elements of the previous stream. 

And the _count()_ method is a terminal operation, which returns stream’s size.