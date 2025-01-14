This is an application which provides a map of South Africa based on the 1:50,000 topographic map sheets published by the Chief Directorate: National Geospatial Information. The map can be seen in action at https://ngimaps.frith.dev/.

## How it works

The 1,912 individual sheets of the 1:50,000 topographical map series are available as GeoTIFFs from the [CDNGI Geospatial Portal](http://www.cdngiportal.co.za/cdngiportal/). I downloaded these maps, warped them to Web Mercator projection, and trimmed off the marginalia so that just the map itself remains. The resulting files are available as [Cloud Optimized GeoTIFFs](https://www.cogeo.org/) (COGs) at https://mapimages.frith.dev/50k/.

These COGs are assembled into a [MosaicJSON](https://github.com/developmentseed/mosaicjson-spec) file. A small Python service based on [TiTiler](https://developmentseed.org/titiler/) renders this layer into XYZ ("slippy") tiles. An instance of [MapProxy](https://mapproxy.org/) sits in front of the tiler; it stores rendered tiles in an on-disk cache. Finally, an instance of NGINX sits in front of it all, serving cached tiles directly from the disk and otherwise passing the requests on to MapProxy. The NGINX service also serves the [Leaflet](https://leafletjs.com/)-based frontend for viewing the map.

Note: generating tiles for zoom levels < 9 can be very demanding as it entails assembling many individual sheets into a tile. If you deploy this stack, I suggest pre-seeding those levels before going live.