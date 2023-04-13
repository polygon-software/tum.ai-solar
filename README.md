# tum.ai Solar Challenge

## Motivation

The future of energy is green! To accelerate the clean energy revolution, we must use the resources we have in the most effective manner. Last year, 50% more solar panels were installed on german roofs than the year before. While this is great news, it does not paint the full picture. Due to supply chain limitations and skill shortage, most home owners in germany have to wait up to 1 year for a consultation appointment with solar roof experts. For an installation, the waiting periods are even crazier: Waiting times for 2-3 years before a solar roof can be installed has become the new normal. The current solar market does not scale with the demand!

That's why we must ensure that the limited solar panel resources we have are used in the most efficient way possible! Sadly, the current system is not at all efficient. Solar construction companies serve their customers on a first-come-first-serve basis: Whoever requests the solar panel first gets the panel installed first. This inherently means that solar installations on large roofs at location with optimal sun conditions are often postponed in favor of installations locations that are less well-suited, simply due to the fact that one requests came in before the other. 

To optimize the wellfare accross germany, we should turn this process around and first evaluate which houses in germany are best suited for solar panels. Then, we should panel the best suited houses first, getting the most efficiency out of each newly installed panel, before moving on to less optimal houses. With limited solar panels available, maximizing the efficiency of each panel is crucial and the only way to maximize the clean energy produced in the country. 

Luckily, we as Data Scientists can solve this challenge using openly available data! Using datasets from the german weather insitution [Deutscher Wetterdienst - DWD](https://www.dwd.de/), we can find out which regions in germany have the most sun. With [Püem Street Maps - OSM)[https://www.openstreetmap.org/about], we have a crowd sourced dataset available containing detailed information for any building accross the country, allowing us to run efficiently analysis on individual buildings at scale. Lastly, using Satelite Image Data from commercial vendors like [Google Maps Platform - GMP](https://developers.google.com/maps), we can even run computer vision algorithms on high quality satelite imagery to run fine grained analysis on individual houses. 

## Key Questions

With so much data publicly available, we can answer many solar related questions that are important for the clean energy future of germany, like:

- How many german roofs must be equipped with solar panels to subtitute all of germanies focile energy sources?
- In which regions of germany should solar be substituted the most?
- Where are the 100 buildings in germany with the largest roofs and the best efficiency per square meter?
- How much electricity can the home owner of the house at location long/lat produce per year?

## Background Knowledge

### Sun Radiations dependence on geographic location

The more sun radiation a solar panel receives, the more energy it produces. Although this curve might not be linear, it is still monotonic. Hence, to find out how efficient a rooftop is, knowing how much sun radiation the roof receives is an important step. The amount of sunlight hitting a unit squared area is measured in kWh/m^2. Solar panels can not convert 100% of this energy into power, though. They typically have an efficiency factor of 0.15 - 0.2. How much solar energy lands on a square meter depends on the geographic location of the house. Generally, the closer a house is to the equator, the more energy does the sun have per square meter due to the angle between the ground and the sunrays.

![Solar Irradiance](https://upload.wikimedia.org/wikipedia/commons/5/55/Seasons.too.png)
[Source](https://en.wikipedia.org/wiki/Solar_irradiance#Projection_effect).

### Panel efficiency dependence on roof Azimuth and Tilt

The azimuth and tilt of a roof influences the efficiency of the installed solar panels. Positioning a solar panel on a roof that faces the true south, with a roof that is slightly tilted, yields the most efficient conversion between incoming sun radiation and generated electricity. 

![Azimuth and Tilt](https://i0.wp.com/www.prostarsolar.net/wp-content/uploads/2020/10/Solar-Panel-angle.jpg?resize=1024%2C454&ssl=1)
[Source](https://www.prostarsolar.net/article/how-to-set-solar-panel-angle-to-sun.html)

### Roof Size

Installing a multitude of solar panels on one roof is much cheaper and labor efficient than installing single solar panels on multiple roofs. Panels must not only be installed, but also cleaned and maintained regularly, which is currently a limiting factor due to labor shortage. Hence, installing as many panels on a single roof as possible reduces the time and energy needed for maintenance workers to travel between locations. Therefore, the bigger the roof, the better. 

## Datasets

### Sun Radiation

The German Weather Institude has a large collection of open source datasets, including one that includes the total radioation: [Gridded annual sum of incoming shortwave radiation (global radiation) on the
horizontal plain for Germany based on ground and satellite measurements](https://opendata.dwd.de/climate_environment/CDC/grids_germany/annual/radiation_global/DESCRIPTION_gridsgermany_annual_radiation_global_en.pdf)

Their [Dataset for 2022](https://opendata.dwd.de/climate_environment/CDC/grids_germany/annual/radiation_global/grids_germany_annual_radiation_global_2022.zip) describes a grid of 1 km^2 resolution containing radiation measurements accross germany for the year 2022. The PDF linked above contains detailed descriptions how the data can be interpreted. Note that the data is stored in the [Gauss-Krueger Coordiante System](https://gfzpublic.gfz-potsdam.de/rest/items/item_8827_5/component/file_130038/content). You can refer to the coding hints below to find out how you can convert between regular longitude-latitude coordinates and the Gaus-Krueger Coordinates. 

### Building Information

[Open Street Map](https://www.openstreetmap.org/about/en) is a community driven map data provider that also contains [3D Building Information](https://osmbuildings.org/?lat=48.14907&lon=11.56744&zoom=16.0&tilt=30). 
In OSM Buildings, buildings are defined in various Level of Details (LOD). For some buildings, only their base outline is available as a 2D Polygon. However, most buildings also contain height information such that the 3D baseline polygon can be extruded to a 3D cubic form. Only for some buildings, more detail is given, such as detailed information about balconies or windows. However, such details are only given for famous buildings that OSM Contributors mostly modelled by hand. 
We can determine the size of a rooftop even from the LOD0 simply by calculating the area of the polygonial building outline. It might not be a perfect representation, but a good approximation. 
All OSM Data is Open to the public and can be requested through various open APIs, such as the [OSM Buildings API](https://osmbuildings.org/documentation/data/). 
However, you can also download all OSM Data for offline use rom providers like [Geofabrik](https://download.geofabrik.de/europe.html) and process the data offline. This is extremely useful for our case, as spaming the OSM API for millions of buildings accross germany would certainly result in rate limits or even IP blocks. All available OSM Data accross [Germany](https://download.geofabrik.de/europe/germany.html) is 3.8 GB in size, and includes not just Building Data but all data OSM has about germany, including addresses, streets, and much more. Note that you can also just download and process the data for a certain region in germany to speed up your development. The smallest region is [Bremen](https://download.geofabrik.de/europe/germany/bremen.html) with only 18 MB in size.
To find out how to work with the OSM data in Python, refer to the coding hints section below.

![OSM LOD](https://osmbuildings.org/blog/2018-02-28_level_of_detail/lod.png)

### Satelite Information




