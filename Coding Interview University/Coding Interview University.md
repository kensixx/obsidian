# SOLID
Resources:

| Link                                                                                                         | Description                                  |
| ------------------------------------------------------------------------------------------------------------ | -------------------------------------------- |
| https://www.oodesign.com/                                                                                    | OODesign.com                                 |
| https://github.com/jwasham/coding-interview-university?tab=readme-ov-file#additional-detail-on-some-subjects | Coding Interview University -> SOLID Section |
1. [x] SOLID - 
2. [ ] Design Patterns, 1 example for each category 
3. [x] OOP Pillars
	- [x] Abstraction
	- [x] Encapsulation
	- [x] Inheritance
	- [x] Polymorphism
1. [x] Best practices pag create restful api
2. [x] Interface vs Abstract
3. [x] Concurrency
4. [x] Stream
5. [ ] collect HackerRank or relevant coding questions, show examples
6. [ ] add Spring Boot terminologies, Spring DI, autowiring - definitions
7. [ ] add HashMap, Hashset, ArrayList, etc. and when to use those

# SOLID Summary
The SOLID principles are five design principles that help developers create more maintainable and scalable object-oriented systems:
   - **S - Single Responsibility Principle (SRP):** A class should have only one reason to change, meaning it should have only one responsibility.
   - **O - Open/Closed Principle (OCP):** Software entities (classes, modules, functions) should be open for extension but closed for modification.
   - **L - Liskov Substitution Principle (LSP):** Objects of a superclass should be replaceable with objects of a subclass without affecting the functionality.
   - **I - Interface Segregation Principle (ISP):** A client should not be forced to implement interfaces it doesn’t use. Instead of having one large interface, break it into smaller ones.
   - **D - Dependency Inversion Principle (DIP):** High-level modules should not depend on low-level modules. Both should
# Single Responsibility Principle
- **There should be only one reason for a class to change.**
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
- The LSP in simple terms states that objects of the same superclass should be able to be swapped with each other without breaking anything.
- For example, if we have a `Cat` and a `Dog` class derived from an `Animal` class, any functions using the `Animal` class should be able to use `Cat` or `Dog` and behave normally.

![](attachments/Pasted%20image%2020240821112150.png)

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
## Real-World Example:
Reference: https://stackoverflow.com/a/48855491/7209628

```php
<?php

interface Database 
{
    public function selectQuery(string $sql): array;
}

class SQLiteDatabase implements Database
{
    public function selectQuery(string $sql): array
    {
        // sqlite specific code

        return $result;
    }
}

class MySQLDatabase implements Database
{
    public function selectQuery(string $sql): array
    {
        // mysql specific code

        return $result; 
    }
}
```

This design conforms to the LSP because the behavior remains unchanged regardless of the implementation we choose to use.

And yes, you can violate LSP in this configuration doing one simple change like so :

```php
<?php

interface Database 
{
    public function selectQuery(string $sql): array;
}

class SQLiteDatabase implements Database
{
    public function selectQuery(string $sql): array
    {
        // sqlite specific code

        return $result;
    }
}

class MySQLDatabase implements Database
{
    public function selectQuery(string $sql): array
    {
        // mysql specific code

        return ['result' => $result]; // This violates LSP !
    }
}
```

Now the subtypes cannot be used the same way since they don't produce the same result anymore.
## Additional Example - Rectangles & Squares
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
The Singleton pattern is a creational design pattern that ensures a class has only one instance and provides a global point of access to that instance.

**Intent:**
- To restrict the instantiation of a class to one single instance and provide a global access point to it.

**When to Use:**
- When there should be exactly one instance of a class that is shared across the system.
- Common examples include logging, configuration settings, or managing a connection to a database.

**Implementation:**

- The class has a private constructor to prevent instantiation from outside.
- A private static instance of the class is created and returned through a public static method.

Example:
```java
public class Singleton {
    // Private static variable to hold the single instance
    private static Singleton instance;

    // Private constructor to prevent instantiation
    private Singleton() {}

    // Public method to provide access to the instance
    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```
## Behavioral Patterns
### Strategy Pattern
The Strategy pattern is a behavioral design pattern that defines a family of algorithms, encapsulates each one, and makes them interchangeable. The strategy pattern lets the algorithm vary independently from the clients that use it.

**Intent:**
- To enable selecting an algorithm at runtime based on the situation.
- To encapsulate related algorithms into a group that can be swapped in and out without changing the client code.

