.. _openlayers.layers.proprietary:

Proprietäre Raster Themen
=========================

Die vorherigen Abschnitte verwendeten Layer, die auf dem standardisiertem
:abbr:`WMS (OGC Web Map Service)` oder eigenen Kachelsets basierten. Online
Kartenwendungen wurden aber nicht zuletzt durch die Verfügbarkeit von
proprietären Kacheldiensten so populär. ol3 stellt Layertypen zur Verfügung, die
mit jenen Diensten über deren API kommunizieren können.

In diesem Abschnitt werden wir das Beispiel aus dem
:ref:`vorherigen Abschnitt <openlayers.layers.cached>` erweitern, in dem wir
einen Layer hinzufügen, der die Bing-Kacheln als Quelle nutzt.

Um zu verstehen, wie ol3 zusammen mit Google Maps verwendet werden kann, sei auf
das entsprechende Online Beispiel unter 
http://ol3js.org/en/master/examples/google-map.html verwiesen.

.. _openlayers.layer.proprietary.bing:

Bing!
-----

Wir fügen nun also einen Bing-Layer hinzu:

.. rubric:: Übungen

#.  Suchen Sie in der Datei ``map.html`` die Stelle, an der die 
    :abbr:`OSM (OpenStreetMap)` ``source`` konfiguriert wird und
    ändern Sie jene zu einer ``ol.source.BingMaps``:

    .. code-block:: javascript

        source: new ol.source.BingMaps({
          imagerySet: 'Road',
          key: 'Ak-dzM4wZjSqTlzveKz5u0d4IQ4bRzVI309GxmkgSVr1ewS6iPSrOvOKhA-CJlm3'
        })

    .. note:: 
        
        Die Bing-API muss mit einem API-Schlüssel ``key`` angesprochen werden.
        Das vorliegende Beispiel verwendet einen solchen Schlüssel, der nicht 
        für Ihre eigenen Anwendungen verwendet werden sollte. Um selbst einen
        Schlüssel kann die Registrierung unter https://www.bingmapsportal.com/
        verwendet werden.
    
#.  Speichern Sie ihre Änderungen in der Datei ``map.html`` und testen Sie das
    Ergebnis im Browser: @workshop_url@/map.html
    
.. figure:: proprietary1.png
   
    Eine ol3-Karte, die die Bing-Kacheln verwendet.

Vollständiges Beispiel
``````````````````````

Die von Ihnen angepasste HTML-Datei ``map.html`` sollte etwa wie folgt aussehen:

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
        .ol-attribution ul,
        .ol-attribution a,
        .ol-attribution a:not([ie8andbelow]) {
          color: black;
        }
      </style>
      <script src="ol3/ol.js" type="text/javascript"></script>
      <title>OpenLayers 3 example</title>
    </head>
    <body>
      <h1>My Map</h1>
      <div id="map" class="map"></div>
      <script type="text/javascript">
        var map = new ol.Map({
          target: 'map',
          renderer: 'canvas',
          layers: [
            new ol.layer.Tile({
              source: new ol.source.BingMaps({
                imagerySet: 'Road',
                key: 'Ak-dzM4wZjSqTlzveKz5u0d4IQ4bRzVI309GxmkgSVr1ewS6iPSrOvOKhA-CJlm3'
              })
            })
          ],
          view: new ol.View2D({
            center: ol.proj.transform([-93.27, 44.98], 'EPSG:4326', 'EPSG:3857'),
            zoom: 9
          })
        });
      </script>
    </body>
  </html>

Als letzten Layertypus wollen wir uns 
:ref:`Vektorthemen <openlayers.layers.vector>` anschauen.
