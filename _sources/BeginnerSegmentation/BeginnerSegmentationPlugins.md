# Day 4, Session 2 - Interfacing CellProfiler with other software tools via files and plugins

*Lab authors: Beth Cimini*. 

---

## Learning Objectives
- Learn how to download CellProfiler plugins from GitHub and set CellProfiler to use them
- Learn more about how to use software containers (such as Docker or Podman) to help CellProfiler access other tools
- Use CellProfiler from a pixi environment, speeding up access to Cellpose

**Lab Data** in [this folder](https://github.com/CellProfiler/tutorials/raw/master/BeginnerSegmentation/bonus_materials_EN.zip), as well as the images from the Intro to CellProfiler session on Tuesday (which you hopefully still have, but is [linked here](https://github.com/CellProfiler/tutorials/raw/master/BeginnerSegmentation/Archive_EN.zip) if not)

---

## **Setup steps to do first**

1. Download at least one RunCellpose container and one ilastik container from Dockerhub.
This will happen automatically the first time you call a given Docker from CellProfiler (i.e. run a CellProfiler pipeline that uses the Docker) but if you are running this tutorial in a workshop setting we strongly recommend download in advance of the workshop as these are large files (5-10 GB) and bandwidth is often limited in a workshop setting.
In Docker Desktop or Podman Desktop you can search for containers in the top search bar (see below).
Make sure you select a tag (version) that is supported by the plugin you are using and then select "Pull".
We recommend `biocontainers/ilastik:1.4.0_cv2` for ilastik and `cellprofiler/runcellpose_with_pretrained:3.1.2.2` for Cellpose.

```{figure} ./TutorialImages/DockerSearch.png
:width: 400
:align: center

Search in Docker Desktop for your desired container
```

2. Once your containers are downloading, either 
2a. Clone (i.e. download whole) the Cellprofiler-plugins repository.
In your terminal type `git clone https://github.com/CellProfiler/CellProfiler-plugins.git`. OR 
2b. Visit the [CellProfiler plugins repository](https://github.com/CellProfiler/CellProfiler-plugins) and then download and unzip all the plugins by hitting the green `Clone` button and then `Download Zip`
OR 
2c.Alternatively, download just the [Runilastik plugin](https://github.com/CellProfiler/CellProfiler-plugins/blob/master/active_plugins/runilastik.py) and [RunCellpose plugin](https://github.com/CellProfiler/CellProfiler-plugins/blob/master/active_plugins/runcellpose.py) by selecting the "Download raw file" button.

```{figure} ./TutorialImages/GitHubDownloadButton.png
:width: 400
:align: center

GitHub's "Download Raw Files" button
```

3. In CellProfiler's File -> Preferences menu (CellProfiler -> Preferences in Mac), set the `CellProfiler plugins directory` to the folder containing the plugins.
4. Close and reopen CellProfiler to load the plugins.

## **Background information**

### General background information

This exercise is meant to extend and build upon our old friend, the Beginner Segmentation exercise available from Tuesday morning. Please consult that tutorial for general information on how to configure CellProfiler as well as information about the images, if you want a refresher. It will be generally assumed you understand the modules covered in that tutorial, including input, object creation, overlays, and saving. 

### What is this exercise?

While CellProfiler is an image analysis tool itself, it also serves as a workflow manager that can interact with other tools, automatically passing data back and forth so that you can effectively use other tools within a single CellProfiler pipeline.
This means in a hypothetical pipeline you have have CellProfiler do steps 1 and 2, Tool A do step 3, Tool B do step 4, and CellProfiler do step 5 all while configuring Tools A and B within CellProfiler and without having to handle data import or export from other tools.
In this tutorial, you'll explore 3 different ways of accessing work done in other tools:

1. Loading masks created by other segmentation tools (in this case, [Cellpose](https://www.cellpose.org/), but the same strategy works for many tools which use this format).
1. Accessing [ilastik](https://www.ilastik.org/) to use a trained pixel classification model via CellProfiler's plugins system.
1. Running Cellpose in CellProfiler via CellProfiler's plugins system and a Docker container.
1. Running Cellpose and CellProfiler in the same Python environment

### Plugins

CellProfiler has a number of plugins that act like modules within a pipeline.
Plugins are available [on Github](https://github.com/CellProfiler/CellProfiler-plugins).
Read more about them in [CellProfiler plugins documentation](plugins.cellprofiler.org) to learn more.
To quote from that site:

>Plugins advance the capabilities of CellProfiler but are not officially supported in the same way as modules. A module may be in CellProfiler-plugins instead of CellProfiler itself because:
>- it is under active development
>- it has a niche audience
>- it is not documented to CellProfiler’s standards
>- it only works with certain versions of CellProfiler
>- it requires extra libraries or other dependencies we are unable or unwilling to require for CellProfiler
>- it has been contributed by a community member

````{tip}
While that documentation has instructions for [installing plugins](https://plugins.cellprofiler.org/using_plugins.html#installing-plugins-without-dependencies), in step 2 it suggests downloading all of the CellProfiler plugins; this is generally a good thing to do, but if you prefer you can download individual plugins from GitHub with the website button below as well or instead.
````

```{tip}
We strongly recommend making a dedicated folder to store your CellProfiler plugins, as loading can be slow if there are a lot of other miscellaneous files around (such if they sit in the Downloads folder, for example).
```

### Docker Desktop and Podman Desktop

In Exercise 3 (and for Windows users, optionally Exercise 2 as well), we want to access software that we either don't want to install locally because it's painful if you're not pretty comfortable in Python (Cellpose) or that we CAN install locally but may not play nicely with other tools' multiprocessing setup (ilastik). 

Computer scientists will often use software **containers** to ship tools or data that are hard to install - [here](https://journals.sagepub.com/doi/10.1177/25152459211017853) is a good introduction and overview for non-computer scientists. You can think of containers as "a pre-configured operating system in a box". Because they come to you pre-configured, installation of any software happens once-and-only-once (by the creator of the container, not by you), and should stay working for long after ie the latest Mac upgrade breaks a certain older software installation. Groups like [biocontainers](https://biocontainers.pro/) have already containerized many of the tools you know and love. There are a number of types of software containers, but one of the most common is called a **Docker** container; many containers made by Docker, which are therefore sometimes referred to as Docker containers, can nevertheless be run in other container programs, like Singularity and Podman. 

Most life scientists aren't aware of or don't use containers, especially because typical usage involves accessing them via your terminal. But Docker doesn't have to mean terminal! There are [containers that spit out interactive websites for you to use](https://github.com/COBA-NIH/docker_gradio_demo), and CellProfiler has plugins that are specifically set up to call other tools that live (and are executed) inside containers. 

To do this, CellProfiler needs to have the infrastructure for Docker containers up and running, which you can give it by installing a free program like [**Docker Desktop**](https://www.docker.com/products/docker-desktop/) or [**Podman Desktop**](https://podman-desktop.io/downloads). You don't need to make an account to use it, but you probably will need to reboot your computer after installation is complete. Then, whenever you want to use a container-calling plugin in CellProfiler, you simply must make sure that one of these programs is open and running, CellProfiler will take care of everything else!

```{figure} ./TutorialImages/DockerDesktop.png
:width: 700
:align: center

The Docker Desktop interface for Mac. Containers can be searched for in the top search bar!
```

```{warning}
Anytime you're installing a program that can run other programs, you must of course be careful. Containers give you access to a huge variety of tools that might otherwise be hard to install (or install together), but **exercise caution in only running containers you recognize or trust**!
```

## **Exercise 1: Importing masks from another segmentation tool**

In this exercise, we will be loading in *label masks* (sometimes also called *label matrices*) made in Cellpose. Simply, a label mask is an image in which all the background pixels have a value of 0, all the pixels of object 1 have a value of 1, all the pixels of object 2 have a value of 2, etc. It is a commonly used format by many tools that do object segmentation (though unsuitable for applications where you have overlapping objects). Since many tools (including CellProfiler) can make these masks, CellProfiler has the ability to read them in and automatically detect them as objects.

We've used Cellpose 2.2.2 to generate nuclear masks from the DNA images and cell masks from the ActinGolgi images. 

### Only if you are curious - how did we make these label masks?
Cellpose was installed in a conda environment according to the [official instructions](https://github.com/MouseLand/cellpose?tab=readme-ov-file#installation) for installation with the GUI. The software was started in that conda environment using the command `cellpose`.

Nuclear masks were made by dragging each DNA image into the GUI, selecting the 'nuclei' model with default settings, and then saving them individually as PNG using *Cmd+N* (Mac) *Ctl+N* (Windows). Since there were only 10 images and the hotkeys were available, this was not too time-consuming to do.

```{figure} ./TutorialImages/CellposeGUI.png
:width: 700
:align: center

Nuclei segmented in the Cellpose GUI
```

Cell masks were made by navigating into the image folder and then running the command below; this requires some knowledge of the [Cellpose command line syntax](https://cellpose.readthedocs.io/en/latest/cli.html) but is much easier for large numbers of images.

`python -m cellpose --dir . --img_filter Ch4 --pretrained_model cyto2 --diameter 40 --save_png --verbose`

```{figure} ./TutorialImages/CellposeCLI.png
:width: 700
:align: center

 Command line execution of Cellpose
 ```

### Loading the label masks into CellProfiler

1. Open up a clean copy of CellProfiler (or run File -> New Project) and drag `bonus_1_import_masks.cppipe` into the pipeline panel.
1. Drag the `images_Illum-corrected` subfolder from the main Beginner Segmentation exercise and also the `cellpose_masks_cells` subfolder from this exercise. *Do not drag in the `cellpose_masks_nuclei` folder.*
1. Put CellProfiler into TestMode <img src="./TutorialImages/startTestMode.png" width="120"/>, open the eye icon <img src="./TutorialImages/EyeOpen.png" width="50"/> next to OverlayOutlines, and then hit <img src="./TutorialImages/Step.png" width="120"/> 3 times to create a classical segmentation and compare it with the Cellpose-generated version. You can check the settings in the OverlayOutlines module to see which outline color corresponds to which segmentation in the output.
1. Optionally, open the Workspace Viewer using <img src="./TutorialImages/ViewWorkspace.png" width="120"/> to create easily on-the-fly customizable overlays.
1. Hit <img src="./TutorialImages/NextImageSet.png" width="120"/> and repeat a couple of times to examine the segmentations on more images.

```{admonition} Question for you
Are there cases where you think classical segmentation typically performs better? Where Cellpose performs better?
```

```{figure} ./TutorialImages/WorkspaceViewerImportedMasks.png
:width: 700
:align: center

Visualization of images and objects in the Workspace Viewer
```
### Reverse engineer how this worked
1. Open the NamesAndTypes module. 
1. Scroll down in NamesAndTypes to find the entry that adds the masks - is there any setting you notice about it that is different than for the other images?
1. Find the entry in NamesAndTypes that has 2 rules the image has to pass, not just one - do you understand why that is?

### Bonus exercise (for later if you feel exceptionally good on time) - add the provided nuclear segmentations as well
1. Drag and drop the `cellpose_masks_nuclei` folder in the Images module to add the nuclear masks to the image list.
1. Go to the NamesAndTypes module and configure it so both kinds of masks can be loaded (hint: there's a duplicate button you can use to make a second copy of a channel!)
  - You'll probably have to modify the existing settings for loading the cell masks by changing its rule or adding a second rule. Do you understand how/why?

## **Exercise 2: Running ilastik locally or from a container**

As you are well aware of by now, classical segmentation requires the thing you care about in an image be bright and every single other pixel dark(er). If you have a good clean fluorescent signal for the thing you care about, great! If not, you may need to resort to some tricks.

One such trick is to train a classifier on a pixel-by-pixel basis to say "here is what I think is the likelihood that this is a pixel you want to end up in your segmentation"; this classifier then ends up creating for each  pixel a *probability value* that corresponds to how likely it thinks it is you want to segment that pixel. If your classifier is good, that will give you an image where the pixels you care about are high-probability (bright) and everywhere else is low-probability (dark). That's exactly what we want! Life scientists tend to call this **"Pixel Classification"**; computer scientists will sometimes refer to it as **"Semantic Segmentation"**.

There are a few popular Fiji plugins for doing this, including Weka Trainable Segmentation and Labkit, and you should absolutely use them if they work better for you! We tend to use ilastik, because it makes it easy to automate creating a classifier from a very small number of images and then bulk-applying it to many others in "Batch Processing" mode. Earlier in the course we demonstrated running ilastik, and *then* CellProfiler. 

```{figure} ./TutorialImages/IlastikBatchMode.png
:width: 700
:align: center

ilastik's Batch Processing mode
```

Why do two steps though, when you can do one instead? In this tutorial we'll take a pre-trained ilastik classifer and run it inside our CellProfiler pipeline, so we can find and measure objects all in one step.

You will need either ilastik or Docker/Podman Desktop installed on your computer for this exercise. If ilastik, make sure it is CLOSED, if Docker/Podman Desktop, make sure it is OPEN. We recommend installing ilastik because it is a good and helpful tool, and will allow you to do this exercise's bonus challenge.

```{warning}
If you are on Windows, execute RunIlastik in Local mode (working on an installed copy of ilastik, rather than a copy inside a Docker file), this exercise will work in TestMode, but not will not run in analysis mode - we are working with the ilastik developers to determine why that is. This is fine for the purpose of this exercise; if you have a lot of your own data you want to run later, you can still use ilastik in a two step process, and/or use containerized ilastik.
```

### Only if you are curious - how did we train the classifier?

This classifer was made in ilastik 1.4.1b5 by training on 4 images (A14_site1, E18_site1, D16_site1, and C12_site1) in which we labeled just two classes: one class for nucleoli (yellow in the figure below), and one class for every other part of the image (blue in the imagebelow). One COULD have made more classes, but this, in practice, worked.

```{figure} ./TutorialImages/IlastikLiveUpdate.png
:width: 700
:align: center

Screenshot of this ilastik classifer
```

When using ilastik for fluorescence microscopy, you will likely get the best performance if you **keep your annotations extremely minimal** - the classifier you're going to use was trained by using 31 total pixels of annotation across the 4 images (13 pixels of annotation inside nucleoli, and 18 pixels outside of them); no image had more than 14 pixels annotated in total. We strongly suggest you make your classifiers one pixel at a time, doing point annotations! Resist the urge to draw squiggly lines all over the image! This feels counterintuitive but we promise it's true.

### Load the classifier and evaluate it
1. Drag `bonus_2_ilastik.cppipe` into the pipeline panel.
1. Drag the `images_Illum-corrected` subfolder from the main exercise into the Images panel.
1. Open the RunIlastik module, set the path to your project file (`NucleoliDetection.ilp`) and then also:
  - ilastik-installed (recommended): set the path to your local installation of ilastik.
  - Docker-installed: change the top setting from Local to Docker.
  - Podman-installed: change the top setting from Local to Podman

3. Put CellProfiler into TestMode <img src="./TutorialImages/startTestMode.png" width="120"/>, open the eye icons <img src="./TutorialImages/EyeOpen.png" width="50"/> next to Runilastik and OverlayOutlines, and then hit <img src="./TutorialImages/Run.png" width="120"/>. You can check the settings in the OverlayOutlines module to see which outline color corresponds to which segmentation in the output.
  - You may wish to put a pause <img src="./TutorialImages/Pause.png" width="50"/> next to SaveImages, or uncheck it <img src="./TutorialImages/InactivatedModule.png" width="50"/>, to keep it from saving images, but that's up to you.

```{note}
If using a container, the very first time you execute the Runilastik module, it will need to download a ~5GB file, which may be slow depending on your internet connection. You only need to do this step once, however!
```

4. Evaluate your prediction in Runilastik across a few image sets - how well does it perform? Does it perform worse on images it wasn't trained on?

```{figure} ./TutorialImages/RunIlastik.png
:width: 700
:align: center

Screenshot of the Runilastik module 
```

5. Evaluate your segmentations across a few image sets using OverlayOutlines and/or the WorkspaceViewer - are there cases where you think the ilastik model-based segmentation performs better? The filtering-and-masking segmentation?

```{figure} ./TutorialImages/WorkspaceViewerilastik.png
:width: 700
:align: center

Evaluating segmentations in the Workspace Viewer
```


### Bonus exercise (for later if you feel exceptionally good on time) - train your own ilastik model

Based on your evaluations above, can you identify some places where additional training might help fix some issues in the ilastik model? 

1. Open `NucleoliDetection.ilp` in ilastik.
1. Navigate to the `Training` tab. 
1. Turn `LiveUpdate` on.
1. Pick an image you think needed some help and add (a very small number of single-pixel) annotations. Did things get better or worse? 
1. Add some very large annotations (a long, squiggly line, for example) to one image, then switch images in order to prove to yourself large annotations can actually harm in some cases rather than help (if you want to save your classifier, go back and use the eraser tool to delete these!).
1. If you think you've sufficiently improved the prediction, save this model and return to CellProfiler. Did it help in the cases where you thought it would?

## **Exercise 3: Running Cellpose from a container runner**

RunCellpose is by far our most popular plugin, simply because a) Cellpose is awesome! and b) installing Python packages when you aren't very computationally comfortable isn't. You can use the plugin in either of two modes - using a local `pixi`, `conda` or `python` installation that contains both CellProfiler and Cellpose, OR using Docker. The run time with Docker is substantially slower (about a minute more per image, in our testing), but if installation would take you a long time and be frustrating, in this sense you can "trade" your personal hands-on frustration time for time where CellProfiler is running on your computer (but you aren't there). For many people, this is a good trade!

### Start Docker/Podman Desktop
1. Start Docker/Podman Desktop.

### Load the pipeline and evaluate segmentation
1. Drag `bonus_3_cellpose.cppipe` into the pipeline panel.
1. Drag the `images_Illum-corrected` subfolder from the main exercise into the Images module
1. Put CellProfiler into TestMode <img src="./TutorialImages/startTestMode.png" width="120"/> 
1. (Optional) If using Podman, adjust the setting `Run CellPose in a Docker/Podman container or local python environment` to `Podman`
1. If needed, adjust the Cellpose model to a version CellProfiler can find
1. Open the eye icons <img src="./TutorialImages/EyeOpen.png" width="50"/> next to RunCellpose and OverlayOutlines, and then hit <img src="./TutorialImages/Run.png" width="120"/>
  - You may wish to put a pause next to SaveImages, or uncheck it, to keep it from saving images, but that's up to you

```{note}
If you didn't already pull the container in the Docker Desktop section above, the very first time you hit the RunCellpose module, it will need to download a ~13GB file, which may be slow depending on your connection. You only need to do this step once however!
```

```{figure} ./TutorialImages/RunCellpose.png
:width: 700
:align: center

The output of the RunCellpose module
```

4. As before, using OverlayOutlines and/or the WorkspaceViewer, evaluate segmentation on a few images. Where is CellProfiler doing better, and where is Cellpose doing better?
5. Use the <img src="./TutorialImages/Info.png" width="35"> button to learn more about the different parameters you can pass to Cellpose (we don't offer all of them, but many!) - how does tweaking these affect your output? How about changing the model you're using, and/or the image you're segmenting?

## **Exercise 4: Running Cellpose and CellProfiler in the same Python environment**

It's very cool that this container trick works, but if for you, the tradeoff of "less install time but longer per-image runtime" turns out to be a bad one, we've given you a chance to also try running the programs in a shared Python environment to see just how much faster things can be.

```{note}
We haven't provided a conda environment here for CellProfiler plus Cellpose, just a pixi one. If you're using conda instead of pixi, if a TA has time, they may be able to help you set this up, or you can try the [instructions in our developers wiki](https://github.com/CellProfiler/CellProfiler/wiki) for your OS, but if it doesn't work, don't feel bad - CellProfiler is actually quite hard to install due to its multi-language dependencies and general complexity. You can go ahead and just skip this part, if you can't get it installed - we've given you plenty of bonuses in this exercise!
```

1. Open your terminal, navigate into the folder where you have downloaded or cloned the GitHub repository for the course, and then into the subfolder `pixi_envs/cellprofiler_cellpose` . Run `pixi shell --frozen` to initialize the environment (this may take a couple of minutes the first time you run it, since things will need to download). Run `python -m cellprofiler` to start CellProfiler (again, this may take a minute on the first time)
1. Generally, do all the same steps as Exercise 3, but instead of choosing `Docker` or `Podman`, choose Python. Do you notice a difference?

### Bonus exercise (for later if you feel exceptionally good on time) - test on a more varied data set

As you may recall, data we gave you in this exercise was all very clean, and all from the negative control wells of this experiment. The Advanced Segmentation data we used on Tuesday - [folder linked again here](https://drive.google.com/file/d/10kZMhr2bVR14vMP7kG5iZGujjUI3Nn3Y/view?usp=sharing) contains images with a broader variety of phenotypes - try replacing the images in the `Images` module with this folder instead and see how well the two kinds of segmentation work in different conditions!

```{figure} ../AdvancedSegmentation/TutorialImages/Fig2.png
:width: 700
:align: center
A broader range of phenotypes can be harder to consistently account for in conducting segmentation.
```

```{tip}
You’re encouraged to explore the performance in some random images on your own (you can do this from the `Test` menu with the `Random Image Set` option), but if you find yourself consistently ending up with images that look similar you can try examining images from the following list of wells- A08, A12, B12, B18, C7, D6, D19, D22, E3. You can pick these with `Test` -> `Choose Image Set`
```

## Bonus exercise (for later if you feel exceptionally good on time) - improve Cellpose segmentation

Based on your evaluations above, can you identify some places where additional training might help fix some issues in the Cellpose model? Run `pixi shell --frozen` inside one of the course's `cellpose` folders (3 or 4, per your preference), open Cellpose with some of the `Ch1` images and see if you can train a model that will perform better.

## What next? Want to know more about CellProfliler plugins and modules?
1. Read the [CellProfiler-plugins paper](https://pubmed.ncbi.nlm.nih.gov/37690102/)
2. Watch this [video](https://www.youtube.com/watch?v=fgF_YueM1b8) to learn how to write a module

---