Example:
```java
// Strategy Interface
interface PaymentStrategy {
    void pay(int amount);
}

// Concrete Strategies
class CreditCardPayment implements PaymentStrategy {
    public void pay(int amount) {
        System.out.println("Paid " + amount + " using Credit Card");
    }
}

class PayPalPayment implements PaymentStrategy {
    public void pay(int amount) {
        System.out.println("Paid " + amount + " using PayPal");
    }
}

// Context class
class ShoppingCart {
    private PaymentStrategy paymentStrategy;

    public void setPaymentStrategy(PaymentStrategy paymentStrategy) {
        this.paymentStrategy = paymentStrategy;
    }

    public void checkout(int amount) {
        paymentStrategy.pay(amount);
    }
}

// Client code
public class StrategyPatternExample {
    public static void main(String[] args) {
        ShoppingCart cart = new ShoppingCart();

        // Pay using credit card
        cart.setPaymentStrategy(new CreditCardPayment());
        cart.checkout(100); // Outputs: Paid 100 using Credit Card

        // Pay using PayPal
        cart.setPaymentStrategy(new PayPalPayment());
        cart.checkout(150); // Outputs: Paid 150 using PayPal
    }
}
```
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
- they will be used by interface implementing classes, aka "impl classes"
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
- Abstract involves reusability of code to other subclasses.
- Abstract makes use the inheritance concept in code (**share code among many related classes**), by providing **common base class** methods that the subclasses override
- **Consider using abstract classes and inheritance when our problem makes the evidence “A is a B”.** 
	- For example, “Dog is an Animal”, “Lamborghini is a Car”, etc.

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
## Abstraction
Abstraction is hiding complexities of implementation and exposing simpler interfaces.

In OOP, abstraction means hiding the complex implementation details of a program, exposing only the API required to use the implementation. 

> In Java, we achieve abstraction by using **interfaces and abstract classes**.
## Encapsulation
**Encapsulation is hiding the state of an object from the consumer of an API** and providing publicly accessible methods bound to the object for read-write access. 

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
## Inheritance
**Inheritance is the mechanism that allows one class to acquire all the properties from another class by inheriting the class.**

In Java, we do this by extending the parent class. Thus, the child class gets all the properties from the parent:

```java
public class Car extends Vehicle { 
    //...
}
```

