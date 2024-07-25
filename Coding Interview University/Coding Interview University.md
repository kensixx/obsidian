# SOLID
Resources:

| Link                                                                                                         | Description                                  |
| ------------------------------------------------------------------------------------------------------------ | -------------------------------------------- |
| https://www.oodesign.com/                                                                                    | OODesign.com                                 |
| https://github.com/jwasham/coding-interview-university?tab=readme-ov-file#additional-detail-on-some-subjects | Coding Interview University -> SOLID Section |
1. SOLID 
2. Design Patterns, 1 example for each category 
3. Best practices pag create restful api
4. Interface vs Abstract
5. concurrency
6. Stream
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
