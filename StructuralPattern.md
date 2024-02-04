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
##### Adapter class to make the new image format to work with the existing format.
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
- Example :
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
  
