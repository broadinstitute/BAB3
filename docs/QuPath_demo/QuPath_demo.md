# Day 3, session 3: Practical introduction to QuPath

*Lab author: Shatavisha Dasgupta* . 

Adapted from:
1. [QuPath documentation](https://qupath.readthedocs.io/en/stable/docs/tutorials/index.html)
2. [Pete Bankhead's video tutorial](https://youtu.be/xtjsigsUrms?si=PLyccu1S0xfYJHYN)
3. [From Samples to Knowledge 2025 Repo](https://saramcardle.github.io/FS2K/README.html)

## **Learning Objectives**
This tutorial provides an overview of the basic functionalities of QuPath.
Specifically, the tutorial covers the following:
- Creating a QuPath project
- Viewing pyramidal files
- Setting channel names of images
- Manually annotating regions of interest
- Detecting cells in the annotated regions
- Performing measurements on the detected cells and exporting the data

**Lab Data** in [this folder](https://drive.google.com/drive/folders/1z0FnjmPLGjEnIpPu_P8PTB-4ck6Z9BcA?usp=drive_link) (Originally sourced from [here](https://openslide.cs.cmu.edu/download/openslide-testdata/Hamamatsu/) and [here]((https://openslide.cs.cmu.edu/download/openslide-testdata/Aperio/CMU-1.svs)))

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

## **1. Background information**

### **1.1 What is QuPath?**
-------------------------------------

QuPath is an open source software for bioimage analysis created and maintained by Peter Bankhead and his team at The University of Edinburgh. QuPath is commonly used for **digital pathology in research** because it offers a powerful set of tools for working with **whole slide images** - but it can be applied to lots of other kinds of image as well.

Read the [original publication introducing QuPath](https://doi.org/10.1038/s41598-017-17204-5)!

QuPath provides powerful tools for **annotation, visualization, and image analysis** through an easy-to-use modern interface. It includes built-in algorithms for cell and tissue detection, interactive machine learning for object and pixel classification, and supports many image formats, including whole-slide and multiplexed images. It also integrates with tools like ImageJ, OpenCV, OMERO, StarDist, InstanSeg, and Bio-Formats, and the functionalities can be expanded using Groovy scripts and extensions. Many of these functionalities are beyond the scope of this tutorial -- if you are interested, [QuPath docs](https://qupath.readthedocs.io/en/stable/docs/tutorials/index.html) and these videos/channels ([1](https://www.youtube.com/playlist?list=PLlGXRBscPbCCA1yGCThNqdYKgTPOvjigp), [2](https://www.youtube.com/playlist?list=PLSCpSsEmyRpANBGQXB_oGslW9NIJz4A12), [3](https://www.youtube.com/playlist?list=PL4ta8RxZklWkPB_pwW-ZDVAGPGktAlE5Y)) are great resources to delve into!


### **1.2 What are pyramidal file formats?**
-------------------------------------
- Whole slide images (WSIs) are a crucial component of translational research focussing on Histopathology, because they allow comparing changes in diseased tissue with healthy tissue, which are often present on the same slide. Additionally, they also allow quantifying parameters of prognostic significance, such as depth of invasion of tumors
- WSIs contain a wealth of information -- and billions of pixels -- so these tend to be of substantial size, often exceeding 10 GB
- To make viewing, processing, and analysis more efficient, WSIs are commonly stored in pyramidal file formats
- A pyramidal file stores the same image at multiple resolutions within a single file. These resolutions are arranged in a pyramid-like hierarchy, with the full-resolution image at the base and progressively lower-resolution versions above it
- This allows the software to load only the resolution needed for a given zoom level, rather than loading the entire high-resolution image into memory
- In histology, this is especially useful because low-resolution views allow rapid navigation across the tissue, while high-resolution views allow detailed examination of cellular and tissue structures
- As shown in the figure, pyramidal images are typically organized as:
  - Level 0: Full-resolution image
  - Higher levels: Downsampled versions, such as 1/2, 1/4, or 1/8 scale
  
<img src="TutorialImages/pyramidalimage.jpg" alt="alt text" width="400" height="300">

- Common use cases include:
  - Digital pathology: .svs, .scn, .ndpi, .tiff
  - Spatial biology: .tiff, .ome.tiff
  - Geospatial imaging: GeoTIFF
- Pyramidal file formats offer several advantages for large-image analysis:
  - Faster loading and rendering
  - Lower memory usage during visualization
  - Efficient low-resolution previews
  - Region-of-interest extraction without decoding the entire image
- Limitations:
  - Pyramidal files can be larger because they store multiple resolutions
  - They often require specialized software for reading, writing, or processing
- Common File Formats:
  - TIFF-based formats such as .tiff and .ome.tiff
  - Vendor-specific formats such as .svs (Aperio), .ndpi (Hamamatsu), or .scn (Leica)
- These files can be read using tools such as:
  - OpenSlide
  - Bio-Formats
  - pyvips
  - tifffile
  - QuPath

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

## **2. Exercise steps**

## [**2.1 PART I**](#21-part-i)
 - [2.1.1 Install and launch QuPath](#211-install-and-launch-qupath)
 - [2.1.2 Create a QuPath project](#212-create-a-qupath-project)
 - [2.1.3 Add or remove images to your QuPath project](#213-add-or-remove-images-to-your-qupath-project)
 - [2.1.4 View and export images](#214-view-and-export-images)
 - [2.1.5 Set up image properties](#215-set-up-image-properties)
 - [2.1.6 Annotate images](#216-annotate-images)
 - [2.1.7 View and export annotation measurements](#217-view-and-export-annotation-measurements)

## [**2.2 PART II**](#22-part-ii-1)
 - [2.2.1 Create a QuPath project](#221-create-a-qupath-project)
 - [2.2.2 Annotate regions of interest](#222-annotate-the-region-of-interest)
 - [2.2.3 Perform cell detection](#223-perform-cell-detection)
 - [2.2.4 Annotate areas within the region of interest](#224-annotate-areas-within-the-region-of-interest)
 - [2.2.5 Create and run object classifier](#225-create-and-run-object-classifier)
 - [2.2.6 Perform positive cell detection](#226-perform-positive-cell-detection)
 - [2.2.7 Visualize data on the image](#227-visualize-data-on-the-image)
 - [2.2.8 View and export detection measurements](#228-view-and-export-detection-measurements)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

## **2.1 PART I**

### **2.1.1 Install and launch QuPath**
-------------------------------------

Please install the latest version from [here](https://qupath.github.io/)
Once installed, click on the QuPath icon, and wait for QuPath to launch.
When the QuPath window opens, you will notice the welcome screen, which links to QuPath documentation, the [image analysis forum](forum.image.sc), and also the [source code](https://github.com/qupath/qupath)

<img src="TutorialImages/qupathwelcomescreen.jpg" width="450" height="300">

### **2.1.2 Create a QuPath project**
-------------------------------------

Although it is possible to view and work with single images in QuPath, creating a "Project" makes saving and reloading data associated with multiple images much more efficient. A QuPath project groups related images to easily switch between them via thumbnails and also organizes associated data files, scripts, and classifiers.

  We start by creating a "project folder", which can be any folder, stored anywhere on your computer, but it **must be empty**.

  This can be done by doing either of the following:
  1. Click on `File`; then `Project` and `Create project`

  <img src="TutorialImages/createproject_1.png" width="300" height="360" style="margin-left: 40px;">

  2. Clicking on the `Create project` button in the analysis pane

  3. Dragging and dropping the project folder into QuPath

  You can name the project anything you like -- for this tutorial, I have named it `qupath_project`. Once the project is created, the project name will appear in the analysis pane.

  <img src="TutorialImages/createproject_3.png" width="270" height="100" style="margin-left: 40px;">

### **2.1.3 Add or remove images to your QuPath project**
-------------------------------------

#### **2.1.3.1 Add images**
  
  You can add images via `File --> Project --> Add images`. If you have not created a project and just want to open an image, you can also click on `File --> Open`. 
  Alternatively, you can drag and drop your images into QuPath -- this might be the more efficient option if you are working with a multiple images.
  
  <img src="TutorialImages/addimage_1.png" width="300" height="400" style="margin-left: 40px;">

  Let us start by importing the image `CMU-1.ndpi`. This is a hematoxylin-eosin stained section of skin scanned by Hamamatsu scanner.
  
  When you import the image, a dialog box will open where you can set parameters related to the image being imported, as shown below.

  <img src="TutorialImages/importimage.png" width="350" height="500" style="margin-left: 40px;">

  The image `CMU-1.ndpi` is a brightfield, H&E image, so we set the image type accordingly, and leave all other parameters unchanged, because we do not want to rotate the image right now, and there are no objects to import along with the image.
  
  
  Once the image(s) have been added to your project, you will see some folders created in your project folder.

  ```
  qupath_project
  ├── classifiers
  │   └── classes.json
  ├── data
  │   ├── 1
  │   │   ├── data.qpdata
  │   │   ├── server.json
  │   │   ├── summary.json
  │   │   └── thumbnail.jpg
  │   └── metadata.json
  ├── project.qpproj
  └── project.qpproj.backup

  ```

  <img src="TutorialImages/createproject_4.jpg" width="250" height="200" style="margin-left: 40px;">

#### **2.1.3.2 Remove images**

To remove an image, right-click on the image thumbnail in the analysis pane, and select `Remove image`. 

<img src="TutorialImages/removeimage_1.png" width="350" height="400" style="margin-left: 40px;">

⚠️ **Tip:** If your project has only once image, or if you have added only one image to your project and want to remove it, make sure to close the viewer by right-clicking anywhere in the viewer, and clicking on `Multi-view.. --> Close viewer`.

<img src="TutorialImages/removeimage_2.png" width="540" height="400" style="margin-left: 40px;">

⚠️ **Tip:** QuPath projects usually do not contain the images, but saves the path where your images are located. If you change the location of the images since creating the project, QuPath may have trouble finding the images when you re-open your project. QuPath does try to help by providing a dialogue box highlighting the paths that are missing and providing some suggestions of file paths where it thinks the images might be located. This dialogue box will also have a `Search…` button, with which you can set the new directory where the images are now located.
Clicking on `Apply changes` will update the project with the new file paths!


### **2.1.4 View and export images**
-------------------------------------

#### **2.1.4.1 Customize the viewer**

Once you have imported the image(s) to your project, you will see the name(s) of these image(s) in the analysis pane (red box), and the image in the image viewer.

<img src="TutorialImages/imageviewer_1.png" width="700" height="400" style="margin-left: 40px;">

In the viewer pane, you will notice the following:

1. Slide overview (green arrow)
2. X and Y co-ordinates and pixel values for the channels in the image (red arrow)
3. Scale bar (green box)

This is a good time to familiarize yourself with the options available under the `View` button. You will notice that there are options to toggle on or off the slide overview, analysis pane, scale bar and so on.

<img src="TutorialImages/imageviewer_2.png" width="200" height="700" style="margin-left: 40px;">


#### **2.1.4.2 Zooming and panning**

The **scroll wheel** of your mouse (or equivalent scrolling motion on a trackpad) can be used to **zoom in and out** of an image within QuPath.

Alternatively, you can click on `View` and `Zoom` to zoom in or out, or to zoom to specific magnifications (e.g. 400%)

<img src="TutorialImages/zoomimage.png" width="350" height="400" style="margin-left: 40px;">

⚠️ **Tip:** You can also navigate to a specific part of the WSI by clicking on that area in the slide overview window. 

You can pan the image with the move tool (red box) and clicking on the magnifying glass icon (green box) will fit the image to the viewer.

<img src="TutorialImages/panimage.png" width="900" height="50" style="margin-left: 40px;">


#### **2.1.4.3 Export images**

QuPath can also be handy for preparing figures for your publications or presentations. 
The `Export images` button under `File` provides several options for exporting an image.

<img src="TutorialImages/exportimage_1.png" width="400" height="450" style="margin-left: 40px;">

If you select `Original pixels`, you will be provided a list of available file formats for saving the image. 

<img src="TutorialImages/exportimage_2.png" width="500" height="200" style="margin-left: 40px;">

<img src="TutorialImages/exportimage_3.png" width="500" height="200" style="margin-left: 40px;">


Selecting `Rendered RGB (with overlays)` exports the image as it appears in the QuPath viewer, including overlays. Please remember that because the image is converted to RGB, the exported pixel values may not match the original data. This option is therefore suitable for preparing figures, but not for downstream quantitative analysis.

The `Export snapshot` button includes two additional options that allow more of the QuPath interface to be exported, such as toolbars and panels.

<img src="TutorialImages/exportimage_4.png" width="400" height="450" style="margin-left: 40px;">

⚠️ **Tip:** You can change the appearance of the scalebar by clicking `Edit --> Preferences` and changing the parameters for Scalebar under the Viewer tab. The screenshot below shows the default parameters and appearance of the scalebar when default parameters are used.

<img src="TutorialImages/imageviewer_4.png" width="600" height="500" style="margin-left: 40px;">

<img src="TutorialImages/imageviewer_3.png" width="200" height="200" style="margin-left: 40px;">

You can always change the Scalebar font size, font weight and thickness according to your preference, as shown below, and the changes will be reflected in the scalebar in the viewer.

<img src="TutorialImages/imageviewer_5.png" width="600" height="450" style="margin-left: 40px;">

<img src="TutorialImages/imageviewer_6.png" width="200" height="200" style="margin-left: 40px;">


### **2.1.5 Set up image properties**
-------------------------------------

#### **2.1.5.1 Set image type**

You will be prompted to set the image type, e.g. Brightfield (H-DAB), Brightfield (H&E), Fluorescence etc. when you add an image to a project.

You can always change this by clicking on `Image` in the analysis pane, and then `Image type`. The options for setting the image type will appear in a separate window.

<img src="TutorialImages/setimageproperties_1.png" width="650" height="500" style="margin-left: 40px;">

It is also important to correctly set the pixel size. QuPath does this automatically when the image is loaded, but you can also set this manually by clicking on `Pixel width` or `Pixel height` entering the desired values.

<img src="TutorialImages/setimageproperties_2.png" width="600" height="300" style="margin-left: 40px;">

#### **2.1.5.2 Separate stains**

The first step to separating the stains in the image is to set the image type accurately, as described in the section above. For brightfield images, QuPath separates the stains by using [color deconvolution method introduced by Ruifrok and Johnston](https://pubmed.ncbi.nlm.nih.gov/11531144/). Please make sure to check out the [QuPath docs](https://qupath.readthedocs.io/en/stable/docs/tutorials/separating_stains.html) to learn more about color deconvolution in QuPath.

We will next follow the steps described below to set up the stain vectors. This is an important step because if the stain vectors are inaccurate, steps that rely on color deconvolution, such as cell detection, may perform poorly because the signals from different stains may not be properly separated.

1. Since you set the image type as Brightfield (H&E), you would be able to see the names of the stains, Hematoxylin and Eosin, under the `Image` tab.

<img src="TutorialImages/setimageproperties_3.png" width="350" height="100" style="margin-left: 40px;">

2. You can also visualize the different stains in your image individually by clicking `View --> Brightness and Contrast`.

<img src="TutorialImages/setimageproperties_4.png" width="550" height="700" style="margin-left: 40px;">

The `Brightness and Contrast` button provides many helpful options,e.g.,

*Viewing one stain at a time*

<img src="TutorialImages/setimageproperties_5.png" width="700" height="700" style="margin-left: 40px;">

<img src="TutorialImages/setimageproperties_6.png" width="700" height="700" style="margin-left: 40px;">

*Viewing the image in grayscale*

<img src="TutorialImages/setimageproperties_7.png" width="700" height="700" style="margin-left: 40px;">

*Inverting the colors of the image*

<img src="TutorialImages/setimageproperties_8.png" width="700" height="700" style="margin-left: 40px;">

3. Next, we annotate an area of the image that has clear examples of the stains that we want to separate, along with an area of the background. For this, we use the rectangle annotation tool (red box).

<img src="TutorialImages/setimageproperties_9.png" width="700" height="700" style="margin-left: 40px;">

We then click on `Analyze --> Estimate stain vectors` and then `Yes`

<img src="TutorialImages/setimageproperties_10.png" width="700" height="700" style="margin-left: 40px;">

QuPath will then show the 3D relationship between the red, green, and blue pixels in the image as 2D scatterplots, and also indicates the existing stain vectors with colored lines.

<img src="TutorialImages/setimageproperties_11.png" width="600" height="600" style="margin-left: 40px;">

These scatterplots help assess whether the stain vectors match the pixel colors in the selected region. Ideally, the vectors should closely align with most of the scattered points.

If the vectors are not well aligned, the `Auto` option can estimate improved stain vectors from the selected region. However, unexpected colors, such as greenish pixels, can distort the estimate. In such cases, adjusting parameters like `Exclude unrecognised colors (H&E only)` can help remove irrelevant colors and produce more reliable stain separation.


### **2.1.6 Annotate images**
-------------------------------------

Annotations in QuPath can have many applications. Common use cases include measuring lengths or areas, defining specific regions where analysis should be applied, such as cell detection, as well as selecting representative areas for training a classifier. For this tutorial, we will learn to annotate some regions interest (ROI) in an image and perform and export measurements from these ROIs.


#### **2.1.6.1 Draw annotations**

**Annotation objects** can be created by drawing specific shapes on the image, using the (1) rectangle, (2) ellipse, (3) line, (4) polygon, or (5) polyline tools. The (6) brush tool and the (7) wand tool are particularly handy for drawing custom shapes. All of these tools can be applied by selecting the tool, clicking on the image, and dragging the mouse. For the polygon tool, the shape can be completed by releasing the mouse or by double-clicking where the final point should be.

<img src="TutorialImages/annotation_1.png" width="400" height="50" style="margin-left: 40px;">

⚠️ **Tip:** The wand tool is very helpful for making precise annotations around tissue boundaries.

The (9) selection tool converts the annotation tools into tools for selection. This is helpful for selecting multiple annotated regions with a single click and drag. Clicking on the selection tool changes the annotation tool icons into their dashed versions.

<img src="TutorialImages/annotation_2.png" width="400" height="35" style="margin-left: 40px;">

Annotations can be deleted by selecting the annotation and pressing `Delete` on the keyboard, or right-clicking on the annotation and selecting `Delete object`.
If you make a mistake while annotating with the brush or wand tool, simple press `Option` and click and drag your mouse to erase the incorrectly annotated area. 
Right-clicking on an annotation "locks" it, and prevents it from getting accidentally deleted. 

Make sure to explore some of these annotation tools and draw different shapes and sizes on the image!

#### **2.1.6.2 Set annotation properties**

QuPath allows you to make annotations of different classes - these classes depend on the image you are working with and your biological question. For example, if you are working with images of kidney tissue, these classes can be different structures you have annotated in the image and want to analyze further, e.g. glomerulus, tubules etc. Or, if you are working with images of diseased tissue, these can be tumor area / abnormal area or normal / healthy area and so on. 

We will now create some annotations of a particular class and set the properties of these annotations.

1. Create an annotation class by clicking on the three dots next to `Auto set` under the `Annotations` tab in the Analysis pane. Then click `Add/Remove --> Add class`. A window will open where you can enter the name of the annotation class; we will enter Squamous_Epithelium.

<img src="TutorialImages/annotation_3.png" width="500" height="250" style="margin-left: 40px;">


<img src="TutorialImages/annotation_4.png" width="300" height="150" style="margin-left: 40px;">


2. This new class of annotation will appear in the analysis pane. If you like, you can double-click on the annotation and select a different color for your annotation by clicking on the drop-down -- I have changed the annotation color to Lime.

<img src="TutorialImages/annotation_5.png" width="550" height="300" style="margin-left: 40px;">

<img src="TutorialImages/annotation_6.png" width="250" height="150" style="margin-left: 40px;">

3. We will now draw some annotations on the squamous epithelial lining of the tissue. You can use any tool to do this; I have used the brush tool. You will see the list of annotations appear in the analysis pane. We will now assign a class to these annotations.

<img src="TutorialImages/annotation_7.png" width="1000" height="600" style="margin-left: 40px;">

4. Double-click on any annotation -- you will see the outline color change to yellow, which means you have now selected the annotation. 
If you now right-click on the annotation, you will have the option to assign it a class. Click on `Set classification --> Squamous_Epithelium`. You can do this one-by-one for each annotation. You will see now the thumbnails of the annotations will have a class next to them, and the class Sqaumous_Epithelium has a number next to it indicating the number of annotations that exist in this class.

<img src="TutorialImages/annotation_9.png" width="500" height="300" style="margin-left: 40px;">

<img src="TutorialImages/annotation_10.png" width="1000" height="500" style="margin-left: 40px;">


⚠️ **Tip:** To assign a class to multiple annotations in one step, select all the annotation thumbnails in the analysis pane. This will select all the annotations in the viewer. You can then right-click on any annotation and set the classification as described above.

5. Now we will set the properties for each annotation, i.e., assign a unique label to each annotated area of the class Squamous_Epithelium.
   
   To assign labels, we double-click on each annotation, select `Annotations --> Set properties` and enter a name, You can also add a description if you like. I have named the annotations S_E_1 (abbreviation for Squamous_Epithelium) through S_E_5. You will notice that the annotation labels now appear next to the annotation thumbnails in the analysis pane, and you can also visualize these labels in the viewer.

<img src="TutorialImages/annotation_13.png" width="400" height="300" style="margin-left: 40px;">


<img src="TutorialImages/annotation_11.png" width="400" height="300" style="margin-left: 40px;">


<img src="TutorialImages/annotation_14.png" width="1000" height="400" style="margin-left: 40px;">


    Why do we do this step?

    If we do not assign unique labels to each annotation, when we perform measurements and export the data, we will not be able to tell exactly which annotation the measurements belong to. This is because, the exported measurement table will look like the figure below, where each row has the measurements for each annotation, but without labels, so we cannot look back at the image and map the measurements back to the annotations.


<img src="TutorialImages/annotation_15.png" width="1000" height="400" style="margin-left: 40px;">

    However, if we assign unique labels to the annotations, the exported table will include these annotation labels, as you can see in the figure below, and these labels can also be visualized in the viewer. This allows us to precisely map the measurements back to the annotations in the image.

<img src="TutorialImages/annotation_16.png" width="1000" height="400" style="margin-left: 40px;">


  Now try making annotations of a different class with different names as shown in the figure below!
  
  ⚠️ **Hint:** You will pretty much use the same steps as described above!

<img src="TutorialImages/annotation_17.png" width="950" height="400" style="margin-left: 40px;">


### **2.1.7 View and export annotation measurements**

For every annotation created on the image, QuPath performs some measurements such as area, perimeter, X and Y co-ordinates.
There are several ways of viewing these measurements.
- If you click on the thumbnail of any annotation under the Annotations tab, you will see the measurements that QuPath has made for that annotation will populate the table below.

<img src="TutorialImages/annotation_18.png" width="400" height="600" style="margin-left: 40px;">

- You can also view the per-annotation measurement table by clicking on `Measure --> Show annotation measurements` 

<img src="TutorialImages/measurement_1.png" width="200" height="200" style="margin-left: 40px;">


<img src="TutorialImages/measurement_2.png" width="900" height="400" style="margin-left: 40px;">

To export the measurements, you can click on `Save` in the measurement table. This will allow you to export this table as a `.txt` file.


Alternatively, you can click on `Measure --> Export measurements`. But before you do this, make sure you have saved the changes by clicking `File --> Save`.

<img src="TutorialImages/measurement_3.png" width="200" height="200" style="margin-left: 40px;">

This will open a window where you will need to set the following:
- Choose the image for which you want to export the measurement. In this tutorial, we have only one image, and select this by clicking on the arrow (red box)

<img src="TutorialImages/measurement_4.png" width="500" height="400" style="margin-left: 40px;">

You will see the selected image move to the box on the left.

<img src="TutorialImages/measurement_5.png" width="500" height="400" style="margin-left: 40px;">

- Click on `Choose` to set up the name of the file and the location where the file should be saved.

- Select the type of measurements you want to export, i.e., image, annotations, detections etc. We want to export the annotation measurements.

<img src="TutorialImages/measurement_6.png" width="500" height="400" style="margin-left: 40px;">

- Select whether you want the measurements exported to a `.csv` or `.tsv` file. Let us export the measurement to a `.csv` file.

<img src="TutorialImages/measurement_7.png" width="500" height="400" style="margin-left: 40px;">

- Then, click `Export`. QuPath will export the data into a `.csv` having the name that you set and the location that you selected. For this tutorial with the annotations I made, the file looks like this opened in Excel.

<img src="TutorialImages/measurement_8.png" width="1500" height="200" style="margin-left: 40px;">

If you would want to get a bit more practice with steps 2.4 to 2.7, feel free to download a couple more H&E images from [here](https://openslide.cs.cmu.edu/download/openslide-testdata/Hamamatsu/) and add to your current project!
-------------------------------

## **2.2 PART II**

For this part of the tutorial, we will learn how to detect positive cells, meaning cells that show staining, by analyzing a tissue section immunohistochemically stained with a nuclear marker. Before detecting positive cells, we will also annotate specific regions of interest within the tissue section, allowing us to link individual cell measurements back to the tissue regions where those cells are located.
We will be working with the `OS-2.ndpi` image.

### 2.2.1 Create a QuPath project
-------------------------------------
We will create a new QuPath project and add the `OS-2.ndpi` image to the project, following steps detailed in 2.1.2 and 2.1.3. I have named the project `qupath_project_IHC`; you can name the project according to your preference. We will set the image type as `Brightfield (H-DAB)`.

### 2.2.2 Annotate the region of interest
-------------------------------------
Let us first create a new class of annotations called `ROI` (region of interest), following steps detailed in 2.1.6. 
Next, using the wand tool (or any annotation tool of your preference), let us annotate a specific part of the tissue. This is the part of the tissue where we want to detect the cells.

<img src="TutorialImages/annotation_ROI_1.png" width="550" height="300" style="margin-left: 40px;">


### 2.2.3 Perform cell detection
-------------------------------------
We will now detect all the cells in `ROI` to perform measurements on these cells, and also to later classify these cells as positive or negative for the nuclear marker.

1. Select the annotation `ROI`; the color of the outline will change to yellow.
2. Click on `Analyze --> Cell detection --> Cell detection`

<img src="TutorialImages/cell_detection_1.png" width="300" height="200" style="margin-left: 40px;">

3. This will open a window where you can set up cell detection parameters, e.g. setting the minimum and maximum area of the cells, intensity threshold, and so on. Also, note that QuPath detects cell outlines by expanding the nucleus outlines by a fixed distance; this can also be set at this step.

<img src="TutorialImages/cell_detection_2.png" width="350" height="500" style="margin-left: 40px;">

For the purpose of the tutorial, we will use the default parameters and hit `Run`, and QuPath will start detecting the cells.

<img src="TutorialImages/cell_detection_3.png" width="350" height="200" style="margin-left: 40px;">

When QuPath finishes, the cell (detections) outlines will appear on the image within the annotated area (`ROI`) where the detection was run, and the number of detections will be updated in the analysis pane next to the annotation thumbnail.

<img src="TutorialImages/cell_detection_4.png" width="350" height="200" style="margin-left: 40px;">




### 2.2.4 Annotate areas within the region of interest
-------------------------------------
If you click on any of the detected cells, you will see in the analysis pane that the `Parent` of this cell (detection) object is `Annotation (ROI)`
Now, let's say, we are interested in comparing features of cells that are located in two specific areas within this `ROI`, e.g. `region_1` and `region_2`. In that case, we would want the detected cells to also have these additional annotation labels, i.e `region_1` and `region_2`.

To achieve that, we will perform the following steps:

1. Lock the annotation `ROI`. Right-click on the annotated area, select `Annotations --> Lock`

<img src="TutorialImages/annotation_ROI_2.png" width="550" height="450" style="margin-left: 40px;">

2. We now annotate the addtional areas of interest within `ROI`. I used the rectangle annotation tool, but any tool can be used.

<img src="TutorialImages/annotation_ROI_3.png" width="550" height="450" style="margin-left: 40px;">


3. To create annotations of the exact same size, I selected the first annotation, and clicked on `Objects --> Annotations --> Duplicate selected annotations`

<img src="TutorialImages/annotation_ROI_4.png" width="450" height="450" style="margin-left: 40px;">

4. The duplicated annotation will appear on top of the original annotation. Use the move tool to drag it to the position where you actually want it placed.

<img src="TutorialImages/annotation_ROI_5.png" width="450" height="450" style="margin-left: 40px;">


<img src="TutorialImages/annotation_ROI_6.png" width="450" height="450" style="margin-left: 40px;">

5. Name these annotations `region_1` and `region_2`, by selecting each annotation, right-clicking and the selecting `Annotations --> Set properties`. Feel free to use any name that you like. In your real analysis, this will be guided by your research question.


6. Now, we need to let QuPath know that `region_1` and `region_2` belong to our first annotation `ROI`. This is called inserting objects into a hierarchy, where annotation objetc `ROI` will be the root or parent, and annotation objects `region_1` and `region_2` will be the children.
To do this, select the thumbnails for `region_1` and `region_2` in the analysis pane (shift + down arrow keys), and then `Insert in hierarchy`

<img src="TutorialImages/object_hierarchy_1.png" width="450" height="300" style="margin-left: 40px;">

The analysis pane will update to indicate that the root annotation object `ROI` has two chidren objects.


<img src="TutorialImages/object_hierarchy_2.png" width="200" height="250" style="margin-left: 40px;">

7. Lock the annotations `region_1` and `region_2`


We will click on `Analyze --> Cell detection --> Positive Cell detection`. 

<img src="TutorialImages/cell_detection_1.jpg">

This opens up a window where we can set the parameters that will allow for accurate cell detection. Once the appropriate parameters are set, we can hit `Run`, and the cells will be detected.
<img src="TutorialImages/cell_detection_2.jpg">

The Annotations pane will show the count of all cells, positive cells and negative cells that were detected.
<img src="TutorialImages/cell_detection_3.jpg">

We can also visualize the detections as a heatmap overlaid on the image by clicking `Measure --> Show measurement maps`
<img src="TutorialImages/cell_detection_4.jpg">



### 2.2.5 Create and run object classifier
-------------------------------------


### 2.2.6 Perform positive cell detection
-------------------------------------
We can next assign the positive cells that were detected to specific areas in the tissue, e.g. tumor or stroma. This can help us compare the counts or other characteristics of the positive cells in the tumoral or the stromal regions.
For this, we first annotate some regions within our region of interest as 'tumor' or 'stroma', and train the classifier `Classify --> Object classification --> Train object classifier`
<img src="TutorialImages/classification_1.jpg">

We will now get the measurement of the positive cells stratified into the classes (tumor/stroma) that we set.
<img src="TutorialImages/classification_2.jpg">


### 2.2.7 Visualize data on the image
-------------------------------------





### 2.2.8 View and export detection measurements
-------------------------------------
QuPath makes some basic measuerements of the annotations and detections by default if no additional instruction is provided. However, more granular or more advanced measurements can also be made by clicking on `Analyze --> Calculate features` and then selecting the features that you would want to extract.
<img src="TutorialImages/measurement_1.jpg">

These measurements can also be viewed as a heatmap overlaid on the image by clicking `Measure --> Show measurement maps` or as a table by clicking `Measure --> Show detection measurements`
<img src="TutorialImages/measurement_2.jpg">

<img src="TutorialImages/measurement_3.jpg">

Finally, we export the measurements, by clicking `Measure --> Export measurements` and selecting the measurements we want to export (e.g. measurements for all images / some images, annotation measurements, detection measurements) and also the file format in which we would want the measurements saved (e.g., .csv, .tsv)

<img src="TutorialImages/export_1.jpg">

You will see the file with the measurements saved in your QuPath project folder.

<img src="TutorialImages/export_2.jpg">

--------------------------------------------------------------------------------------------
--------------------------------------------------------------------------------------------

## Congrats! Now you know the basics of QuPath!


## Learn more about QuPath:
- [QuPath Repo](https://github.com/qupath)
- [QuPath docs](https://qupath.readthedocs.io/en/stable/)
- [QuPath YouTube Channel](https://www.youtube.com/@petebankhead/videos)

---