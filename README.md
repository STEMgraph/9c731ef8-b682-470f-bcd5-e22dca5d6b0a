<!---
{
  "id": "9c731ef8-b682-470f-bcd5-e22dca5d6b0a",
  "depends_on": [
   "93b1742d-4a11-4390-a6c4-2818f3de569e",
   "fcef696e-079c-4d83-b611-7b378bb8ac07",
   "7f50ba23-f5a6-4bc7-887f-ed9247220544"
   ],
  "author": "Stephan Bökelmann",
  "first_used": "2025-06-04",
  "keywords": ["SVG", "conversion", "PNG", "JPEG", "Linux", "command-line", "rasterization"]
}
--->
# Converting SVG Files to Raster Images on Linux

> In this exercise you will learn how to convert SVG vector graphics into raster image formats like PNG and JPEG using Linux command-line tools. Furthermore we will explore how rendering options such as resolution, scaling, and anti-aliasing affect the rasterization process.

## Introduction

Writing images as text-based descriptions, like SVG, gives us a fundamentally powerful way to create and manipulate graphics. Every element of the image is described explicitly in a human-readable format: shapes, colors, transformations, and even text annotations are stored directly in the file as markup. This explicit structure allows us to leave comments, track changes with version control systems like Git, and maintain a clear history of how a picture was built over time.

Because SVG files are text, we can review changes line-by-line, identify who modified which parts, and merge different changes easily — something that is virtually impossible with traditional raster images like PNG or JPEG. This makes SVG a natural fit for workflows that prioritize transparency, reproducibility, and traceability. Especially in scientific illustrations, technical diagrams, or any form of documentation, it is crucial to understand exactly how an image was produced and be able to recreate or modify it precisely.

However, there are many situations where raster images are still necessary. Many software applications, printers, websites, and digital systems require images in fixed-size raster formats. These formats are easier to handle for real-time rendering, less computationally demanding, and compatible with environments that do not support SVG. Yet, when we generate such raster images from SVG, we are only producing a *projection* — a simplified, pixel-based representation — of the true underlying image, which contains much richer structural information.

In a well-structured workflow, the SVG source remains the master copy: complete, editable, and version-controlled. Raster images serve as temporary exports for distribution or publication but do not replace the original source. This separation ensures long-term maintainability and avoids losing the ability to reproduce or adjust the image later.

In this exercise, you will learn how to perform this conversion process using command-line tools on Linux, while maintaining full control over resolution, scaling, and output quality.

### Further Readings and Other Sources

* [ImageMagick Convert Documentation](https://imagemagick.org/script/convert.php)
* [rsvg-convert Documentation](https://manpages.ubuntu.com/manpages/latest/man1/rsvg-convert.1.html)
* [Understanding Rasterization](https://en.wikipedia.org/wiki/Rasterisation)
* [SVG Rasterization Explained (YouTube)](https://www.youtube.com/watch?v=daWx1vSrbrA)

## Tasks

### Task 1: Install Required Tools

1. Update your package list and install ImageMagick and librsvg using `apt`:

   ```bash
   sudo apt update
   sudo apt install imagemagick librsvg2-bin
   ```
2. Verify that `convert` and `rsvg-convert` are available:

   ```bash
   convert -version
   rsvg-convert --version
   ```

### Task 2: Prepare a Source SVG File

1. Use one of your previous SVG files from the earlier exercises, or create a new one.
2. Make sure your SVG contains a combination of basic shapes, gradients, and transformations.
3. Save the file as `mygraphic.svg` in your working directory.

### Task 3: Simple Conversion Using ImageMagick

1. Convert your SVG to PNG with default settings:

   ```bash
   convert mygraphic.svg mygraphic.png
   ```
2. Inspect the generated file `mygraphic.png` using any image viewer.
3. Observe how the viewBox and transformations are rendered.

### Task 4: Control Output Resolution (ImageMagick)

1. Export at a higher resolution using the `-density` option:

   ```bash
   convert -density 300 mygraphic.svg mygraphic_300dpi.png
   ```
2. Compare the file size and sharpness against the default export.
3. Export at a very low resolution (e.g., 30 DPI):

   ```bash
   convert -density 30 mygraphic.svg mygraphic_30dpi.png
   ```

### Task 5: Conversion to JPEG with Background Fill

1. SVG often has transparent backgrounds, but JPEG does not support transparency.
2. Convert while setting the background color to white:

   ```bash
   convert -background white -flatten mygraphic.svg mygraphic.jpg
   ```
3. Inspect the result and verify that the background is white.

### Task 6: Batch Conversion and Automation

1. Duplicate your SVG file several times to simulate multiple files:

   ```bash
   cp mygraphic.svg file1.svg
   cp mygraphic.svg file2.svg
   cp mygraphic.svg file3.svg
   ```
2. Write a simple loop to convert all SVG files to PNG:

   ```bash
   for file in *.svg; do
     convert "$file" "${file%.svg}.png"
   done
   ```
3. Verify that all PNG versions are created.

### Task 7: Observe Rendering Limitations of ImageMagick

1. Notice that ImageMagick may render some advanced SVG features (like filters, complex gradients, or certain text features) incorrectly or not at all.
2. Experiment by adding filters or complex gradients to your SVG file and observe the output.
3. Reflect on the limitations of using general-purpose tools for specialized rendering tasks.

### Task 8: Simple Conversion Using rsvg-convert

1. Use `rsvg-convert` to render your SVG to PNG:

   ```bash
   rsvg-convert -o mygraphic_rsvg.png mygraphic.svg
   ```
2. Inspect the output and compare it to ImageMagick's result.
3. Verify that gradients, filters, and transformations are rendered more faithfully.

### Task 9: Control Output Size with rsvg-convert

1. Export the SVG to a fixed width and height:

   ```bash
   rsvg-convert -w 800 -h 600 -o mygraphic_rsvg_resized.png mygraphic.svg
   ```
2. Observe how the scaling behaves relative to the SVG’s internal coordinate system and viewBox.

### Task 10: Simulate Viewport Changes by Editing viewBox

1. Edit your `mygraphic.svg` file and modify the viewBox to zoom into a part of your drawing:

   ```xml
   <svg viewBox="50 50 100 100" width="400" height="400" ...>
   ```
2. Re-export with rsvg-convert:

   ```bash
   rsvg-convert -o mygraphic_zoomed.png mygraphic.svg
   ```
3. Observe how the altered viewBox affects the exported raster image.

## Advice

While ImageMagick is useful for quick conversions, `rsvg-convert` provides much more faithful rendering of SVG content. It correctly handles gradients, filters, and complex transformations that ImageMagick may ignore or approximate. For automated pipelines or production-quality exports, `rsvg-convert` is often the better choice on Linux systems. Always preserve your original SVG files and test different resolutions and scaling options to achieve optimal raster output.
