# PINE Processing Instruction and Help page 2023

## Install Plugin

## Processing Steps

### 1) Check EO and Image Files and Names

File Names should be in the format of `<Line Number>_<Eposure Number>_<Date as DDMMYY>_<Time as HHMMSS>.tif` as Long as at least the first half is corect this process should work. You will also need to check the EO file and make sure that it either has a corasponding name column that matches the file names or an exposure number column that does not have any repeats. If a File name repeats then the full file path must be in the EO and the repeats must be in difrent folders.

### 2) Run Compresion and Geo Cleaning in ThreadPooler

Open the ThreadPooling.exe at K:\\Programs\\ThreadPooling\\ThreadPooling.exe and fill in the following fields:

* **Directory:** `P:\Path\To\Folder\With\Tif\Files`
* **File Extention:** `tif`
* **Command:** `C:\Osgeo4w\osgeo4w.bat`(or whatever your local OSGeo4W.bat location is)
* **Arguments:**`gdal_translate -CO "PROFILE=BASELINE" -of GTIFF -MO "TIFFTAG_DOCUMENTNAME=AGI" -MO "TIFFTAG_IMAGEDESCRIPTION=AGI" -CO "TILED=YES" -CO "COMPRESS=LZW" -CO "PREDICTOR=2" -b 1 -b 2 -b 3 -b 4 {{file}} .\<NewDir>\{{file}} & gdaladdo -ro .\<NewDir>\{{file}} 4 8 16`
* **Create a new directoy:**`<NewDir>`
* **Number of Threads:** This field is up to how much your computer can handle. It is editable while the processes are running so start low at 6 to 10 and rise it up as possible.

I suggest running a test first to make sure you don't overwrite the existing files on accident.

### 3) Run EO Linker or EO reformating

#### 3a \[optional\]) Set up your own copy of the PINE Qgis Project

This step is not necessary but can be useful. If you don't do this and later need to change or replace an existing shape file you will need to find everyone who may have it open and ask them to close it. This does mean that to get any updates to the layout you will need to copy the files over though.

To do this simply make a copy of <To be Defined>.qgz into a folder of your username in the project DB folder. When first opening it make sure all the layers work and redirect them if needed. Do not drop any layers as you will proboly want to fix them at some point.

#### 3b) Use the EO_Linking tab in the PINE processing plugin to format and link your original EO file for processing.

Open Qgis and, if you have not already, install the PINE Processing plugin (see [Instalation](README.md#install))

### 4) Generate TFW files for Compressed Images

### 5) Settup Image Footprints

### 6) Select Image Sets on Easments and Send to Processing

### 7) Fix Processing Offsets and Errors

### 8) QC Final Cliped Easment Images

# FAQ
