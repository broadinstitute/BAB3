# Day 1, Exercise 5 - Making manual annotations with Piximi

*Lab authors: Beth Cimini, Kate Bowers* . 

---

## Learning Objectives
- Practice making manual annotations in multichannel images in Piximi
- Turn images into measurements and graphs

**Lab Data** in [this folder](https://drive.google.com/drive/folders/1TyV_hzN2pDpX0lVQ_jh08A9AG5e4Bao8?usp=drive_link)

---

While [Piximi](https://piximi.app/){cite}`Moser2024-vt` can do a lot of cool things, today we'll only be using two of its functionalities - manual annotation and measurement. 

We'll be using a dataset of microglia stained in rat brains - you can read more of the specifics in the [dataset readme](https://drive.google.com/file/d/1wdz7pikmfWAEXUeGQ3VcDn1rwnqKyYYm/view?usp=sharing).

## Exercise part 1 - manually annotate some nuclei

```{margin} Want to learn more about working with Piximi?
Check out the [documentation](https://documentation.piximi.app/intro.html) (which includes some full-length tutorials), or check them out on [the image.sc forum!](https://forum.image.sc/tag/piximi)
```

- Download the dataset linked above to your local machine. We'll be using it throughout the week, so don't delete it!
- Open Piximi.app, and click "Start New Project"
- Drag the images in, and tell Piximi that they have 2 channels

<img src="./images/Channels.png">



- The images should now all be visible in Piximi's main experiment view - hit "Annotate" to bring them into the Annotator View (also called the Image View)
<img src="./images/AnnotationButton.png">

```{tip}
Running into trouble? Toggle on Piximi's help mode on the left bar, which will help you figure out how to use each feature!

<img src="./images/HelpMode.png" width="150%">
```

- Hit the `+` button next to the word Kinds to make a new object kind - you'll probably want to call it Nuclei or something like that.

<img src="./images/NewKind.png">

  - Optionally - if you'd like to separate your nuclei into categories (such as "microglial nuclei" or even just "big nuclei" vs "small nuclei"), you can do this in one of two ways: 
    - In the menu under Nuclei, you can make as many categories as you like, and if a category is selected while you are annotating, the object will be automatically assigned to that category
    - Once you've made and saved your annotations, you can see the objects you've made back in the Experiment view - you can add (or change!) categories for as many objects as you would like 
    <img src="./images/ExperimentViewCategorization.png">

- Time to play! Piximi has several kinds of annotation, [which you can learn more about here](https://documentation.piximi.app/pages/detail/imageviewer.html#annotation-tools) and see in action [here](https://documentation.piximi.app/pages/detail/imageviewer-tools-annotation.html). Which works well for you? Are some better for certain "classes" of nuclei?

```{tip}
Want to change the colors or adjust the brightness contrast? Piximi [can do that](https://documentation.piximi.app/pages/detail/imageviewer.html#image-tools)!
```
- When you are done annotating (and/or want to change some categories for some annotations), hit the back arrow to return to the main view.
<img src="./images/SaveAndReturn.png">

- Make sure to save your project file for future use!

-----

## Exercise part 2 - making some measurements

Rarely do we want stop at just finding objects and categorizing them - often, we want to know things about them. 

- Follow Piximi's [Measurements tutorial](https://documentation.piximi.app/pages/tutorial/creating-measurements.html) to learn how to create and export measurements for your identified nuclei.
  - If you've created categories (or want to do so now), you can see how your measurements differ by categories as well.

---


## **Bonus Exercise - annotating some cells**

- Try to annotate some cells - in the annotator view, hit `+` to create a new "Kind" called cells

<img src="./images/NewKind.png" width="100%">

- Annotate some cells, trying a few different Piximi functionalities for annotation - do you find the same tools optimal 

---


<!---
```{admonition} Question
:class: important

Questions to ask
 ```
 --->