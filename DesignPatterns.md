# Design Patterns

### 1. Creational 
- Focused on the creation process of objects 
### 2. Structural
- Focused on the class composition and relationship that helps to efficient structure
### 3. Behavioural
- Focused on communication among objects and classes.


### Creational 
#### 1. Singleton
- has only one instance and provides a global point of access to that instance.
- Example - config. setting, DB connections, and logging systems.
```
public class MySingleton {
private static MySingleton instance = null;
private MySingleton(){
      }
public static MySingleton getInstance(){
    if(instance == null){
      instance = new MySingleton();
      }
  }
}
```
#### 2. Factory Method
- involves defining an interface for creating objects while delegating the responsibilities of deciding which class to instantiate to its subclasses.
- allowing for interchangeable implementations and promoting code reuse. Ex: Collection have set, list etc.
```
interface Product {
  void doSomething();
}

class ConcreteProduct implements Product {
  public void doSomething(){
    System.out.println("Method implemented");
  }
}

class Creator {
  public Product createProduct(){
    return new ConcreteProduct();
  }
}
```
#### 3. Abstract Factory
- create families of related or dependent objects without explicitly specifying their concrete classes. 
- It takes one step further from the factory method by introducing an abstract base type for the factory itself.
- Example - UI components across different platforms - make abstract Component Base and call accordingly with the platform.

Interfaces   
```
interface Shape {
  void draw();
}

interface Color {
 void fill();
}
```
Concrete Classes
```
class Circle implements Shape {
  @Override
  public void draw(){
    System.out.println("Inside Cicle::draw() method");
  }
}

class Square implements Shape {
  @Override
  public void draw(){
    System.out.println("Inside Square::draw() method");
  }
}

class Red implements Color {
  @Override
  void fill(){
     System.out.println("Inside Red::fill() method");
  }
}

class Blue implements Color {
  @Override
  void fill(){
     System.out.println("Inside Blue::fill() method");
  }
}

```
Abstract Factory and concrete factory classes
```
interface AbstractFactory {
  Shape createShape();
  Color createColor();
}

class RedShapeFactory implements AbstractFactory {
  @Override
  public Shape createShape(){
    return new Circle();
  }

  @Overrride
  public Color createColor(){
    return new Red();
  }
}

classBlueShapeFactory implements AbstractFactory {
  @Override
  public Shape createShape(){
    return new Square();
  }

  @Overrride
  public Color createColor(){
    return new Blue();
  }
}
```


#### 4. Builder
- Focuses on Separating the construction of a complex object from its representation
- to generate different representations of the object.
- Example - Generation of optional parameters while using build method.


```
class Car {
private String make;
private String model;
private int year;

public Car(CarBuilder builder){
  this.make = builder.getMake();
  this.model = builder.getModel();
  this.year = builder.getYear();
}

public String getMake(){
  return make;
  }

public String getModel(){
  return model;
  }

public String getYear(){
  return year;
  }

public Car setYear(int year){
    this.year = year
    return this;
  }

}
```

Builder Class

```
class CarBuilder {
  private String make;
  private String model;
  private int year;

  public CarBuilder(String make, String model){
    this.make = make;
    this.model = model;
  }

  public CarBuilder setYear(int year){
    this.year = year;
    return this;
  }

  public String getMake(){
    return make;
  }

  public String setMake(){
    this.make = make;
  }

  public String getModel(){
    return model;
  }

  public String setModel(){
      this.model = model;
  }

  public Car build() {
    return new Car(this);
  }

}
```

Client Call

```
public class Client {
  public static void main(){
    Car car = new CarBuilder("Landrover","Rangerover").setYear(2022).build();
  }
}
```
#### 5. Prototype
- creating new objects by cloning an existing object, rather than invoking a constructor.
- newly created object are independent of the original object and can be used as needed.
- Example -
```
interface Prototype {
  Prototype clone();
}
```
Concrete Class 
```
class Circle implements Prototype {
  private int x;
  private int y;
  private int radius;

  public Circle(int x, int y, int radius){
    this.x = x;
    this.y = y;
    this.radius = radius;
  }

  @Override
  public Prototype clone(){
    return new Circle(this);
  }
}

```

Client class 
```
public class Client {
  public static void main(String ar[]){
  Circle original = new Circle(1,2,3);
  Circle cloned = (Circle)original.clone();
  System.out.println("Are they the same object? "+ (original == cloned) );  //false
  }
}
```