When we extend a class, we form an [IS-A relationship](https://www.baeldung.com/java-inheritance-composition). **The _Car_ IS-A _Vehicle_.** So, it has all the characteristics of a _Vehicle_.

While we inherit from the parent class, a developer could also override a method implementation from the parent. **This is known as [method overriding](https://www.baeldung.com/java-method-overload-override#method-overriding).**
## Polymorphism

> Polymorphism is the concept of method overloading in Java.

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
> **This is a Java-equivalent to the lambda expressions in JavaScript.**

Java Streams or Streams is a major new functionality in Java 8 – [_java.util.stream_](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/stream/package-summary.html).

The `java.util.stream` package consists of classes, interfaces, and many types **to allow for functional-style operations over elements.**

Key Definitions:
- From a developer point of view, it is a new concept that might just look like a Collection, but it is in fact much different from a Collection.
- A Stream **does not** hold any data. This is very important to keep that in mind and understand.
	- There is no data in a _Stream_, however, there is data held in a _Collection_.
- A Stream shouldn't Modify the Source.

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

### Iterating[](https://www.baeldung.com/java-8-streams-introduction#1-iterating)

Stream API helps to substitute _for_, _for-each_, and _while_ loops. It allows concentrating on operation’s logic, but not on the iteration over the sequence of elements. For example:

```java
for (String string : list) {
    if (string.contains("a")) {
        return true;
    }
}
```

This code can be changed just with one line of Java 8 code:

```java
boolean isExist = list.stream().anyMatch(element -> element.contains("a"));
```

### Filtering[](https://www.baeldung.com/java-8-streams-introduction#2-filtering)

The _filter()_ method allows us to pick a stream of elements that satisfy a predicate.

For example, consider the following list:

```java
ArrayList<String> list = new ArrayList<>();
list.add("One");
list.add("OneAndOnly");
list.add("Derek");
list.add("Change");
list.add("factory");
list.add("justBefore");
list.add("Italy");
list.add("Italy");
list.add("Thursday");
list.add("");
list.add("");
```

The following code: 
- creates a `Stream<String>` of the `List<String>`
- finds all elements of this stream which contain _char “d”_, 
- and creates a new stream containing only the filtered elements:

```java
Stream<String> stream = list.stream().filter(element -> element.contains("d"));
```
### Mapping[](https://www.baeldung.com/java-8-streams-introduction#3-mapping)

To convert elements of a _Stream_ by applying a special function to them and to collect these new elements into a _Stream_, we can use the _map()_ method:

```java
List<String> uris = new ArrayList<>();
uris.add("C:\\My.txt");
Stream<Path> stream = uris.stream().map(uri -> Paths.get(uri));
```

So, the code above converts `Stream<String>` to the `Stream<Path>` by applying a specific lambda expression to every element of the initial _Stream_.

If you have a stream where every element contains its own sequence of elements and you want to create a stream of these inner elements, you should use the _flatMap()_ method:

For example:

```java
List<Detail> details = new ArrayList<>();
details.add(new Detail());
Stream<String> stream
  = details.stream().flatMap(detail -> detail.getParts().stream());
```


### Matching[](https://www.baeldung.com/java-8-streams-introduction#4-matching)

Stream API gives a handy set of instruments to validate elements of a sequence according to some predicate. 

To do this, one of the following methods can be used: _anyMatch(), allMatch(), noneMatch()._ Their names are self-explanatory. Those are terminal operations that return a _boolean_:

```java
boolean isValid = list.stream().anyMatch(element -> element.contains("h")); // true
boolean isValidOne = list.stream().allMatch(element -> element.contains("h")); // false
boolean isValidTwo = list.stream().noneMatch(element -> element.contains("h")); // false
```

For empty streams, the _allMatch()_ method with any given predicate will return _true_:

```java
Stream.empty().allMatch(Objects::nonNull); // true
```

This is a sensible default, as we can’t find any element that doesn’t satisfy the predicate.

Similarly, the _anyMatch()_ method always returns _false_ for empty streams:

```java
Stream.empty().anyMatch(Objects::nonNull); // false
```


### Reduction[](https://www.baeldung.com/java-8-streams-introduction#5-reduction)

Stream API allows reducing a sequence of elements to some value according to a specified function with the help of the _reduce()_ method of the type _Stream_. This method takes two parameters: first – start value, second – an accumulator function.

Imagine that you have a `List<Integer>` and you want to have a sum of all these elements and some initial _Integer_ (in this example 23). So, you can run the following code and result will be 26 (23 + 1 + 1 + 1).

```java
List<Integer> integers = Arrays.asList(1, 1, 1);
Integer reduced = integers.stream().reduce(23, (a, b) -> a + b);
```


### Collecting[](https://www.baeldung.com/java-8-streams-introduction#6-collecting)

The reduction can also be provided by the _collect()_ method of type _Stream._ This operation is very handy in case of converting a stream to a _Collection_ or a _Map_ and representing a stream in the form of a single string. 

There is a utility class _Collectors_ which provide a solution for almost all typical collecting operations. For some, not trivial tasks, a custom _Collector_ can be created.

```java
List<String> resultList 
  = list.stream().map(element -> element.toUpperCase()).collect(Collectors.toList());
```

This code uses the terminal _collect()_ operation to reduce a `Stream<String>` to the `List<String>`.


# Java Concurrent / Concurrency
Resource: https://www.baeldung.com/java-util-concurrent

The _java.util.concurrent_ package provides tools for creating concurrent applications.
## Main Components [](https://www.baeldung.com/java-util-concurrent#main-components)

These are some of the most useful utilities from the _java.util.concurrent_ package:
- _Executor_
- _Future_
# Spring Boot Questions

## What is Spring Boot?
 Spring Boot is basically an extension of the Spring framework, which eliminates the boilerplate configurations required for setting up a Spring application.

It takes an opinionated view of the Spring platform, which paves the way for a faster and more efficient development ecosystem.
## Differentiate Spring Framework and Spring Boot?
the Spring framework provides comprehensive infrastructure support for developing Java applications.

It’s packed with some nice features like Dependency Injection, and out of the box modules like:
- Spring JDBC
- Spring MVC
- Spring Security
- Spring AOP
- Spring ORM
- Spring Test
## Explain IoC and Dependency Injection?
### Summary
- **Inversion of Control (IoC)** is a  concept where the control of object creation and lifecycle management is given to a framework or container.
- **Dependency Injection (DI)** is an implementation of the IoC principle, where dependencies are injected into a class by the framework rather than the class managing them itself.

**Dependency Injection (DI)** is a **specific implementation** of the IoC principle. 

It refers to the process of injecting dependencies (objects that a class needs to function) into a class, rather than the class creating those dependencies itself.

In Spring, the IoC container is responsible for managing the lifecycle of beans (objects). 

When you declare beans in the Spring configuration, the container is responsible for instantiating, configuring, and assembling the dependencies of these beans.

Example in Spring:

Spring uses DI to wire beans together. For instance, if you have a Service class that depends on a Repository, you can declare these beans in the Spring context, and Spring will automatically inject the Repository into the Service using DI.

```java
@Service
public class MyService {
    private final MyRepository repository;

    @Autowired
    public MyService(MyRepository repository) {
        this.repository = repository;
    }
}
```
In this example, MyService depends on MyRepository, and Spring will inject MyRepository into MyService when the application starts.
## What is Hibernate?
1. ****Hibernate ORM:**** Hibernate is an object-relational mapping (ORM) framework, that provides a bridge between Java objects and relational database tables. Overall no need to write SQL queries manually. 
2. - Spring Data JPA****:**** Recommended way to integrate Spring with Hibernate, It simplifies data access using JPA annotations.
---
Here are some common interview questions for a Senior Developer role focused on Spring Boot:
## Core Spring Boot
1. What is the difference between `@Component`, `@Service`, `@Repository`, and `@Controller`?
**Answer:**
- `@Component`: A generic stereotype for any Spring-managed component.
- `@Service`: Specialization of `@Component`, used for business logic.
- `@Repository`: Specialization of `@Component`, used for data access logic.
- `@Controller`: Specialization of `@Component`, used to define a web controller in an MVC application.
 
2. Difference between @RestController and @Controller annotations?
- The `@RestController` annotation is a convenience annotation that combines `@Controller` and `@ResponseBody`. 
- It is used to create RESTful web services where the return value of methods is serialized directly to the HTTP response body.

3. How do you handle exceptions in Spring Boot?
	**Answer:** 
	- You can handle exceptions in Spring Boot using `@ControllerAdvice` and `@ExceptionHandler` for controller-level exception handling, 
	- or by creating custom error pages and configuring them using `ErrorController`
### Security and Authentication
6. *How do you secure a Spring Boot application?*
   - Discuss the use of Spring Security, including basic authentication, JWT, OAuth2, etc.

7. *What is OAuth2? How do you implement it in a Spring Boot application?*
   - Explain the OAuth2 authorization flow and the use of Spring Security OAuth2 for implementation.

### Data Access
8. *How does Spring Boot handle data access with JPA?*
   - Discuss how to configure Spring Data JPA, create repositories, and manage transactions.

9. **What is the difference between CrudRepository, JpaRepository, and PagingAndSortingRepository?**
	- **CrudRepository**
		Purpose:
		- Provides basic CRUD (Create, Read, Update, Delete) operations.
		- It is the simplest interface in Spring Data JPA and is meant for basic operations without the need for pagination or sorting.
	- **PagingAndSortingRepository**
		Purpose:
		- Extends **CrudRepository** and adds methods for pagination and sorting of records.
	- **JpaRepository**
		Purpose:
		- Extends **PagingAndSortingRepository** and adds JPA-specific methods, such as batch operations and the ability to flush persistence contexts.
		- It offers a more extensive set of methods compared to CrudRepository

10. *How would you optimize database access in a Spring Boot application?*
    - Talk about strategies like query caching, batch processing, and connection pooling.

### Microservices and Distributed Systems
11. *What is Spring Cloud, and how does it relate to Spring Boot?*
    - Discuss components like Spring Cloud Config, Netflix Eureka, Zuul, and Hystrix.

12. *How would you implement service discovery in Spring Boot?*
    - Explain how to use Spring Cloud Netflix Eureka for service registration and discovery.

13. *What are Circuit Breakers, and how can you implement them in Spring Boot?*
    - Discuss the use of Resilience4j or Hystrix for implementing circuit breakers.

14. *How do you handle distributed tracing and logging in a microservices environment?*
    - Talk about tools like Spring Cloud Sleuth, Zipkin, and ELK stack.

### Testing
17. *How do you test a Spring Boot application?*
    - Discuss testing strategies, including unit testing, integration testing, and tools like JUnit, Mockito, and Spring Boot Test.

18. *What is TestRestTemplate, and how do you use it in Spring Boot?*
    - Explain how to use TestRestTemplate for testing REST endpoints.
### Performance and Scalability
19. *How do you monitor and optimize the performance of a Spring Boot application?*
    - Talk about monitoring tools like Actuator, Prometheus, Grafana, and performance tuning techniques.

20. *How would you scale a Spring Boot application in a cloud environment?*
    - Discuss containerization (Docker), orchestration (Kubernetes), and auto-scaling.

### Advanced Topics
21. *What are the different ways to deploy a Spring Boot application?*
    - Talk about deploying to standalone servers, cloud platforms, containers, etc.

22. *How do you handle long-running processes or batch jobs in Spring Boot?*
    - Explain the use of Spring Batch or scheduling with @Scheduled.

23. *How would you implement a distributed cache in a Spring Boot application?*
    - Discuss using caching solutions like Redis or Hazelcast with Spring Cache.

### Design and Architecture
24. *Can you describe a complex architecture you have worked on using Spring Boot?*
    - Be ready to discuss the overall design, challenges faced, and solutions implemented.

25. *How do you approach designing RESTful APIs in Spring Boot?*
    - Discuss REST principles, HATEOAS, versioning, and documentation with Swagger/OpenAPI.

These questions cover a wide range of topics that could come up during an interview for a Senior Developer role with a focus on Spring Boot. Be sure to also discuss any specific projects or experiences you've had, as real-world examples are often key in senior-level interviews.