CellProfiler Pipelines
======================

A collection of pipelines for the [CellProfiler](http://www.cellprofiler.org/) cell image analysis software, developed with the neuroimmunology research group of [Prof C. Linington](http://www.gla.ac.uk/researchinstitutes/iii/staff/christopherlinington/) at the Institute of Infection, Immunity and Inflammation, University of Glasgow, UK.

Copyright (C) 2015 Steve Muecklisch

This work is licensed under a [Creative Commons Attribution 3.0 Unported License](http://creativecommons.org/licenses/by/3.0/deed.en_GB).

Context
-------

These pipelines are used to characterise different glial cell cultures to quantify neurite density and myelination, differentiation of oligodendrocytes, proliferation and nuclei as well as cell densities (pixel intensity in various channels). Cell cultures analysed are either [dissociated myelinating cultures](http://link.springer.com/protocol/10.1007%2F7651_2014_129), neurosphere-derived astrocytes generated from striata of P1 Sprague-Dawley rats, or oligodendrocyte cultures purified from P1 rat cortex.

After a few days in culture, cells were fixed with 4% PFA and immunostained for different markers. Primary antibodies used were the following: SMI-31 (1:1500, Abcam), Z2 (anti-MOG, 1:200, clone Z2), PLP/AA3 (1:100, hybridoma supernatant supplied by S. Barnett), MBP (1:100, Millipore), Olig2 (1:1000, Millipore), and GFAP (1:500, Dako). Bound antibodies were visualized using appropriate combinations of species/isotype specific fluorochrome-conjugated secondary antibodies (1:500, Invitrogen, either 488nm or 568nm), followed by mounting in a media containing DAPI. To analyse cell proliferation, cells were pulsed with EdU and stained using the Click-iT EdU Alexa Fluor 594 Kit following the manufacturer's instructions.

Images were captured using an Olympus BX51 fluorescent microscope with standard filters for DAPI, GFP and Cy3. Images were saved as TIFFs at 300dpi (1392x1040 pixels). Each pipeline scans for `.tif` image files in the specified input folder and contained subfolders. A trailing number in the file name (before the extension) is extracted as variable `<Number>` and the subfolder name is stored as `<Group>` -- these are reflected in the output file (`DATA.csv`). Furthermore, for each input image an output image in PNG format is generated, visualising the extracted data with the purpose of easy manual verification of the data generation process.

### dapi.cp

Counts the number of stained nuclei (DAPI, EdU Alexa Fluor 594) in both culture systems for pictures in 20x magnification.

The blue channel of the input image is extracted, illumination correction applied, and nucleic objects detected. The only measurement exported is the count of nuclei.

### dapi-edu-aa3/olig2.cp

Both pipelines quantify the absolute number of cells (as in `dapi.cp`), absolute number of proliferating cells (EdU Alexa Fluor 594), and absolute number of oligodendrocytes (Olig2 or AA3 stained for 488nm, respectively) as in `olig2-aa3.cp`. Furthermore, the amount of proliferating oligodendrocytes (EdU+/Olig2+ or EdU+/AA3+) is identified. Pictures were taken with 20x magnification.

The pipeline extracts red, green and blue channels from the input image and corrects for illumination. In the red and blue channels nucleic objects are identified; whereas the green channel gets a threshold applied, small features (Olig2) or myelin strings (AA3) are suppressed and remaining objects identified. Subsequently, green objects contained within red objects are determined, signifying proliferating oligodendrocytes.

Measurements exported are the counts of identified objects in all channels as well as the proportion of green objects contained within red objects (`Mean_RedObjects_Children_GreenObjects_Count`). To obtain the absolute number of double-positive (red and green) cells this proportion should be multiplied with the absolute number of red objects (`Count_RedObjects`).

### myelin.cp

Provides quantitative analysis of axonal density and myelination (as proportion): the axonal marker SMI-31 (568nm) and myelin marker MOG (Z2, 488nm) were stained. This also works for other myelin markers such as MBP and PLP (AA3). Pictures were taken with 10x magnification.

The pipeline extracts both the red and the green channel from the input image. The green channel is corrected for illumination, objects are detected and subsequently filtered for their shape, so that only strings of myelin are left. The red channel only gets a binary threshold applied. Measurements exported are the remaining areas as well as total image area in pixels.

### olig2-aa3.cp

Analyses the number of OPCs and mature oligodendrocytes, when stained with Olig2 (568nm) and PLP (AA3, 488nm). Pictures were taken with 20x magnification.

The pipeline extracts both the red and the green channel from the input image. The red channel is corrected for illumination, the remaining pixel intensity rescaled, a 25% threshold applied, small features suppressed and remaining objects identified. The green channel gets a threshold applied, myelin strings are suppressed and remaining objects identified. Measurements exported are the counts of identified objects in the red and green channels.

### pixel-int-g/r.cp

Quantifies pixel intensity in the green or red channel, respectively. This was optimised for gfap-stained astrocytes in myelinating cultures or neurosphere-derived astrocyte cultures in 20x magnification, but can easily be modified (re-set threshold) to be used to analyse any pixel intensity in the green or red channel.

The respective channel is corrected for illumination, a threshold applied and the occupied area measured. Exported measurements are the area occupied as well as the total image area in pixels.

References
----------

* Linder, M., Thümmler, K. et al: *[Fibroblast growth factor signalling in multiple sclerosis: inhibition of myelination and induction of pro-inflammatory environment by FGF9](http://brain.oxfordjournals.org/content/early/2015/04/22/brain.awv102.long).* In Brain, April 2015.
