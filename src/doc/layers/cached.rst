.. _openlayers.layers.cached:

Cached Tiles
============

By default, the Tile layer makes requests for 256 x 256 (pixel) images to fill your map viewport and beyond. As you pan and zoom around your map, more requests for images go out to fill the areas you haven't yet visited. While your browser will cache some requested images, a lot of processing work is typically required for the server to dynamically render images.

Since tiled layers make requests for images on a regular grid, it is possible for the server to cache these image requests and return the cached result next time you (or someone else) visits the same area - resulting in better performance all around.


.. _openlayers.layers.cached.xyz:

ol.source.XYZ
--------------------

The Web Map Service specification allows a lot of flexibility in terms of what a client can request. Without constraints, this makes caching difficult or impossible in practice.

At the opposite extreme, a service might offer tiles only at a fixed set of zoom levels and only for a regular grid. These can be generalized as tiled layers with an XYZ source - you can consider X and Y to indicate the column and row of the grid and Z to represent the zoom level.


.. _openlayers.layers.cached.osm:

ol.source.OSM
--------------------

The `OpenStreetMap (OSM) <http://www.openstreetmap.org/>`_ project is an effort to collect and make freely available map data for the world. OSM provides a few different renderings of their data as cached tile sets. These renderings conform to the basic :ref:`XYZ grid <openlayers.layers.cached.xyz>` arrangement and can be used in an OpenLayers map. The ``ol.source.OSM`` layer source accesses OpenStreetMap tiles.

.. _openlayers.layers.cached.example:

.. rubric:: Tasks

#.  Open the ``map.html`` file from the :ref:`previous section <openlayers.layers.wms>` in a text editor and change the map initialization code to look like the following:
    
    .. code-block:: html

        <script>
          var map = new ol.Map({
            target: 'map',
            renderer: ol.RendererHint.CANVAS,
            layers: [
              new ol.layer.Tile({
                source: new ol.source.OSM()
              })
            ],
            view: new ol.View2D({
              center: ol.proj.transform([-93.27, 44.98], 'EPSG:4326', 'EPSG:3857'),
              zoom: 9
            })
          });
        </script>

#.  In the ``<head>`` of the same document, add a few style declarations for the layer attribution.
    
    .. code-block:: html
    
        <style>
            #map {
                width: 512px;
                height: 512px;
            }
            .ol-attribution ul, .ol-attribution a {
                color: black;
            }
        </style>

#.  Save your changes, and refresh the page in your browser: @workshop_url@/map.html

.. figure:: cached1.png
   
    A map with a tiled layer with an OpenStreetMap source.


A Closer Look
~~~~~~~~~~~~~

Projections
```````````
Review the view definition of the map:

.. code-block:: javascript

    view: new ol.View2D({
      center: ol.proj.transform([-93.27, 44.98], 'EPSG:4326', 'EPSG:3857'),
      zoom: 9
    })

Geospatial data can come in any number of coordinate reference systems. One data set might use geographic coordinates (longitude and latitude) in degrees, and another might have coordinates in a local projection with units in meters. A full discussion of coordinate reference systems is beyond the scope of this module, but it is important to understand the basic concept.

ol3 needs to know the coordinate system for your data. Internally, this
is represented with an ``ol.proj.Projection`` object. The ``transform`` function in the ``ol.proj`` namespace also takes strings that represent the coordinate reference system (``"EPSG:4326"`` and ``"EPSG:3857"`` above).

Locations Transformed
`````````````````````

The OpenStreetMap tiles that we will be using are in a Mercator projection. This is also the default map projection in OpenLayers. Because of this, we need to set the initial center using Mercator coordinates. Since it is relatively easy to find out the coordinates for a place of interest in geographic coordinates, we use the ``ol.proj.transform`` method to turn geographic coordinates (``"EPSG:4326"``) into Mercator coordinates (``"EPSG:3857"``).

Custom Map Options
``````````````````

.. note::

    The projections we used here are the only two projections that ol3 knows
    about. For other projections, we need to configure the projection. The
    easiest way to do this is to use the Proj4js library, which enables
    OpenLayers to transform coordinates between other projections. This is an
    example for a Swiss projection:

.. code-block:: javascript

    Proj4js.defs['EPSG:21781'] = '+title=CH1903 / LV03 +proj=somerc +lat_0=46.95240555555556 +lon_0=7.439583333333333 +x_0=600000 +y_0=200000 +ellps=bessel +towgs84=674.374,15.056,405.346,0,0,0,0 +units=m +no_defs';
    var projection = ol.proj.configureProj4jsProjection({
      code: 'EPSG:21781',
      extent: [485869.5728, 76443.1884, 837076.5648, 299941.7864]
    });

And we need to include a script tag for the Proj4js library:

.. code-block:: html

    <script src="http://cdnjs.cloudflare.com/ajax/libs/proj4js/1.1.0/proj4js-compressed.js" type="text/javascript"></script>

This information can be looked up at http://spatialreference.org/, using the EPSG code.

Layer Creation
``````````````

.. code-block:: javascript

    layers: [
      new ol.layer.Tile({
        source: new ol.source.OSM()
      })
    ],

As before, we create a layer and add it to the layers array of our map config object. This time, we accept all the default options for the source.

CSS
```

.. code-block:: html

    .ol-attribution ul, .ol-attribution a {
      color: black;
    }

A treatment of map controls is also outside the scope of this module, but these style declarations give you a sneak preview. By default, an ``ol.control.Attribution`` control is added to all maps. This lets layer sources display attribution information in the map viewport. The declarations above alter the style of this attribution for our map (notice the small Copyright line at the bottom left of the map).

Having mastered layers with publicly available cached tile sets, let's move on to working with :ref:`proprietary layers <openlayers.layers.proprietary>`.

