<html>

<head>
<link rel="stylesheet" href="https://unpkg.com/leaflet@1.7.1/dist/leaflet.css" integrity="sha512-xodZBNTC5n17Xt2atTPuE1HxjVMSvLVW9ocqUKLsCC5CXdbqCmblAshOMAS6/keqq/sMZMZ19scR4PsZChSR7A==" crossorigin=""/>
    <script src="https://unpkg.com/leaflet@1.7.1/dist/leaflet.js" integrity="sha512-XQoYMqMTK8LvdxXYG3nZ448hOEQiglfqkJs1NOQV44cWnUrBc8PkAOcXy20w0vlaXaVUearIOBhiXZ5V3ynxwA==" crossorigin=""></script>

<style>
//setting the map container to full-screen
html, body {
  height: 100%;
  margin: 0;
}
#mapid { height: 100%; width: 100%; }
</style>
</head>






<body>
<div id="mapid"></div>
<script>

//this is where we initialize our map with its starting attributes
var mymap = L.map('mapid')
//setting the start view to Whitehorse cause idk, gotta start somewhere
mymap.setView([60.7212, -135.0568], 5);
//last attribute here is the zoom level to start out with. Higher is closer. 5 is around province-sized. 10-13 is around city-sized.



//toggle basemap layers
var basemaps = {
    Topography: L.tileLayer.wms(' https://www.gebco.net/data_and_products/gebco_web_services/2019/mapserv?', {
        layers: 'GEBCO_Latest'
    }),

    'Ocean Basemap (ESRI)': L.tileLayer('https://server.arcgisonline.com/ArcGIS/rest/services/Ocean_Basemap/MapServer/tile/{z}/{y}/{x}', {
    }),

    'Basemap 2': L.tileLayer.wms('http://ows.mundialis.de/services/service?', {
        layers: 'OSM-Overlay-WMS'
    }),

    'Basemap 3': L.tileLayer.wms('http://ows.mundialis.de/services/service?', {
        layers: 'OSM-Overlay-WMS,TOPO-WMS'
    })
};

L.control.layers(basemaps).addTo(mymap);

basemaps.Topography.addTo(mymap);



//example marker w popup
var examplemark = L.marker([60.7212, -135.0568]).addTo(mymap);
examplemark.bindPopup("<b>Popup</b><br>I am an example popup").openPop


//example shp file (states) with hover effect
states <- readOGR("shp/cb_2013_us_state_20m.shp",
layer = "cb_2013_us_state_20m", GDAL1_integer64_policy = TRUE)

neStates <- subset(states, states$STUSPS %in% c( "CT","ME","MA","NH","RI","VT","NY","NJ","PA"
))

leaflet(neStates) %>%
addPolygons(color = "#444444", weight = 1, smoothFactor = 0.5,
opacity = 1.0, fillOpacity = 0.5,
fillColor = ~colorQuantile("YlOrRd", ALAND)(ALAND), highlightOptions = highlightOptions(color = "white", weight = 2,
   bringToFront = TRUE))



//example img file
var imageUrl = '/Users/maddy/Documents/business cards/BEST biznezss.jpg',
    imageBounds = [[60.7212, -135.0568], [61.7212, -136.0568]];
L.imageOverlay(imageUrl, imageBounds).addTo(mymap);



//sidebar w layer selection
//needs plugin - sidebarv2


//top-right search bar. Use this to filter specific surveys
//needs plugin - custom-searchbox


//select features using rectangle made by pointer
//needs plugin - selectareafeature


//return co-ordinates that mouse is hovering over at bottom-right of screen

</script>

</body>
</html>
