# TookClassificationMacro
ImageJ macro for analyzing and classifying the transforming ookinetes of Plasmodium berghei

This macro, written in the ImageJ macro language, was created to automatically classify the early oocyst development into 7 differentiation stages and to measure the fluorescence intensity of the sorted parasites.

Installation.
To install the macro, you will need to have/download additional programs and ImageJ plugins.

Programs:
ImageJ version 1.53c or later - https://imagej.nih.gov/ij/
R - https://www.r-project.org/
Rstudio (optional) - https://rstudio.com/products/rstudio/
Plugins:
Template Matching and Slice Alignment - https://sites.google.com/site/qingzongtseng/template-matching-ij-plugin
Morphological Operators for ImageJ - https://blog.bham.ac.uk/intellimic/g-landini-software/
Macros:
TookClassMacro

Once you have installed all the programs/plugins you need to unzip the TookClassMacro.zip file in the ImageJ/macros folder.
If you already have a "Library.txt" file in your ImageJ macros folder just open that file and copy-paste the lines of the "TookClassMacro.txt" that start with "//Save the following functions in the Library.txt file located... from here -> ... to here <-" into a new line of the "Libary.txt" file and delete the slash and asterix of the start and finish of the command lines.
To install the macro open ImageJ then open the tab Plugins->Macros->Install. Find the TookClassMacro.txt and click open.
To run the macro open the tab Plugins->Macros->TookClassificationAndFluorescenceAnalysisMacro.

Optionally, you can create a custom button to by copy-pasting the lines that start with "//Save the following command in the StartupMacros.txt file located... from here-> ... to here <-" in a new line at the bottom of the StartupMacros.txt located in the ImageJ macros folder.


Usage.
The macro accepts two file structures:

- The most common one with three images: 
     - Bright-field image taken with filter a
     - Fluorescence image of the parasite taken with filter a
	 - Fluorescence image of the nucleus taken with filter b

- And another one with four images per parasite if there are alignment issues between the use of different filter cubes:
     - Bright-field image taken with filter a
	 - Fluorescence image of the parasite taken with filter a
	 - Bright-field image taken with filter b
	 - Fluorescence image of the nucleus taken with filter b

The fluorescence images taken with any of the filters (to image the parasite or nucleus) can instead be videos in .avi format. The macro automatically extracts the brightest frame of the video for further analysis.
The images must be in this order and named with ascending numbers with leading zeros e.g. 0001, 0002, 0003, …, n, or timestamps as filenames.

It is recommended to run the macro with a copy of the images/videos, as the operations applied are not reversible. Keep the original images/videos in a separate folder, you might need them later.
This macro classifies the parasites in an unsupervised manner using a machine learning algorithm, where a parasite is compared against a training database. For classification, actual measurements of the parasites are taken using micrometers as the scale. This has two purposes: first it makes the measurements biologically relevant. And second, it makes possible the use of different microscopes and capturing devices configurations, so pixel binning or the use of different sizes of CCD's or CMOS chips should not matter. This implies that the microscope-capturing device must be calibrated using a micrometer ruler or the like. The amount of pixels that corresponds to a given distance in micrometers needs to be provided when asked by the macro.
As it runs, the macro creates folders containing the resulting images, it is important to not change the folder names, as they are the checkpoints needed to resume the macro if it has to be stopped.
The macro first extracts the brightest frame from the videos (if needed) and saves the images in a folder named "Raw_Extracted". The images are then aligned (if needed) and the folder is renamed to "Raw_Extracted_Aligned". After cropping the parasites form the images, a new folder named "Cropped" is created containing the resulting images. After classification, each set of images of a parasite is saved in a folder named accordingly to its differentiation stage ("Ookinetes", "Tooks_1", "Tooks_2", "Tooks_3", "Tooks_4", "Tooks_5", and "Oocysts"). After the fluorescence intensity measurements are done, the "Raw_Extracted_Aligned" folder is renamed to "Raw_Extracted_Aligned_Analyzed".

The fluorescence intensity measurements obtained will be saved in the same directory of the analyzed stage as a spreadsheet file.
The measurements of each parasite consist of 25 rows. The first row is the measurements of the Bright-field image, the second, third, and fourth rows contain the measurements of the red, green, and blue channels of the parasite's fluorescence image respectively. The next three rows contain the measurements of the red, green, and blue channels of the nucleus's fluorescence image. The next 9 rows contain the measurements of three background zones of the parasite's image also separated by color channel. The same is for the next 9 rows which contain the nucleus's background measurements. If your images are monochromatic, just take the measurements of any channel. This repeats for all set of images in the batch (folder).
