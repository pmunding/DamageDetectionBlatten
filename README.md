# DamageDetectionBlatten

Damage Detection of buildings caused by the Blatten glacier. On 28th of May, the Blatten glacier collapsed and caused a lot of destruction. The debris caused by glacier meltdown destroyed a large part of the village, most of the house were buried under the 

This repository contains all the work flow of the damage detection analysis in the form of R markdown file, shape files, QGIS trained model, and images.

---

The process of detecting the damaged building, can be seen in the [Image](4-Misc/Process.png). The initial filter analysis code can be scene in the [R markdown file](4-Misc/Swiss_Blatten_glacier_damage_analysis.Rmd). 

The Process is divided into two parts Pre and Post-event, both have same steps, the only difference is the pre has one extra step "Compactness", as this factor is important for the the detection of houses and roads not the glacier debris or the lake. The Results folder contains the end results.

# Steps:

## Step - 0 -- AOI (Area of Interest)

Our Area of interest is within the coordinates: **46.422532, 7.797989** and **46.406229, 7.829559**. The data has been retrived from the offical site of **Federal Office of the Topography swisstopo Rapid Mapping** [link](https://www.rapidmapping.admin.ch/index_de.html)

Due to limited computational resources, we clipped the area of the village where the damage has occured the most, [pre](1-Pre-event/Step-0%20--%20AOI/19_5_AOI.tif) and [post](2-post-event/Step-0%20--%20AOI/30_5_AOI.tif).

## Step - 1 -- Filters

We have applied Laplacian filter with value 3 on band 1, [pre](1-Pre-event/Step-1%20--%20Filters/pre_laplacian3_ch_1.tif) and [post](2-Post-event/Step-1%20--%20Filters/post_laplacian3_ch_1.tif), and then also on band 3 of [pre](1-Pre-event/Step-1%20--%20Filters/pre_laplacian3_ch_3.tif) and [post](2-Post-event/Step-1%20--%20Filters/post_laplacian3_ch_3.tif) event.

Then we applied the GLCM texture filter on the band 1 of both the [pre](1-Pre-event/Step-1%20--%20Filters/Pre_GLCM_(dissimilarity).tif) and [post](2-Post-event/Step-1%20--%20Filters/Post_GLCM_(dissimilarity).tif) event.


## Step - 2 -- Clipping of Inputs

We removed the unnecessary surrounding part of the the main RGB image and also the filtered images:

[Here](1-Pre-event/Step-2%20--%20Clipping%20of%20Inputs/) are all the clipped images for the pre event.
[Here](2-Post-event/Step-2%20--%20Clipping%20of%20Inputs/) are all the clipped images for the post event.

## Step - 3 -- Final input image

After clipping we have stacked all the four images to form a 6 band raster image for both the scenario: [Pre-event](1-Pre-event/Step-3%20--%20Final%20Input%20Image/Pre-Event%206%20Band%20Raster.tif) and [Post-event](2-Post-event/Step-3%20--%20Final%20Input%20Image/Post-Event%206%20Band%20Raster.tif).

## Step - 4 -- Segmenetation 

The final stacked image is fed to the segmentation algorithm in QGIS with the parameter *Spatial Radius* as 15 and *Range Radius* as 14. The segemenation output shape files for the pre event are [here](1-Pre-event/Step-4%20--%20Segmentation/) and for the post event are [here](2-Post-event/Step-4%20--%20Segmentation/).

## Step - 5 -- Zonal Statistics

## Step - 6 -- Add Compactness

## Step - 7 -- Join Labels

## Step - 8 -- Model Training

## Step - 9 -- Classes

## Step - 10 -- Specific class

