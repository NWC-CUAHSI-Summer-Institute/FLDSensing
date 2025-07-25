# FLDSensing: Remote Sensing Flood Inundation Mapping with FLDPLN

This project aims to  delineate flood extent and depth using remote sensing-derived clean edge pixels to estimate Depth of Flow (DOF) (water stage) using the relationships between FSP (Flood Source Pixels) and FPP (Floodplain Pixels) generated through the [FLDPLN model](https://kuscholarworks.ku.edu/handle/1808/5354).

This repository contains three files: the FLDSensing Jupyter notebook, the conda environment YAML file, and a Tile Size and Spatial reference JSON file. To use this script, FLDSensing requires a FLDPLN inundation library to already be created for a specific area. To download the entire FLDPLN library for the state of Kansas, visit the [Kansas Applied Remote Sensing website](https://kars.ku.edu/pages/kansas-inundation-mapping). FLDSensing also requires a Digital Elevation Model (DEM) associated with the created FLDPLN library to snap and align clean edge pixels to the FLDPLN library. Contact Dr. Xingong Li (lixi@ku.edu) for access to the .bil files.

## Installing
FLDPLN model libraries should be downloaded in advance (from the link above) to run the notebook, and the user should utilize the provided YAML file to install all dependencies for FLDSensing. 
When the libraries are downloaded from the link above, they will contain libraries for the entirety of Eastern Kansas (e.g., Verdigris or Neosho). Within each folder for each of the libraries, the user should place the Spatial reference JSON file in the folder, or else the script will not run. Within each library folder, the user should create an RS folder (to hold clean edge pixels derived from remote sensing imagery) and a bil folder (to hold the DEM). Within the FLDSensing notebook, the user can then specify the path to a chosen library, clean edge pixels, and the DEM.

If the user would like to create a FLDPLN library elsewhere, follow the instructions from the notebooks in this [FLDPLN repository](https://github.com/XingongLi/fldpln)

## GEE web application for RS edge extraction
A GEE web application has been created to allow for the easy generation and export of clean edge pixels in a specific area. To generate a file with clean flood edge pixels, the user provides masking parameters such as land cover, cloud coverage, and NDVI thresholding parameters to remove edge pixels that could lead to an incorrectly derived water stage. To use this app, follow this [link](https://code.earthengine.google.com/07a99d756a3420ecae76fd2766198110). NOTE: A Google account and Google Earth Engine account are required to use this app (the clean edge pixels will be exported to your Google Drive). If the user would like to use a different method of clean edge pixels, they could create a different application (e.g., a Python script) to handle this. A GEEMAP version has also been developed and is located within the FLDSensing Jupyter Notebook. If the user decides to use their own clean edge pixel extraction process, it is highly recommended to utilize some sort of land cover, cloud, and slope masking. As incorrect edge pixels could have a negative impact on FLDSensing results. Clean edge pixels should be the true border of the flood and dry land. Clean edge pixels, ideally, should not border clouds or vegetation, as clouds and vegetation could hide further inundation. The purpose of clean edge pixels is to get an accurate water stage when matched to the FLDPLN inundation library. 

## Notebooks
FLDSensing Jupyter notebook requires minimal user interaction. Once the libraries, DEM, and clean edge pixels are downloaded, the user only needs to specify a path to these areas as specified within the notebook. The FLDPLN library downloaded from the above link has a 5-meter spatial resolution. If the user generates a library of a different resolution, then they should specify that accordingly by modifying the cell_size parameter at the top of the notebook. The user then needs to specify a path to save the resulting FIM. If results are not satisfactory, then modify clean edge pixel parameters, make sure that the proper stream order (stream order where inundation is occurring from) has been selected, and that the data smoothing variables are correct.

## Acknowledgments
FLDSensing project developers: Jackson Edwards, Son Do, and Francisco Gomez. 

We would like to thank Dr. Xingong Li and Dr. Sagy Cohen for their guidance, supervision, and support during this project. We would also like to thank Dr. Jude Kastens and David Weiss for their support and continued development of the FLDPLN model and library. 

[SI Report: FLDSensing (Pages: 48-60)](https://www.cuahsi.org/uploads/pages/doc/202407_Summer_Institute_Final_Report_v2.0.pdf)

