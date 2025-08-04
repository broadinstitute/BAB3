# Day 1, Exercise 3: developing image processing intutions

*Lab authors: Beth Cimini* 

---

## Learning Objectives
- Learn about filters available to you for classical image processing
- Learn to chain filters together

---

Much of classical image processing rests on the basis of using filters to set the thing you care about in an image to bright, and everything else in the image to dark (many kinds of deep learning rest on this too, at least in part, they just have very fancy filters they can compose for doing it!). 

Even if you use deep learning for much of what you do going forward, you may come across situations where a bit of image enhancement helps you identify a structure of interest better, so these are always excellent tools to have in your image analysis "toolbox"!

## Exercise 1

Given a list of filters (that we'll first discuss and demonstrate) and a set of images, let's leave the computers alone for a while and just think through our options. What do we expect will work? What images do we think will be easy, and what images will be hard?

## Exercise 2

Use the interactive notebook below to see if your intuitions were good! 


```{admonition} Question
:class: important

Look at the list of [CellProfiler Image Processing modules](https://cellprofiler-manual.s3.amazonaws.com/CellProfiler-4.2.8/modules/imageprocessing.html). Are there any functions you wish you could access but aren't implemented? What are they, and why do you think they would help?
 ```
<iframe
src="https://ciminilab.github.io/developing-intuitions/apps/Bioimage_Analysis_Intutions.html?embed=true&show-chrome=false"
width=150%
height=1500
></iframe>