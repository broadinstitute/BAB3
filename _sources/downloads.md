# Downloads

In preparation for this course you are asked to download a variety of softwares and image sets. Please download all that you are able in advance and we will assist with installation issues during a portion of the lunch break on the first few days. Please note that not everyone will be successful in installing all softwares (institutional computers often have restrictive installation permissions). That’s okay - we encourage you to work with a buddy!

## Software

Before starting the course, please install all of these software (that you are able). We use them in the course in this order.

- [ImageJ/Fiji](https://Fiji.sc/)
- [CellProfiler](https://cellprofiler.org/)
- [CellProfiler Analyst](https://cellprofileranalyst.org/)
- [Ilastik](https://www.ilastik.org/)
- [QuPath](https://qupath.github.io/)

Please install one of the following environment managers, if not already installed on your computer:

- [Pixi](https://pixi.sh/dev/installation/) (our strong recommendation) OR
- [Micromamba](https://mamba.readthedocs.io/en/latest/installation/micromamba-installation.html#automatic-install) (if you already have conda or mamba running on your machine, this is fine too)

Windows machine users -  you will also need to install the [Microsoft Build Tools](https://visualstudio.microsoft.com/visual-cpp-build-tools)

Optionally, a container runner will also be used in one session but is not required:

- [Docker Desktop](https://www.docker.com/products/docker-desktop/) (our recommendation if you're less computationally comfortable, though note your institution may be charged for it)
- [Podman](https://podman.io/) (our recommendation if you're more computationally comfortable)

### Installing the code and container tools needed

Before you come Thursday morning, please _attempt_ the following; help is available Wednesday at lunch, and then during the Thursday morning session.

#### Container instructions

(These may take upwards of 15-20 minutes to download, and require ~20GB of space)

In Docker Desktop or Podman Desktop you can search for containers in the top search bar (see below).
Make sure you select a tag (version) that is supported by the plugin you are using and then select "Pull".
We recommend `biocontainers/ilastik:1.4.0_cv2` for ilastik and `cellprofiler/runcellpose_with_pretrained:3.1.1.2` for Cellpose.

If they are not appearing in search, you can instead pull them from the terminal with the command `docker pull CONTAINER` or `podman pull CONTAINER`, replacing `CONTAINER` with the container name and tag, e.g. `docker pull biocontainers/ilastik:1.4.0_cv2` . If you get a "platform error" on a Apple Silicon Mac (or other arm64 computer), you can add `--platform=linux/amd64` to your pull command, aka `docker pull biocontainers/ilastik:1.4.0_cv2 --platform=linux/amd64`

```{figure} ./BeginnerSegmentation/TutorialImages/DockerSearch.png
:width: 400
:align: center

Search in Docker Desktop for your desired container
```

#### Code instructions

- Install environment manager for code (Pixi or Micromamba)
- Download or clone the [BAB3_environments](https://github.com/broadinstitute/BAB3_environments) GitHub repository (if you're downloading, hit the green "Code" button, then "Download Zip", then unzip the file)
- Open your terminal, type `pixi --help` or `mamba --help` to ensure the tools are installed and accessible in the terminal
- `cd` into the correct directory (for initial testing, you can try `cellpose3`)
-- For pixi, type `pixi shell --frozen`
-- For mamba, type `mamba create --file {name_of_file.yml}`
-- For older versions of conda (if you have an older version), syntax is `conda env create --file {name_of_file.yml}`

#### Cellpose error

```bash
cd cellpose3 #go to the directory containing the pixi files
pixi shell --frozen #activate the new cellpose3 environment (and install, if not already installed)
```

If successful, you should see `(cellpose3)` before your command line prompt. Within this Pixi shell, you should have everything you need for this workshop. You can activate Cellpose just by running `cellpose`

You can always exit the Pixi environment using the `exit` command.

If you get this error when activating Cellpose: `OMP: Error #15: Initializing libomp.dylib, but found libomp.dylib already initialized.` then run:

```bash
pip install --force-reinstall "cellpose<4" 
```

## Image Sets

Please download the following sets of images. Unzip the folders if necessary, and have each folder of images in one parent location:

- [Beginner Segmentation](https://github.com/CellProfiler/tutorials/raw/master/BeginnerSegmentation/Archive_EN.zip)
- [Beginner Segmentation - bonus materials](https://github.com/CellProfiler/tutorials/raw/master/BeginnerSegmentation/bonus_materials_EN.zip)
- [Advanced Segmentation](https://drive.google.com/file/d/1qVaQiPZKqOPEsa62lYEpm7ot3th7ADqE/view?usp=sharing)
- [Microglia](https://drive.google.com/drive/folders/1TyV_hzN2pDpX0lVQ_jh08A9AG5e4Bao8?usp=drive_link)
- [Translocation](https://github.com/CellProfiler/tutorials/raw/master/Translocation/Archive_EN.zip)
- [Brightfield data for Ilastik](https://github.com/CellProfiler/tutorials/raw/master/PixelBasedClassification/Archive.zip)
- [3D data for Ilastik](https://drive.google.com/drive/folders/103WGOpXX4pu2kUBh3nCEuMrPjI3pZK4N?usp=drive_link)
- [Tissue images for QuPath](https://drive.google.com/drive/folders/1z0FnjmPLGjEnIpPu_P8PTB-4ck6Z9BcA)
- [Diverse images for MicroSAM](https://drive.google.com/drive/folders/1NOt_OeMVr6Y5ZK5CrYtG97-P05yABHlx)
