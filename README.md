# [This is a work in progress] Mapping Corn, Soybeans and "others"

## Introduction

Understanding the types, quantities, and qualities of agricultural crops grown in a region is crucial for predicting yield, estimating import/export needs, 
and assessing factors like soil organic carbon or greenhouse gas emissions []. 
Government agencies and private companies have attempted to create models for determining crop types grown in a country.
Utilizing remote sensing of agricultural land through publicly available imagery from Earth-observing satellites proves to be a more economical approach compared to 
methods like nationwide surveys or direct field visits. For instance, the U.S. Department of Agriculture (USDA) annually produces a Crop Data Layer (CDL) with a
resolution of $30 \times 30 m^2$ for the continental U.S., mapping major and minor agricultural crops, and other land cover types [].

## The Problem 
The CDL is typically released towards the end of January in the following crop calendar, and its production takes approximately 13 months []. 
However, there is a demand for a faster turnaround, and considering that not all countries produce crop maps, this remains a relevant issue.

For our educational project, we simplify the problem by focusing on mapping only three crop classes: Corn, Soybeans, and "other." 
Furthermore, we limit our spatial analysis to Green County, IA.

Here's a satellite base map of the county. Notice the fields, roads, and the river through the middle:

![Green county satellite basemap](https://github.com/aliz237/corn_soy/blob/main/docs/study_area_satellite_j.jpg)

Once our model is deployed, we have the flexibility to experiment with the spatial extent. 
We can test it in a neighboring county where the model was not initially trained, or be more adventurous and evaluate its performance in a different state like Indiana or a southern state.

## Method

The main purpose of this project is to give an example of a pixel level supervised classification task using classical machine learning models under the Google Earth Engine (GEE) platform. We are not aiming for the state of art performance (just yet!). To summarize, given a set of input features (to be defined soon), we would like to classify each pixel as Corn, Soy or Other. 

### Labels 
As with any supervised classification model, training labels are needed to train our models. Ideally, real ground truth data from farmers of the Greene county would be available to us, but we suffice to use CDL to extract the labels for the trainingg pixels for our task (see the Introduction section).

Here's a visual of the study area, the original CDL classification, and the reclassified CDL:

![cdl](https://github.com/aliz237/corn_soy/blob/main/docs/study_area_and_cdl_j.jpg)

Note that the original CDL map contains many more classes than the three classes we are interested in. Those classes are are lumped into one "other" class and colored blue.

### Input data
A timeseries of the Red and Near Infra Red (NIR) band of Sentinel2 over a "typical" corn/soy growing season in midwest US is used as model inputs.

-  why Red and NIR?
-  why use a timeseries and not just a single image
-  issues that arise from mean weekly composites of the imagery
-  Cloudmasking is currently not done, for implementation ease, to be revisited

### Data cube
This is just the Input features together with the target labels grouped into a single multiband image. However the input features
are in the different coordinate system than the label image.

- were there any problems related to projection differences?

### Training Samples

A stratified sampling function implemented in GEE is used to extract $n (=300)$ samples of each class as training labels.
Ones coordinates are chosen on the map, the pixel (at $30 m$ resolution) whose centeroid is closest to the coordinate is sampled.

- why was this sampling used as oppsed to say, random sample of $n$ pixels.
- why sample to being with? Why not use all (or say 80% of the county pixels).
- are there better sampling procedures for this problem
- are they easy to implement in GEE
- limitations 

Here's a visual of the training sample overlayed on a satellite basemap of part of the county:

![training_samples_satellite_basemap](https://github.com/aliz237/corn_soy/blob/main/docs/training_samples_satellite_j.jpg)

Here's a visual of the training sample overlayed on CDL:

![training_samples_cdl_basemap](https://github.com/aliz237/corn_soy/blob/main/docs/training_samples_cdl_j.jpg)

### Model selection and fine tuning

The notebook is written in a way that the model can be configured and supplied to the train function. Any of the classical
machine learning models available in GEE can be used. This makes it easy to experiment with different models and compare their
prediction power on this toy problem. By default, a Random Forest using 30 trees is configured as the baseline.


## Prediction Results

Here's the original CDL map together with the predicted map using only 300 samples of each class:

- update with better result
- Add a confusion matrix for the best model

![pred1](https://github.com/aliz237/corn_soy/blob/main/docs/classification_result_300_samples_j.jpg)
  
## Conclusion

## Next Steps

## References
