1. road data: 
- 1> download the osm data, which is much better than the shp data. https://download.bbbike.org/osm/bbbike/. OSM XML gzip'd will be the correct osm file. 
- 2> convert the lines of osm data to geojson data.
- 3> roadDataConverter to conver the geojson to the format in ttk. save the point and lines and the property of the line.
- 4> use ttk pipline/ttk extract to filter road data based on the property: the final road network contians: </br>
<img src = "./images/clipedData1.png" width = 100> <img src = "./images/clipedData2.png" width = 100>

2. crime data:
Seattle Crime: seattle police data.gz
Chicago Crime: old server database
Purdue Crime: old server database

3. Base map for the paraview
- 1> add QuipMapServices plugin in QGIS
- 2> find the suitable base map.
- 3> zoom in the map to your required zoom level. 
- 4> project/export/export map to image, select the bbox of the extent of the map.
- 5> create a plane in the paraview with the same bbox of the based map. Notice, p2, origina, and p1. While p1 and p2 for base map.
- 6> load the image to the texture of the plane and flip the texture. It's done.
