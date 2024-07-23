# SOLID
Resources:

| Link                                                                                                         | Description                                  |
| ------------------------------------------------------------------------------------------------------------ | -------------------------------------------- |
| https://www.oodesign.com/                                                                                    | OODesign.com                                 |
| https://github.com/jwasham/coding-interview-university?tab=readme-ov-file#additional-detail-on-some-subjects | Coding Interview University -> SOLID Section |
## Single Responsibility Principle
**There should never be more than one reason for a class to change.**
### Example
User Management in a Web Applications
![](attachments/Pasted%20image%2020240723102119.png)
Applying SRP, these functions should be separated into different classes:

- A **User** class solely for storing and managing user details.
- A **UserValidator** class for validating user data.
- A **UserReportGenerator** class for creating user activity reports.
- An **Authenticator** class for handling authentication.
- An **Authorizer** class for managing user permissions.

## Open-Closed Principle
Software entities like classes, modules and functions should be **open for extension** but **closed for modifications**.

The **Open Close Principle** states that the design and writing of the code should be done in a way that new functionality should be added with minimum changes in the existing code. 

The design should be done in a way to allow the adding of new functionality as new classes, keeping as much as possible existing code unchanged.
### Bad Example
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
### Good Example
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
### PlantUML Example
```plantuml
Bob -> Alice : hello
Alice -> Wonderland: hello
Wonderland -> next: hello
next -> Last: hello
Last -> next: hello
next -> Wonderland : hello
Wonderland -> Alice : hello
Alice -> Bob: hello
Bob --> Alice: response
Bob --> Alice: hahahahaha
```
