.. _openlayers.controls.draw:

Features neuzeichnen
====================

Neue Features kann man mittels einer ``ol.interaction.Draw`` zeichnen. Eine 
solche ``Draw``-Interaktion benötigt beim initialisieren eine Vekto ``source``
und einen Geometrietyp.

Vektorlayer und ``Draw``-Interaktion erzeugen
`````````````````````````````````````````````

.. rubric:: Übungen

#.  Wir beginnen mit dem unten aufgeführten Beispiel. Öffnen Sie die Datei
    ``map.html`` im Texteditor und stellen Sie sicher, dass der Inhalt etwa wie
    folgt aussieht.
    
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
            <script src="ol3/ol.js" type="text/javascript"></script>
            <title>OpenLayers 3 example</title>
          </head>
          <body>
            <h1>My Map</h1>
            <div id="map"></div>
            <script type="text/javascript">
              var source = new ol.source.GeoJSON({
                url: 'data/layers/7day-M2.5.json'
              });
              var draw = new ol.interaction.Draw({
                source: source,
                type: 'Point'
              });
              var map = new ol.Map({
                interactions: ol.interaction.defaults().extend([draw]),
                target: 'map',
                renderer: 'canvas',
                layers: [
                  new ol.layer.Tile({
                    title: "Global Imagery",
                    source: new ol.source.TileWMS({
                      url: 'http://maps.opengeo.org/geowebcache/service/wms',
                      params: {LAYERS: 'bluemarble', VERSION: '1.1.1'}
                    })
                  }),
                  new ol.layer.Vector({
                    title: 'Earthquakes',
                    source: source,
                    style: new ol.style.Style({
                      image: new ol.style.Circle({
                        radius: 5,
                        fill: new ol.style.Fill({
                          color: '#0000FF'
                        }),
                        stroke: new ol.style.Stroke({
                          color: '#000000'
                        })
                      })
                    })
                  })
                ],
                view: new ol.View2D({
                  projection: 'EPSG:4326',
                  center: [0, 0],
                  zoom: 1
                })
              });
            </script>
          </body>
        </html>
        
#.  Speichern Sie Ihre Änderungen und laden Sie die Seite im Browser
    neu: @workshop_url@/map.html
    
    Klicken Sie mit der linken Maustaste auf die Karte, um neue Features
    hinzuzufügen.

    .. figure:: draw1.png
   
       Featureerzeugung mittels einer ``Draw``-Interaktion.

.. rubric:: Zusatzaufgabe

#.  Erzeugen Sie einen `Eventhandler` der jeweils für ein neu gezeichnetes
    Feature dessen Koordinaten auf der Konsole ausgibt. (Tipp: der Event
    lautet `drawend`).

.. only:: instructor

    .. code-block:: javascript

        draw.on('drawend', function(evt) {
          window.console.log(evt.feature.getGeometry().getCoordinates());
        });

