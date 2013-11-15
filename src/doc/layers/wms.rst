.. _openlayers.layers.wms:

Web Map Service Layers
======================

When you add a layer to your map, the layer is typically responsible for
fetching the data to be displayed. The data requested can be either raster or
vector data. You can think of raster data as information rendered as an image on
the server side. Vector data is delivered as structured information from the
server and may be rendered for display on the client (your browser).

There are many different types of services that provide raster map data. This
section deals with providers that conform with the :abbr:`OGC
(Open Geospatial Consortium, Inc.)` `Web Map Service (WMS)
<http://www.opengeospatial.org/standards/wms>`_ specification.

Creating a Layer
----------------

We'll start with a fully working map example and modify the layers to get an
understanding of how they work.

Let's take a look at the following code:

.. _openlayers.layers.wms.example:

.. code-block:: html

    <!doctype html>
    <html lang="en">
      <head>
        <link rel="stylesheet" href="ol3/ol.css" type="text/css">
        <style>
          #map {
            height: 512px;
            width: 512px;
          }
        </style>
        <script src="ol3/ol.js" type="text/javascript"></script>
        <title>OpenLayers 3 example</title>
      </head>
      <body>
        <h1>My Map</h1>
        <div id="map"></div>
        <script type="text/javascript">
          var map = new ol.Map({
            target: 'map',
            renderer: ol.RendererHint.CANVAS,
            layers: [
              new ol.layer.Tile({
                title: "Global Imagery",
                source: new ol.source.TileWMS({
                  url: 'http://demo.opengeo.org/geoserver/wms',
                  params: {'LAYERS': 'NE1_HR_LC_SR_W_DR'}
                })
              })
            ],
            view: new ol.View2D({
              center: [0, 0],
              zoom: 1
            })
          });
        </script>
      </body>
    </html>


.. rubric:: Tasks

#.  If you haven't already done so, save the text above as ``map.html`` in the
    root of your workshop directory.

#.  Open the page in your browser to confirm things work:
    @workshop_url@/map.html

The ol.layer.Tile Constructor
------------------------------------

The ``ol.layer.Tile`` constructor gets an object literal of type ``ol.layer.TileOptions`` see: http://ol3js.org/en/master/apidoc/ol.layer.html#TileOptions
In this case we are providing the source key of the options with a ``ol.source.TileWMS``.
A human-readable title for the layer can be provided with the title key, but basically any arbitrary name for the key can be used here.
In ol3 there is a separation between layers and sources, in OpenLayers 2 this was all part of the layer.

The ol.source.TileWMS Constructor
------------------------------------
The ``ol.source.TileWMS`` constructor has a single argument which is defined by: http://ol3js.org/en/master/apidoc/ol.source.html#TileWMSOptions
The url is the online resource of the WMS service, and params is an object literal with the parameter names and their values. Make sure to put quotes around the parameter names.

.. code-block:: javascript

    layers: [
      new ol.layer.Tile({
        title: "Global Imagery",
        source: new ol.source.TileWMS({
          url: 'http://demo.opengeo.org/geoserver/wms',
          params: {'LAYERS': 'NE1_HR_LC_SR_W_DR'}
        })
      })
    ]


.. rubric:: Tasks

#.  This same WMS offers a layer named ``"opengeo:borders"``, which only shows
    the borders of the countries of the world. Change the value of 
    the ``LAYERS`` parameter from ``"world"`` to ``"opengeo:borders"``. 

    Your revised ol.layer.Tile Constructor should look like:
    
    .. code-block:: javascript

        new ol.layer.Tile({
          title: "Global Imagery",
          source: new ol.source.TileWMS({
            url: 'http://demo.opengeo.org/geoserver/wms',
            params: {'LAYERS': 'osm:osm'}
          })
        })


#.  Save your changes and reload the map:
    @workshop_url@/map.html

.. figure:: wms1.png
   
    A map displaying the ``"opengeo:borders"`` layer.

Having worked with dynamically rendered data from a Web Map Service, let's move
on to learn about :ref:`cached tile services <openlayers.layers.cached>`.
