# Day 4, Exercise 2 - 'Segment Anything' Model (SAM) for Microscopy

*Lab authors: Esteban A. Miglietta . 

---

## MicroSAM: 'Segment Anything' Model for Microscopy

Segment Anything for Microscopy (or MicroSAM) is a tool for interactive and automatic annotation for microscopy data. It is built on top of [the Segment Anything Model](https://segment-anything.com/) (aka. SAM), a foundational model developed by Meta AI for prompt-based and automatic segmentation of images. The original SAM model was trained on millions of annotated masks from thousands of images (mostly natural images, but some scientific and medical images as well). MicroSAM is a specific implementation of SAM which specializes it for microscopy and other biomedical imaging data.

You can find all the documentation for this tool [HERE](https://computational-cell-analytics.github.io/micro-sam/micro_sam.html#annotation-tools).

While you can use MicroSAM directly from the [command line](https://computational-cell-analytics.github.io/micro-sam/micro_sam.html#using-the-command-line-interface-cli) or as a [Python library](https://computational-cell-analytics.github.io/micro-sam/micro_sam.html#using-the-python-library) (they even have [quick start jupyter notebooks](https://github.com/computational-cell-analytics/micro-sam/blob/master/notebooks/quick_start.ipynb) that you can run online on Kaggle or Google Colab), we will use the MicroSAM Napari plugin for this workshop.

### Learning Objectives
- Learn about SAM and it's specific implementation for microscopy: MicroSAM
- Explore MicroSAM as a tool for aiding in manual annotations in Napari, using the plugin
- Explore the limitations of the model

**Lab Data** in [this folder](https://drive.google.com/drive/folders/1NOt_OeMVr6Y5ZK5CrYtG97-P05yABHlx?usp=sharing)

---

### Installation
Using Pixi:
Clone the BAB3 repo locally and `cd` into that directory, then:

```
cd pixi_envs/microsam #go to the directory containing the pixi files
pixi install #install the microsam environment using the local lock file
pixi shell #activate the new microsam environment
```

If succesfull, you should see `(microsam)` before your command line prompt. Within this Pixi shell, you should have everything you need for this workshop.

You can always exit the Pixi environment using the `exit` command.

---

### Loading an image and calculating embeddings
In order to use the MicroSAM models to segment the image, first we need to use the model to compute a simplified representation of the image, aka. an *embedding*.

- Open napari using the command `napari` in the command line
- Load a sample image from  `File > Open Sample > SegmentAnything for Microscopy`. We recommend starting with `HeLa 2d example data` for 2D segmentation.

```{margin} NOTE

The very first time you open a sample image, the plugin will first download it, which might take a few seconds.
 ```

- Go to `Plugins > SegmentAnything for Microscopy > 2d annotator`.
- In the plugin window, choose the image for which to compute the embeddings in the `Image Layer` menu and choose the appropriate `Model` according to the type of images to segment.
- In the Embedding Settings menu, choose the `model size` (bigger) and change directory for saving embeddings
- Run `Compute Embeddings` with the default parameters (check where the embeddings are saved, you can specify a different location).

```{margin} NOTE
The very first time you run any model, the plugin will first download the model weights, which might take a few minutes.
```

### Image annotation in 2D ([Documentation](https://computational-cell-analytics.github.io/micro-sam/micro_sam.html#annotator-2d))
- Segment cells using either bounding box prompts or positive/negative point prompts

```{margin}
Try prompting the plugin to segment structures of varying sizes. 
```

- You can also try to do an `Automatic Segmentation`

- Choose a `commit path`, where you want to store the segmentation masks that you generate.

- Remember to `Commit` your segmented objects to save them!

### Annotate an entire folder: The image Series Annotator ([Documentation](https://computational-cell-analytics.github.io/micro-sam/micro_sam.html#image-series-annotator))
- Go to `Plugins > SegmentAnything for Microscopy > Image Series Annotator`.
- As before, select the `Model` according to the image type, and it's `Size`.
- Select the `Input Folder`, `Output Folder` and `embeddings save path` (each one should be a different folder, named accordingly).

```{caution}
Your input folder should **ONLY** contain your images and nothing else (like other files or other subfolders), otherwise the plugin will error.
```

- Click `Annotate Images`. This will calculate the embeddings for all the images in the folder so it might take a while.

- When the embedding computations are done, segment the image the same way you did before and click `Next` (or hit `N`) to move to the next image in the directory and continue annotating.

### Image Annotation in 3D ([Documentation](https://computational-cell-analytics.github.io/micro-sam/micro_sam.html#annotator-3d))
- Similar to the the exercise before, but this time use the `Annotator 3d`, from `Plugins > Segment Anything for Microscopy`

```{margin}
Note that computing embeddings for the whole stack will, expectedly, take a longer time than for a single 2d image!
```

- Use either bounding boxes or point prompts to segment (`S` key) a few slices across the stack. Then, click `Segment Stack` to predict the segmentation in the rest of the intermediate slices. Go through all of them and correct the ones that are wrong using manual prompts.

- Once you finish, you can `Commit` the 3D annotation.


### Bonus Exercise: try SAMJ in Fiji

SAMJ is an implementation of SAM that can be run in ImageJ/Fiji using a plugin.

- Install the SAMJ-IJ for Fiji/ImageJ following the installation guide [HERE](https://github.com/segment-anything-models-java/SAMJ-IJ?tab=readme-ov-file#model-installation)

- Open `Plugins > SAMJ > SAMJ annotator`

- Check the instructions for installing models [HERE](https://github.com/segment-anything-models-java/SAMJ-IJ?tab=readme-ov-file#model-installation)

- Load one of the sample images you used before, calculate the embeddings and try to segment it following [the manual](https://github.com/segment-anything-models-java/SAMJ-IJ?tab=readme-ov-file#annotating-images)

```{margin}
You can use preexisting point annotations (for example, generated using `Find Maxima`) as prompts by choosing the "Preset Prompts" option.
```

