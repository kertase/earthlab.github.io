---
layout: single
category: courses
title: "Plot Different Types of Data Using Open Source Python"
permalink: /courses/scientists-guide-to-plotting-data-in-python/python-plotting-activities/
week-landing: 3
dateCreated: 2020-06-24
modified: 2020-06-24
week: 3
sidebar:
  nav:
comments: false
author_profile: false
course: 'scientists-guide-to-plotting-data-in-python-textbook'
module-type: 'session'
---
{% include toc title="Section Three" icon="file-text" %}

<div class="notice--info" markdown="1">

## <i class="fa fa-ship" aria-hidden="true"></i> Section Three - Open Source Python Plotting Activities 

In section two of this textbook, you will test your skills with plotting different 
types of data using open source **Python**. 


## <i class="fa fa-graduation-cap" aria-hidden="true"></i> Learning Objectives

After completing this section of the Scientist's Guide to Plotting Data in Python online textbook, you will be able to:

* Plot different types of data using open source python approaches. 

</div>

{% include textbook-section-toc.html %}


<div class='notice--success' markdown="1">

## <i class="fa fa-graduation-cap" aria-hidden="true"></i> Challenge: Plot Different Types of Data Using Open Source Python

Throughout these chapters, you've learned a lot on how to plot many different types of data. You've used a combination of multiple libraries to plot data, including **pandas**, **geopandas**, **matplotlib**, **earthpy**, and various others. This challenge will challenge you to use the appropriate packages and tools to plot each type of data that have been covered so far. First, you will plot out time series data, then various forms of vector data, then raster data, and finally you will plot a combination of the raster and vector data. 
</div>

{:.input}
```python
# Import Packages

import os
from glob import glob
import matplotlib.pyplot as plt
import seaborn as sns
import pandas as pd
import geopandas as gpd
import rasterio as rio
from rasterio.plot import plotting_extent
import folium
import earthpy as et
import earthpy.plot as ep
import earthpy.spatial as es

import warnings
warnings.filterwarnings('ignore', 'GeoSeries.notna', UserWarning)

# Add seaborn general plot specifications
sns.set(font_scale=1.5, style="whitegrid")
```

<div class="notice--warning" markdown="1">

## <i class="fa fa-pencil-square-o" aria-hidden="true"></i> Challenge 1: Plot Time-Series Data

The first plot that you will create will show the global loss of glaciers from 1945
to the present using NOAA data. To make this plot, you will have to do the following: 

1. Read in the `.csv` using the API link: `https://datahub.io/core/glacier-mass-balance/r/glacier-mass-balance_zip.zip` using **pandas** to create a `DataFrame`.
2. Parse the dates from the `.csv` file. Assign the date column to be a `DataFrame` index.
3. Plot your data making sure datetime is on the x-axis and `Mean cumulative mass balance` column is on the y-axis. 
4. Set an appropriate xlabel, ylabel, and plot title. 
5. Open and look at the metadata found in the `README.md` file of your download, to find out what the units for the `Mean cumulative mass balance` are.

****

