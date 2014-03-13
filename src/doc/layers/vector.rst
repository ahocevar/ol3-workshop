.. _openlayers.layers.vector:

Vektor Themen
=============
Vector Layers are represented by ``ol.layer.Vector`` and handle the client-side display of vector data. Currently OpenLayers 3 only supports vector rendering in the Canvas renderer.


Rendering Features Client-Side
------------------------------

Let's go back to the WMS example to get a basic world map.  We'll add some feature data on top of this in a vector layer.

.. code-block:: html

    <!doctype html>
    <html lang="en">
      <head>
        <link rel="stylesheet" href="ol3/ol.css" type="text/css">
        <style>
          #map {
            height: 256px;
            width: 512px;
          }
        </style>
        <title>OpenLayers 3 example</title>
        <script src="ol3/ol.js" type="text/javascript"></script>
      </head>
      <body>
        <h1>My Map</h1>
        <div id="map"></div>
        <script type="text/javascript">
          var map = new ol.Map({
            target: 'map',
            renderer: 'canvas',
            layers: [
              new ol.layer.Tile({
                title: "Global Imagery",
                source: new ol.source.TileWMS({
                  url: 'http://maps.opengeo.org/geowebcache/service/wms',
                  params: {LAYERS: 'bluemarble', VERSION: '1.1.1'}
                })
              })
            ],
            view: new ol.View2D({
              projection: 'EPSG:4326',
              center: [0, 0],
              zoom: 0,
              maxResolution: 0.703125
            })
          });
        </script>
      </body>
    </html>

.. rubric:: Tasks

#.  Open ``map.html`` in your text editor and copy in the contents of the initial WMS example. Save your changes and confirm that things look good in your browser: @workshop_url@/map.html


#.  In your map initialization code add another layer after the Tile layer (paste the following). This adds a new vector layer to your map that requests a set of features stored in GeoJSON:

    .. code-block:: javascript

        new ol.layer.Vector({
          title: 'Earthquakes',
          source: new ol.source.GeoJSON({
            url: 'data/layers/7day-M2.5.json'
          }),
          style: new ol.style.Style({
            image: new ol.style.Circle({
              radius: 3,
              fill: new ol.style.Fill({color: 'white'})
            })
          })
        })
    
.. figure:: vector1.png
   
    World map with white circles representing earthquake locations.

.. note::

    Since the GeoJSON data is in ``EPSG:4326`` and the map's view is also in ``EPSG:4326``, no reprojection is needed. In the case that the source projection differs from the view's projection, a ``projection`` property should be specified on the source which indicates the projection of the feature cache, this would mean you would specify the view's projection here normally.
    
A Closer Look
`````````````

Let's examine that vector layer creation to get an idea of what is going on.

.. code-block:: javascript

    new ol.layer.Vector({
       title: 'Earthquakes',
       source: new ol.source.GeoJSON({
        url: 'data/layers/7day-M2.5.json'
      }),
      style: new ol.style.Style({
        image: new ol.style.Circle({
          radius: 3,
          fill: new ol.style.Fill({color: 'white'})
        })
      })
    })

The layer is given the title ``"Earthquakes"`` and some custom options. In the options object, we've included a ``source`` of type ``ol.source.GeoJSON`` which points to a url.

.. note::

    In the case where you want to style the features based on an attribute, you would use a style function instead of an ``ol.style.Style`` for the ``style`` config option of ``ol.layer.Vector``.

.. rubric:: Bonus Tasks

#.  The white circles on the map represent ``ol.Feature`` objects on your ``ol.layer.Vector`` layer. Each of these features has attribute data with ``title`` and ``summary`` properties. Register a singleclick listener on your map that calls ``forEachFeatureAtPixel`` on the map, and displays earthquake information below the map viewport.

#.  The data for the vector layer comes from an earthquake feed published by the USGS (http://earthquake.usgs.gov/earthquakes/catalogs/).  See if you can find additional data with spatial information in a format supported by OpenLayers 3.  If you save another document representing spatial data in your ``data`` directory, you should be able to view it in a vector layer on your map.
