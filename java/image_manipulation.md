# Image Manipulation

## Convert between BufferedImage and byte[]

Use `javax.imageio.ImageIO` to convert between byte streams and `BufferedImage`. (example derived from code at http://www.mkyong.com/java/how-to-convert-byte-to-bufferedimage-in-java/).

```java
byte[] imageData = getImageData();
InputStream byteIn = new ByteArrayInputStream(imageData);
BufferedImage bufferedImage = ImageIO.read(byteIn);

//...

ByteArrayOutputStream byteOut = new ByteArrayOutputStream();
ImageIO.write(bufferedImage, "jpg", byteOut);
byteOut.flush();
byte[] resultData = byteOut.toByteArray();
byteOut.close();
```

## Convert Image to BufferedImage
`BufferedImage` provides a lot more flexibility than `Image`, so converting may be useful. Conversion requires redrawing the `Image` onto the `BufferedImage`'s graphics. (example derived from code at http://stackoverflow.com/questions/13605248/java-converting-image-to-bufferedimage).

```java
    Image originalImage = getOriginalImage();
    BufferedImage bufferedImage = new BufferedImage(width, height, type);

    // Draw the image on to the buffered image
    Graphics2D graphics2D = bufferedImage.createGraphics();
    graphics2D.drawImage(originalImage, 0, 0, null);
    graphics2D.dispose();
```

## Scale Image
Regular `Image` objects can be scaled via `getScaledInstance`, but the performance can be problematic, and the result is not a `BufferedImage`. Using an AffineTransformOp to scale down can be useful. (example derived from code at http://stackoverflow.com/questions/4216123/how-to-scale-a-bufferedimage).

```java
double xScale = targetX / originalImage.getWidth();
double yScale = targetY / originalImage.getHeight();
double scaleFactor = Math.min(xScale, yScale);

/*
    Dimensions should be equal to final size. If smaller, will throw an
    IOException. If larger, will put a box around the generated image.
*/
BufferedImage targetImage =
        new BufferedImage((int)(Math.ceil(originalImage.getWidth() * scaleFactor)),
                (int)(Math.ceil(originalImage.getHeight() * scaleFactor)),
                originalImage.getType());

AffineTransform transform = new AffineTransform();
transform.scale(scaleFactor, scaleFactor);
AffineTransformOp scaleOp = new AffineTransformOp(transform, AffineTransformOp.TYPE_BILINEAR);

/*
    note that scaleOp.filter will supposedly generate targetImage using
    originalImage's color data; however, in practice I've observed this causing
    color distortion.
*/
targetImage = scaleOp.filter(originalImage, targetImage);

```
