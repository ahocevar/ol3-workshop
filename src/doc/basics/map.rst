.. _config.map:

Eine Karte erstellen
====================

Eine Karte besteht in ol3 aus einer Sammlung von Themen (*layers*)
und verschiedenen anderen Elementen, die Benutzerinteraktionen ermöglichen
und/oder verarbeiten (*interactions* und *controls*).

Um eine Karte zu erzeugen benötigen wir drei Bestandteile:

* :ref:`HTML-Markup (Auszeichnung) <config.dissect.markup>`,
* :ref:`CSS-Deklarationen (Stil) <config.dissect.style>` und 
* :ref:`JavaScript-Initialisierungs-Code (Verhalten) <config.dissect.code>`.

.. _config.map.example:

Ein lauffähiges Beispiel
------------------------

Schauen wir uns zunächst ein vollständig lauffähiges Beispiel einer
ol3-basierten Karte an: 

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

#.  Kopieren Sie obigen Text in eine neue Datei :file:`map.html`, die Sie im
    Verzeichnis ``ol3-ws/ol3-training-master`` abspeichern.

#.  Öffnen Sie die HTML-Seite inj einem Browser: @workshop_url@/map.html

.. figure:: map1.png
   
    Die lauffähige Karte im Browser.

Nachdem wir nun eine erste funktionsfähige Karte erzeugt haben, schauen wir uns
nun die :ref:`Einzelbestandteile <config.dissect>` an. 
