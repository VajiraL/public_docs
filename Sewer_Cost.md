# Cost of Centralized Sewer Alternative


## Objective

Estimate the cost for providing full coverage of centralized sewer for the study area.

**Expected output**
 A raster layer of the area with the cost distribution. 
 
## Purpose
*How does this excercise fit in within the research framework ?*

End goal of the research to find out the mix of sanitation technologies ( types of technologies and their spatial fistribution) that would provide the *best* service. 

To describe what is *best*, I plan to use a criteria that evaluates the aspects of economic, social and environmental outcomes. 

I have started with the economic component, where the metric is the cost.
While I expect to consider the life cycle cost for a comparable time period for all alternatives, I have started by estimating the cost of construction which will nessesarily be a part of it.

## Method

Assessing the cost of a known pipe network is straight forward. But the interest in this study is to estimate the cost without knowing the layout of the network. 

For this purpose it may be possible find a relationship between the cost and a set of influencing factors for a known sewer network and apply that relationship to the area of interest. 

For a known sewer network, size and depth of each pipe segment usually decides the cost. These properties are governed by the inputs that goes into hydraulic design of the system, such as the road geometry, topography, locations of outfalls, population and its distribution.

The approach is to use regression analysis methods to find a relationship. Since it is expected to get the results as a spatial distribution, analysis will also be on spatial data. 

### Data

Cost and pipenetwork data obtained from the proposed Gravity Sewer Network of Ratmalana Moratuwa Wastewater Disposal Project, Sri Lanka.

**Cost data**

Cost in LKR for supplying and laying of uPVC pipes of sizes 200 mm to 600 mm at 0.5 m depth intervals up to 6.5 m. 

Format

Pipe Size|Depth|Cost
----------|-----|----
|200 mm| < 1.5 m| 20000|
|200 mm| 1.5 - 2 m| 22000|
|...|...|...|
|600 mm|6 - 6.5 m|75000|

**Pipe network data**
Shapefile layer with pipe network as each pipeline segment between two manholes as a line feature. 
In addition to geometry, each feature has the attributes like invert levels at both ends and the pipe size. 

### Approach 1 | Multiple linear regression

Although it is suspected that relationships may not be linear, this is assumed a good starting point. Also the data prepararion for this analysis will be reusable for other analyses. 

Independant variables: 
Elevation, Distance to a road, Distance along the road network to nearest pump station, population

QGIS3 is used. 

#### Data Preparation

**Assigning costs to each pipe segment**
1. Split the lines into 10 m segments
2. Calculate the depth of each segment using invert levels and the ground level from DEM
3. Refering the depth and pipe size, lookup the cost

**Cost Raster**
1. Extract center of each 10m segment
2. Create Voronoi Diagram around the points
3. Find the Cost density in each polygon by dividing the cost at each point by the area of the polygon
4. Rasterize the layer using the cost density as the burn-in value

**Distance to roads**
1. Get the OpenStreetMap road layer for the area
2. Rasterize the layer
3. Use raster proximity analysis to create a layer with distance to the roads

**Distance to Pump Stations**
1. Create a regularly spaced (30 m) grid of points
2. Use QNEAT3 network analysis plugin to generate the iso-distance layer, using OSM roads as the network



