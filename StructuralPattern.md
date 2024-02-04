### Structural 
#### 1. Adapter
- when an existing class needs to be utilized by an object along with an incompatible interface.
- Two approaches to implement adapter pattern
  1. Class Adapter - make use of inheritance
  2. Object Adapter - make use of composition
- Example: java.util.Arrays.asList() method, Collection.toArray().

##### Existing Image interface
```
interface ImageView {
    void open(String fileName);
}

class ImageViewer implements ImageView {
    @Override
    public void open(String fileName) {
        System.out.println("opening file: " + fileName);
    }
}
```

##### New image viewer with a different interface (HEIF, GIF)
```
class NewImageViewer {
    void openHEIF(String fileName) {
        System.out.println("opening HIEF file: " + fileName);
    }
    void openGIF(String fileName) {
        System.out.println("opening GIF file: " + fileName);
    }
}
```
##### Adapter class
```
class ImageAdapter implements ImageView {
    private NewImageViewer newImageViewer;
    public ImageAdapter(NewImageViewer newImageViewer) {
        this.newImageViewer = newImageViewer;
    }
    @Override
    public void open(String fileName) {
        if (fileName.endsWith(".heif")) {
            newAudioLibrary.openHEIF(fileName);
        } else if (fileName.endsWith(".gif")) {
            newAudioLibrary.openGIF(fileName);
        } else {
            System.out.println("Invalid audio format: " + fileName);
        }
    }
}
```
##### Main Class
```
public class Main {
    public static void main(String[] args) {
        
        ImageViewer imageViewer = new ImageViewer();
        iamgeViewer.open("image.jpg");
        
        NewImageViewer newImageViewer = new NewImageViewer();
        ImageAdapter imageAdapter = new ImageAdapter(newImageViewer);
        imageAdapter.open("image.heif");
        imageAdapter.open("image.gif");
    }
}

```

#### 2. Bridge
- separates the main part(called abstraction) from the specific details(called implementation) so they can change independently.
- helps to keep software flexible, adaptable, extensible
- Example : JDBC API act as bridge between database connection and its implementation.
```
EyeShape interface
interface EyeShape {
	void create();
}

interface EyeColor {
	void addColor();
}
```
##### Concrete class
```
class InCircle implements EyeShape {
	@Override
	public void create() {
		System.out.println("Drawing InCircle");
	}
}


class Square implements IShape {
	@Override
	public void create() {
		System.out.println("Drawing Square");
	}
}

class Red implements IColor {
	@Override
	public void addColor() {
		System.out.println("Applying Red Color");
	}
}

class Blue implements IColor {
	@Override
	public void addColor() {
		System.out.println("Applying Blue Color");
	}
}
```

##### Bridge
```
abstract class ShapeColorBridge {
	protected IShape shape;
	protected IColor color;
	public ShapeColorBridge(IShape shape, IColor color) {
		this.shape = shape;
		this.color = color;
	}
	public abstract void drawShapeWithColor();
}

class CircleBridge extends ShapeColorBridge {
	public CircleBridge(EyeShape shape, EyeColor color) {
		super(shape, color);
	}
	@Override
	public void drawShapeWithColor() {
		shape.create();
		color.addColor();
	}
}

class SquareBridge extends ShapeColorBridge {
	public SquareBridge(EyeShape shape, EyeColor color) {
		super(shape, color);
	}
	@Override
	public void drawShapeWithColor() {
		shape.create();
		color.addColor();
	}
}
```
##### Main Class
```
public class Main {
	public static void main(String[] args) {
		ShapeColorBridge redCircle = new CircleBridge(new Circle(), new Red());
		ShapeColorBridge blueSquare = new SquareBridge(new Square(), new Blue());
		redCircle.drawShapeWithColor();
		blueSquare.drawShapeWithColor();
	}
}
```

#### 3. Composite
- treat a collection of objects in the same manner as a single object.
- hierarchically arranged object.
- 3 key participant
- 1. Component - base interface for all objects in the composition
  2. leaf - behaviour of individual.
  3. composite - responsible for storing child components and processing their operations.
- Example: java.util.Collection - implemented by classes such as ArrayList, HashSet etc.

##### Component interface
```
interface Student {
    void showDetails();
}
```
##### composite class
```
class Group implements Student {
    private String name;
    private List<Student> students;
    public Group(String name) {
        this.name = name;
        this.students = new ArrayList<>();
    }
    public void addStudents(Student student) {
        student.add(student);
    }
    @Override
    public void showDetails() {
        System.out.println("Group: " + name);
        for (Student student : students) {
            student.showDetails();
        }
    }
}
```
##### Leaf
```
class StudentLeaf implements Student {
    private String name;
    private String class;
    public StudentLeaf(String name, String class) {
        this.name = name;
        this.class = class;
    }
    @Override
    public void showDetails() {
        System.out.println("Student: " + name + ", class: " + class);
    }
}
```

##### Main class
```
public class Main {
    public static void main(String[] args) {
        // Create individual employees
        Student std1 = new StudentLeaf("G Vijay", "CS");
        Student std2 = new StudentLeaf("Kedar Jadhav", "IT");
        Student std3 = new StudentLeaf("Tripti vaidya", "EC");
        
        Group group1 = new Group("Group 1");
        group1.addStudents(std2);
        group1.addStudents(std3);
        Group group2 = new Group("Group 2");
        group2.addStudents(emp1);
        group2.addStudents(group1);
       
        group2.showDetails();
    }
}
```
#### 4. Decorator
- add extra functionality to an object by wrapping it with one or more decorator classes at run time.
- enhances the objects capability.
- promotes code reusability without modifying core functionalities.
- Example: Java.io package Objects, BufferedInputStream, etc.

##### Inteface
```  
interface Phone {
    double cost();
    String description();
}
```
##### Concrete class
```
class FeaturePhone implements Phone {
    @Override
    public double cost() {
        return 2000.0;
    }
    @Override
    public String description() {
        return "feature phone";
    }
}
```
##### Decorated class
```
abstract class PhoneDecorator implements Phone {
    private final Phone decoratedPhone;
    public PhoneDecorator(Phone phone) {
        this.decoratedPhone = phone;
    }
    @Override
    public double cost() {
        return decoratedPhone.cost();
    }
    @Override
    public String description() {
        return decoratedPhone.description();
    }
}
```
##### Concrete decorated class

```
class SmartPhoneDecorator extends PhoneDecorator {
    public SmartPhoneDecorator(Coffee coffee) {
        super(coffee);
    }
    @Override
    public double cost() {
        return super.cost() + 15000.0;
    }
    @Override
    public String description() {
        return super.description() + ", smartphone";
    }
}
class IphoneDecorator extends PhoneDecorator {
    public IphoneDecorator(Coffee coffee) {
        super(coffee);
    }
    @Override
    public double cost() {
        return super.cost() + 70000.0;
    }
    @Override
    public String description() {
        return super.description() + ", iPhone";
    }
}
```

##### Main class
```
public class Main {
    public static void main(String[] args) {
        Phone phone = new FeaturePhone();
        
        Phone smartPhone = new SmartPhoneDecorator(phone);
        System.out.println("Cost: Rs:" + smartPhone.cost() + ", Description: " + smartPhone.description());
        Phone iPhone = new IphoneDecorator(phone);
        System.out.println("Cost: Rs:" + iPhone.cost() + ", Description: " + iPhone.description());
    }
}

```


  
  
