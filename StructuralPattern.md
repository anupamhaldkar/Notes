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
