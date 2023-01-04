# PINE Processing Instruction and Help page 2023

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

Open Qgis and, if you have not already, install the PINE Processing plugin (see [Instalation](AGI_PINE_Proccessing_README.md#install)). Once you have the plugin installed and open go to the EO_Linking tab and fill in the values.

If the EO_file already has file names then select the `Reformat Existing Links` option. If the links already have the `.tif` extention then select the `Files have .tif In their names in existing EO file` option as well.

* **Original EO:** `P:\Path\To\EO\File` This should not be in the tif Directory or it will be overwritten.
* **Tif Directory:** `P:\Path\To\Tif\Image\Folder`

##### All Columns are 0 based so the first column is 0 and the second is 1 and so on.

* **Skip Lines:** Number of lines to skip before actual data starts.
* **Exposure/Name Column:** Column number for the Names/Exposure numbers.
* **Easting Column:** Column number for the Easting Value.
* **Northing Column:** Column number for the Northing Value.
* **Elevation Column:** Column number for the Elevation Value.
* **Omega Column:** Column number for the Omega Value.
* **Phi Column:** Column number for the Phi Value.
* **Kappa Column:** Column number for the Kappa Value.
* **Latitude Column:** Column number for the Latitude Value.
* **Longitude Column:** Column number for the Longitude Value.
* **Add Full Path:** Select this option if you want the full file path for the images to be in the EO file.

Once all the values are set select the `Make EO` option and a new EO file of the same name as the original will be made in the tif Directory.

Be warned that if the file already exists in the tif Directory it will be overwritten.

### 4) Generate TFW files for the Compressed Images

In the EO_TFW_GEN tab you can create `.tfw` files for the images. Fill in the Values or load in an existing settings file to start.

* **Load Settings:** `P:\Path\To\settings.txt` This file is generated when the tfw generator is run and can be used to reload previous settings.
* **Tif Directory:** `P:\Path\To\Tif\Image\Folder`
* **Original EO:** `P:\Path\To\EO\File` This should be the one made in step 3.

* **Focal Length(mm):** This value is the focal length in millimeters and will be set automatically from the database if left at `0.00` but can be set to override the database if needed.
* **Original EO:** This value is the Pixel Size in millimeters and will be set automatically from the database if left at `0.00` but can be set to override the database if needed.

##### These values should all auto set to the correct values but may need to be edited in some cases. All Columns are 0 based so the first column is 0 and the second is 1 and so on.

* **Skip Lines:** Number of lines to skip before actual data starts.
* **Name:** Column number for the Names/Exposure numbers.
* **Easting Column:** Column number for the Easting Value.
* **Northing Column:** Column number for the Northing Value.
* **Elevation Column:** Column number for the Elevation Value.
* **Omega Column:** Column number for the Omega Value.
* **Phi Column:** Column number for the Phi Value.
* **Kappa Column:** Column number for the Kappa Value.
* **Latitude Column:** Column number for the Latitude Value.
* **Longitude Column:** Column number for the Longitude Value.
* **Full Name:** Select this option if the EO file has the full path names and not just the file names.
* **Degrees or Angle:** This selection is to set the angle type for the heading. 'Degrees' indicates that East is 0 degrees and North is 90. 'Angle' indicates that North is 0 degrees and East is 90. Both are in degrees, if your angles are in radians you will need to convert them to degrees.
* **Meters or Feet:** This selection is to set the units of the elevation values. The elevation should be above sea-level.

Once all settings are set you can select either `Generate Settings` witch will only make a settings.txt file in the tif Directory or `Create TFW` witch will make both the settings.txt and a tfw file for each `.tif` file in the tif Directory. Any existing tfw files will be overwritten when this is run.

### 5) Setup Image Footprints

### 6) Select Image Sets on Easements and Send to Processing

### 7) Fix Processing Offsets and Errors

### 8) QC Final Clipped Easement Images

# FAQ
