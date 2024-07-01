---
jupytext:
  text_representation:
    extension: .md
    format_name: myst
    format_version: 0.13
    jupytext_version: 1.11.5
kernelspec:
  display_name: Python 3
  language: python
  name: python3
---


# Build a Python Environment for Using the FLDPLN Model

In order to use the FLDPLN model, we need to have a Python environment with the necessary packages installed. This document provides information on how to build the "fldpln" Python environment and how to use it in various environments including local and cloud JupyterLab and in Visual Studio Code (VSC). 

The following steps are tested on my desktop computer (gas-fpg0ff3.home.ku.edu) for the 2024 Summer Institute which include the following major components:
* Create a Python environment and install necessary common packages
* Install fldpln package 
* Install the fldpln_py Python package and MATLAB Runtime

## Create the “fldpln” Python Environment

Here we assume that you don't have conda installed and we start from scratch. If you already have conda installed, you can skip the first step.

### Install Miniconda

First we will download Miniconda to install Python and necessary Python environment management tools on the system. Miniconda is a lightweight version of Anaconda, which is a full-fledged data science platform. Miniconda only includes Python and conda, while Anaconda includes Python, conda, and a suite of other packages.

* Go to the [Miniconda download page](https://docs.conda.io/en/latest/miniconda.html#windows-installers) and download the Miniconda installer for Windows. The web site provides the installer with the most recent Python version. This is fine as we can create a new Python environment with the desired Python version later.
* Run the installer and follow the instructions to install Miniconda on the system. Make sure to check the box to add Miniconda to the system PATH so that you can use conda in the command prompt window.

### Create the Python environment

* Open the Anaconda Prompt (miniconda3) Command Window which is created by Miniconda and has Python 3.10 and conda installed.
* Create the “fldpln” env by running the following command. This will create a new Python environment with the name “fldpln” and the Python version (Python 3.10) comes with Miniconda. **It's important to specify the Python version (in our case 3.11) as this is the hightest Python version that the MATLAB Runtime supports.**
  ```
  conda create -n fldpln python=3.11
  ```
* List the available envs using the following command:
  ```
  conda env list
  ```

### Install Mamba for faster package management

* Activate the “fldpln” env using the following command:
```
conda activate fldpln
```
* Install Mamba, a faster and more efficient package manager than conda, using the following command:
```
conda install -c conda-forge mamba
```

## Install Necessary Common Packages

Make sure the “fldpln” env is activated before installing those packages. 
```
  conda activate fldpln
```

* Install geopandas package using the following command. This will also install many dependent general and geospatial packages including numpy, pandas, scipy, gdal, shapely etc.
```
mamba install -c conda-forge geopandas
```
* Install “h5py” (for reading .mat file), “rasterio”, “openpyxl” (for reading Excel xls file) and “fastparquet” (for reading parquet file, i.e., .snz file) using the following command:
```
mamba install -c conda-forge h5py rasterio openpyxl fastparquet
```
  Note that we cannot use “import gdal” but have to use “from osgeo import gdal” and this is explained [here](https://opensourceoptions.com/blog/how-to-install-gdal-with-anaconda/), **The GDAL distribution from conda-forge installs GDAL as parts of the OSGEO package, which includes GDAL, OGR, and OSR**.
* Install packages for mapping
  * rio-cogeo for CloudOptimized GeoTIFF (COGEO) creation plugin for rasterio using the following command. Note the hyphen in the package name.
  * lxml for processing XML and HTML
  * psycopg2 for PostgreSQL database adapter
```
mamba install -c conda-forge rio-cogeo lxml psycopg2
```
* Install dask package for parallel computing. This package is not necessary if inundation mapping is not time critical (say, for research purpose).
```
mamba install -c conda-forge dask
```

## Install FLDPLN Related Python Packages

Currently, there are two python packages are needed for using the FLDPLN model. The first package is the fldpln_py package which is a wrapper for the FLDPLN model implemented in MATLAB. The second package (fldpln_mapping), which **has NOT been created yet,** contains Python scripts for tiling and mapping FLDPLN libraries.  

### Install the FLDPLN Model Python Package

The FLDPLN model is developed in MATLAB and compiled into a Python package (fldpln_py). The compiled Python package can be installed on any system (Windows or Linux) with Python and MATLAB Runtime installed. Note that the MATLAB Runtime is required but free to run the compiled Python package.

#### Install the fldpln_py Python package

  * The simplest way we can zip the package and make it available for download and install it by running "python setup.py install" or "pip install ." in the package folder.
  * We can also put the package on Github's root folder and install it using "pip install git+https://github.com/opengeos/leafmap.git"
  * Ideally, we should publish the package to PyPI or Anaconda Cloud and install it using pip or conda

#### Install the MATLAB Runtime
  * The MATLAB Runtime installer for Windows automatically sets the library path during installation, but on Linux or macOS you must add the libraries manually. See [here](https://www.mathworks.com/help/compiler_sdk/cxx/mcr-path-settings-for-run-time-deployment.html) for more information.

### Install the fldpln_mapping package

The mapping package has several pure Python scripts which can be directly used/imported in notebooks. In the future, we plan to create a package, fldpln_mapping, and publish it on PyPI so that it can be installed using pip.
 
## Using the Environment in Different Development Environments

The fldpln python environment can be used in different development environments including Visual Studio Code (VSC) and JupyterLab. Below are the steps to use the environment in those environments.

### Use the environment on cloud JupyterLab

Below are the additional steps needed to use the environment on the virtual machine's JupyterLab running on the [AWI 2i2c-based cloud computing infrastructure](https://staging.ciroh.awi.2i2c.cloud/hub/login).
```
conda activate fldpln
mamba install -c conda-forge jupyter
Conda activate fldpln
```

After this, when you create a new launcher, you should see notebook using the fldpln environment as an option as shown in the screenshot below.
![JupyterLab on the cloud](./images/JupyterLab_on_awi_cloud.png)

### Use the environment in local JupyterLab

You may also want to use the environment with  Jupyter notebooks on your own machine. In this case, we need to install the jupyterlab package into the env using the following command:
  ```
  mamba install -c conda-forge jupyterlab
  ```
After installation, first navigate to the folder where your Jupyter notebooks are located and run the following command to start JupyterLab in a web browser. Note the space between the words.
```
jupyter lab
```
You can shutdown the local JupyterLab by using "Ctrl + C" in the command window where you started JupyterLab.

You may also want to install the Git extension for JupyterLab for version control:
  ```
  conda install -c conda-forge jupyterlab-git
  ```

### Use the ‘fldpln’ environment in VSC

The Python environment should be available in VSC for writing Python scripts. Below are some trouble shooting steps if the environment is not available in VSC.
* Make sure the conda command is added to system’s PATH environment variable so that VSC can use the conda to activate a specific python env.
  * When install miniconda (or Anaconda), the default installation setting doesn’t add its executable path (on my machine “C:\Users\lixi\Anaconda3\”) to the PATH environment variable.
  * This is done by using the Advanced System Settings in Control Panel (see the screenshot below)
    ![Setup PATH variable](./images/PATH_environment_variables.png)
* VSC terminal 
  * VSC uses powershell as the default terminal which cause some issues even after including conda path in the PATH environment variable.
  * A quick solution is to change the default powershell terminal in VSC to the regular cmd terminal by press CTRL+SHIFT+P in VSC and search for “terminal select default profile” and select “Command Prompt C:\WINDOWS\System32\cmd.exe”
* When using VSC to run Jupyter notebooks, VSC automatically installed ipykernel into the environment.

VSC also supports Jupyter notebooks. But this needs the ipython kernel package be installed into the environment with the following command. Note that the kernel can also be installed when you open a notebook in VSC and choose the environment.
  ```
  mamba install -c conda-forge ipykernel
  ```

## Replicate the fldpln Environment Using YAML File

Once the pyhton environment is built, it is possible to export the environment as a YAML configuration file and replicate the environment on a different machine. We can export the fldpln environment to a .yaml (or .yml) configuration file using the following command:
```
conda env export -n fldpln > fldpln.yaml
```
or using the following command if you are already in the fldpln env:
```
conda env export > fldpln.yaml
```
**For some reason, the first line in the YAML file ("name: base") does not reflect the environment name we want (i.e., "fldpln"). In this case, we need to change "base" to "fldpln" before creating the environment on the target machine.**

On the target machine (assuming that conda and mamba have already been installed) in a command window, navigate to the folder where the .yaml file is saved, and run the following either conda or mamba to create the fldpln env using the file:
```
mamba env create -f fldpln.yaml
```
```
conda env create -f fldpln.yaml
```
  
Solving the environment and installing all the packages might take a surprisingly long time especially using conda, so be patient.

**Note that different YAML files are needed for Windows and Unix-based systems**. Two YAML files, fldpln_windows.yaml and fldpln_awi_2i2c.yaml, are created for Windows and Unix-based systems respectively. **Those YAML files don't have the fldpln and fldpln_py packages installed.**