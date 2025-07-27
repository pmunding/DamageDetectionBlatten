# DamageDetectionBlatten

Damage Detection of buildings caused by the Blatten glacier. On 28th of May, the Blatten glacier collapsed and caused a lot of destruction. The debris caused by glacier meltdown destroyed a large part of the village, most of the house were buried under the Landslide and some were damaged by the artifical lake caused by the blockage of the river passing through the village.

This repository contains all the workflow of the damage detection analysis in the form of R markdown file, shapefiles, images, and we have also provided a model which can be utilized in QGIS Model Designer for reproduction and/or regeneration of results and also for automation.

---

The process of detecting the damaged building, can be seen in the [Image](4-Misc/Process.png). The initial filter analysis code can be seen in the [R markdown file](4-Misc/Swiss_Blatten_glacier_damage_analysis.Rmd). 

The whole analysis is divided into two parts Pre and Post-event, both have same steps, with slight differences for example "Compactness" was added to differentiate between the houses and elongated road segments, as this factor is important for the differentiation of the shape of houses and roads not the glacier debris or the lake. The Results folder contains the end results.

# Steps for Pre and Post events

## Step - 0 -- AOI (Area of Interest)

Our Area of interest is within the coordinates: **46.422532, 7.797989** and **46.406229, 7.829559**. The data has been retrived from the offical site of **Federal Office of the Topography swisstopo Rapid Mapping** [link](https://www.rapidmapping.admin.ch/index_de.html)

Due to limited computational resources, we choose the north-eastern part of the village due to high spatial frequency. As there were houses damaged by galcier landslide, houses damaged by the lake, and some undamaged houses  [pre](1-Pre-event/Step-0%20--%20AOI/19_5_AOI.tif) and [post](2-post-event/Step-0%20--%20AOI/30_5_AOI.tif).

## Step - 1 -- Filters

We have applied Laplacian filter with value 3 on band 1, [pre](1-Pre-event/Step-1%20--%20Filters/pre_laplacian3_ch_1.tif) and [post](2-Post-event/Step-1%20--%20Filters/post_laplacian3_ch_1.tif), and then also on band 3 of [pre](1-Pre-event/Step-1%20--%20Filters/pre_laplacian3_ch_3.tif) and [post](2-Post-event/Step-1%20--%20Filters/post_laplacian3_ch_3.tif) event.

Then we applied the GLCM texture filter on the band 1 of both the [pre](1-Pre-event/Step-1%20--%20Filters/Pre_GLCM_(dissimilarity).tif) and [post](2-Post-event/Step-1%20--%20Filters/Post_GLCM_(dissimilarity).tif) event.


## Step - 2 -- Clipping of Inputs

We removed the unnecessary surrounding part of the the main RGB image and also the filtered images:

[Here](1-Pre-event/Step-2%20--%20Clipping%20of%20Inputs/) are all the clipped images for the pre event.
[Here](2-Post-event/Step-2%20--%20Clipping%20of%20Inputs/) are all the clipped images for the post event.

## Step - 3 -- Final input image

After clipping we have stacked all the four images to form a 6 band raster image for both the scenario: [Pre-event](1-Pre-event/Step-3%20--%20Final%20Input%20Image/) and [Post-event](2-Post-event/Step-3%20--%20Final%20Input%20Image/).

## Step - 4 -- Segmenetation 

The final stacked image is fed to the segmentation algorithm for OTB Toolbox in QGIS with the parameter *Spatial Radius* as 15 and *Range Radius* as 14. The segemenation output for the pre event are [here](1-Pre-event/Step-4%20--%20Segmentation/) and for the post event are [here](2-Post-event/Step-4%20--%20Segmentation/).

## Step - 5 -- Zonal Statistics

We have calculated the statistics for all the segments. These statistics include min, max, mean, standard deviation, etc. Which were then used fo rthe training of the model along with compactness.

[Pre](1-Pre-event/Step-5%20--%20Zonal%20Statistics/) <br>
[Post](2-Post-event/Step-5%20--%20Zonal%20Statistics/)

## Step - 6 -- Add Compactness

At this point segments' compactness was added to differentiate between the more compact houses' segments and elongated roads' segments because with compactness there was no significant sprectral difference between the two. Step 6 is only for the pre-event.

[files](1-Pre-event/Step-6%20--%20Add%20Compactness/)

## Step - 7 -- Join Labels

Then we joind the Sample Labels with these segments having statistics already calculated in order to train the model.

[pre event labels](1-Pre-event/Step-1%20--%20Filters/PreEvent%20Labels/) <br>
[post event labels](2-Post-event/Step-1%20--%20Filters/PostEvent%20Labels/)

These labels are manually picked with 195 labels for Pre-event and 198 for Post-event

## Step - 8 -- Model Training

The sample segmebts produced in the previous step were then fed to the SVM for training. Both the models for pre and post events are trained separately.

[Pre - model](1-Pre-event/Step-8%20--%20Model%20Training/)<br>
[Post - model](2-Post-event/Step-8%20--%20Model%20Training/)

## Step - 9 -- Classes

The model gives us different classes for both the events based on the number of classes in the samples.

[pre - event classes](1-Pre-event/Step-9%20--%20Classes/) <br> [post - event classes](2-Post-event/Step-9%20--%20Classes/)

## Step - 10 -- Specific class

For the Pre event only the Houses are extracted from among the other classes and for the Post event only the Glacier and Lake has been extracted from among the other classes in order to perform the intersection.

# Results:

The output from both the models is being intersected to find out the buildings which are damaged or not, final classes are Damaged by Lake, Damaged by landslide, Not Damaged. The result files can be found [here](3-Results/).

# Automation / Reproduction:

Next, copy all the folders from the **`Sample Folders`** directory into the following locations:

- Paste them inside `1 - Pre Event`
- Paste them inside `2 - Post Event`

After that:

1. Put all required inputs into the `Step-1 -- Model Inputs` folder.
2. Open **QGIS**.
3. Add the inputs as layers in your project.
4. Run the model in the **QGIS Model Designer** (Graphical Modeler).

The model will populate all the empty folders with the output data.

> ⚠️ **Note:** Please make the respective changes in the model for the **Post-Event** setup as well.



