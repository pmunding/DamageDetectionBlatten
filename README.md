# DamageDetectionBlatten

Damage Detection of buildings caused by the Blatten glacier. On 28th of May, the Blatten glacier collapsed and caused a lot of destruction. The debris caused by glacier meltdown destroyed a large part of the village, most of the house were buried under the rumble. 

This repository contains all the work flow of the damage detection analysis in the form of R markdown file, shape files, QGIS trained model, and images.

## View Analysis
R code for the initial analysis: <br>
[Rmd file](Rmd/Swiss_Blatten_glacier_damage_analysis.Rmd)


# Steps:

## AOI (Area of Interest)

Our Area of interest is within the coordinates: **46.422532, 7.797989** and **46.406229, 7.829559**. The data has been retrived from the offical site of **Federal Office of the Topography swisstopo Rapid Mapping** [link](https://www.rapidmapping.admin.ch/index_de.html)

Due to limited computational resources, we clipped the area of the village where the damage has occured the most. The clipped images can been viewed:<br>
[Pre-event](AOI/19_5_AOI.tif) <br>
[Post-event](AOI/30_5_AOI.tif)

## 1 (Spatial Filters and Texture analysis)

We have applied Laplacian filter with value 3 on band 1 and band 3 of both the images. <br>
Pre-event images: 
[band 1](1/laplacian_filter_ch_1/pre_laplacian3_ch_1.tif) and
[band 3](1/laplacian_filter_ch_3/pre_laplacian3_ch_3.tif) <br>
Post-event images: 
[band 1](1/laplacian_filter_ch_1/post_laplacian3_ch_1.tif) and
[band 3](1/laplacian_filter_ch_3/post_laplacian3_ch_3.tif)

Then we applied the GLCM texture filter on the band 1 of both the pre and post event images:

[pre-event image](1/GLCM_ch_1/pre.tif) <br>
[post-event image](1/GLCM_ch_1/post.tif)

## 2 (Stacking)

After the filters have been applied, we stacked up the original images with the laplacian filters of both bands 1 and 3 and also the GLCM output and form a 6 band image for both pre and post event.

[pre-event image](2/6_band_image/pre_6_band.tif) <br>
[post-event image](2/6_band_image/post_6_band.tif)

Later, we removed the unnecessary surrounding part of the final image as well:

[pre-final](2/clipped_6_band_image/pre_6_band_clipped.tif) <br>
[post-final](2/clipped_6_band_image/post_6_band_clipped.tif)