For help with this challenge, see your previous activities involving time series, or [the time series chapters](https://www.earthdatascience.org/courses/use-data-open-source-python/use-time-series-data-in-python/) from the EarthLab website. 

</div>

# TODO Make links data tips outside of the challenge box

{:.input}
```python
# Download the data & Set your working directory
et.data.get_data(
    url="https://datahub.io/core/glacier-mass-balance/r/glacier-mass-balance_zip.zip")
os.chdir(os.path.join(et.io.HOME, "earth-analytics", "data"))
```

{:.output}
    Downloading from https://datahub.io/core/glacier-mass-balance/r/glacier-mass-balance_zip.zip
    Extracted output to /root/earth-analytics/data/earthpy-downloads/glacier-mass-balance_zip





{:.output}
{:.display_data}

<figure>

<img src = "{{ site.url }}/images/courses/scientists-guide-to-plotting-data-in-python-textbook/03-plotting-activities/2020-06-24-plot-data-activity/2020-06-24-plot-data-activity_8_0.png">

</figure>




<div class="notice--warning" markdown="1">

## <i class="fa fa-pencil-square-o" aria-hidden="true"></i> Challenge 2: Create a Map Of RMNP Trails for the South Zone of the Park

The second plot you are going to make is a vector plot that displays 
hiking trails in Rocky Mountain National Park (RMNP) - which is located in Colorado. 

To make this plot, do the following: 

1. Read in the trails, trailsheads and ranger zones data for the Rocky Mountain National Park trails and the National Parks Service boundaries. The code to open the data is in the cell below for you to run.
2. Create a new GeoDataFrame that only contains the South Zone of the RMNP ranger zones `ranger_zones[ranger_zones["SUBDISTRIC"] == "SOUTH"]` data  for the park.
3. Change the Coordinate Reference System of the boundary dataset to match the trails dataset. To get the Coordinate Reference System of the trails dataset, you can use `trails_dataframe_name.crs`. To set it to the boundary dataset, you can use the `to_crs()` function like so: `boundary_dataframe_name.to_crs(trails_dataframe_name.crs)`
4. Plot the trails data and the trailheads data together in a map. 
5. Add the polygon boundary of the South Ranger zone to the map.  
6. Add a title to your plot. 

</div>

****

# Turn this into a data tip BELOW this challenge div
For help with this challenge, see your previous activities involving vector plotting, [this chapter explaining reprojecting data into a new CRS](https://www.earthdatascience.org/courses/use-data-open-source-python/use-time-series-data-in-python/) from the EarthLab website, or [this chapter covering vector and legend plotting](https://www.earthdatascience.org/courses/scientists-guide-to-plotting-data-in-python/plot-spatial-data/customize-vector-plots/python-customize-map-legends-geopandas/). 




# TODO -- read in the trails as a URL to keep code simpler

{:.input}
```python
# Open RMNP Boundary -- we might not need this
# RMNP Boundary Layer: Lat lon 4326 -- direct download
rmnp_boundary = gpd.read_file(
    "https://opendata.arcgis.com/datasets/7cb5f22df8c44900a9f6632adb5f96a5_0.geojson")

```

{:.input}
```python
# Trails are UTM Zone 13N --  reproject to lat long 
rmnp_trails = gpd.read_file("https://opendata.arcgis.com/datasets/e1e0bcb87eb94960bc04f76e03936385_0.geojson")
rmnp_trails_4326 = rmnp_trails.to_crs(rmnp_boundary.crs)

```

{:.input}
```python
# Open ranger zones  - EPSG 4326
ranger_zones = gpd.read_file(
    "https://opendata.arcgis.com/datasets/fdffd3272ba546da9176416b814d2e8f_0.geojson")

# Trailheads - 4326
trailheads = gpd.read_file(
    "https://opendata.arcgis.com/datasets/55748f2f1d8a4db7aa26f7549e74be57_0.geojson")
```

{:.input}
```python
# Subset trails to include only trails for ranger subdistrict SOUTH
south_zone = ranger_zones[ranger_zones["SUBDISTRIC"] == "SOUTH"]

# Clip the trails and trailheads data
trails_south = gpd.clip(rmnp_trails_4326, south_zone)
trailheads_south = gpd.clip(trailheads, south_zone)
```

{:.input}
```python
# Add your code to create this plot here 
```


{:.output}
{:.display_data}

<figure>

<img src = "{{ site.url }}/images/courses/scientists-guide-to-plotting-data-in-python-textbook/03-plotting-activities/2020-06-24-plot-data-activity/2020-06-24-plot-data-activity_19_0.png">

</figure>




<div class="notice--warning" markdown="1">

## <i class="fa fa-pencil-square-o" aria-hidden="true"></i> Challenge 3: Overlay a Terrain Model on top of a Hillshade  

Next you will create a base map to put your trails on. To begin, open up the Digital Elevation Model)USGS_1_n41w106.tif (`USGS_1_n41w106.tif`) for RMNP.

# TODO  add data to figshare and add other layers too
* Add this data on figshare and i think it might be nice to add the other layers too and reproject for them 
* this is not a reprojection activity
* Add a note about the NED data - they are using the NED 30m data which you can get from national map. (1 arc second data)

1. Read in the USGS DEM into a numpy array using rasterio. 
2. Create a hillshade from the USGS DEM using the syntax: `es.hillshade(dem-numpy-array-name-here)`
3. Overlay the DEM o top of the hillshade 
4. Be sure to add a title to your plot .

</div>

# TODO -- make the code below into a data tip
For help with this challenge, please see [this chapter](https://www.earthdatascience.org/courses/scientists-guide-to-plotting-data-in-python/plot-spatial-data/customize-raster-plots/) on the EarthLab website covering raster data plots, as well as your previous lessons on plotting raster data. 

# TODO -- update instructions so they know to add a 

{:.input}
```python
# All these data are 4326 so reprojecting to that above makes sense
et.data.get_data(url="https://ndownloader.figshare.com/files/23399609")
ned_30m_path = os.path.join("earthpy-downloads", "USGS_1_n41w106.tif")
```

{:.output}
    Downloading from https://ndownloader.figshare.com/files/23399609




{:.output}
{:.display_data}

<figure>

<img src = "{{ site.url }}/images/courses/scientists-guide-to-plotting-data-in-python-textbook/03-plotting-activities/2020-06-24-plot-data-activity/2020-06-24-plot-data-activity_22_0.png">

</figure>




<div class="notice--warning" markdown="1">

## <i class="fa fa-pencil-square-o" aria-hidden="true"></i> Challenge 4: Overlay Trails on top of Elevation Data.

Next you will overlay the trails data on top of the elevation data that you plotted above.
Keep in mind a few things when you do this

1. You will need to create a plotting_extent object to use in the `extent=` plot parameter to ensure that your raster data line up with the vector data.
2. 

Be sure to add a title to your map. 

</div>

For help with this challenge, please see [this chapter](https://www.earthdatascience.org/courses/scientists-guide-to-plotting-data-in-python/plot-spatial-data/customize-raster-plots/plotting-extents/) on the EarthLab website covering plotting extents and overlaying datatypes. 


{:.output}
{:.display_data}

<figure>

<img src = "{{ site.url }}/images/courses/scientists-guide-to-plotting-data-in-python-textbook/03-plotting-activities/2020-06-24-plot-data-activity/2020-06-24-plot-data-activity_24_0.png">

</figure>




<div class="notice--warning" markdown="1">

## <i class="fa fa-pencil-square-o" aria-hidden="true"></i> Challenge 5: OPTIONAL Plot Interactive Vector Data

Create an interactive map of the RMNP trails in the SOUTH ranger zone using 
**folium**. To make this plot, you will have to do the following:

1. Convert the clipped trails `GeoDataFrame` (`trails_south`) into a `json` object with the `to_json()` method. 
2. Create a **folium** map with the location centered on `40.37244, -105.66472` and a starting zoom level of `10`. 
3. Turn your json lines into a folium feature with `folium.features.GeoJson()`. 
4. Add your lines to your folium map with `your_map_name.add_child(your_folium_feature_name)`. 
5. Display the map by calling the map object at the bottom of a notebook cell.

</div>

For help with this challenge, please see [this chapter](https://www.earthdatascience.org/courses/scientists-guide-to-plotting-data-in-python/plot-spatial-data/customize-vector-plots/interactive-leaflet-maps-in-python-folium/) on the EarthLab website covering interactive maps.


# TODOS BELOW -- 


{:.output}
{:.execute_result}






