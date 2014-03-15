.. _openlayers.controls.modify:

Features modifizieren
=====================

Um features zu verändern, werden wir eine ``ol.interaction.Select`` mit einer
``ol.interaction.Modify`` kombinieren. Diese Interaktion teilen sich einen
gemeinsamen ``ol.FeatureOverlay``.


Vektorlayer und ``Modify``-Interaktion erzeugen
```````````````````````````````````````````````

.. rubric:: Tasks

#.  Wir beginnen wieder mit dem unten aufgeführten Beispiel. Öffnen Sie die
    Datei ``map.html`` im Texteditor und stellen Sie sicher, dass der Inhalt
    etwa wie folgt aussieht.
    
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
              var overlay = new ol.FeatureOverlay({
                style: new ol.style.Style({
                  image: new ol.style.Circle({
                    radius: 7,
                      fill: new ol.style.Fill({
                      color: [0, 153, 255, 1]
                    }),
                    stroke: new ol.style.Stroke({
                      color: [255, 255, 255, 0.75],
                      width: 1.5
                    })
                  }),
                  zIndex: 100000
                })
              });
              var modify = new ol.interaction.Modify({ featureOverlay: overlay });
              var select = new ol.interaction.Select({ featureOverlay: overlay });
              var map = new ol.Map({
                interactions: ol.interaction.defaults().extend([select, modify]),
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
    
    Klicken Sie mit der linken Maustaste auf die Karte um ein Erdbeben
    auszuwählen (``interaction.Select``) ziehen Sie das Feature anschließend mit
    der Maustaste an eine neue Lokation (``interaction.Modify``)


Einige Details
``````````````

Schauen wir uns genauer an, wie wir Features editieren können.

.. code-block:: javascript

    var overlay = new ol.FeatureOverlay({
      style: new ol.style.Style({
        image: new ol.style.Circle({
          radius: 7,
          fill: new ol.style.Fill({
            color: [0, 153, 255, 1]
          }),
          stroke: new ol.style.Stroke({
            color: [255, 255, 255, 0.75],
            width: 1.5
          })
        }),
        zIndex: 100000
      })
    });
    var modify = new ol.interaction.Modify({ featureOverlay: overlay });
    var select = new ol.interaction.Select({ featureOverlay: overlay });


Wir erzeugen zwei Interaktionen, eine Instanz von ``ol.interaction.Select`` um
Features vor dem editieren auszuwählen, und eine Instanz von 
``ol.interaction.Modify`` um die Geometrien tatsächlich zu verändern. Beiden 
Interaktionen weisen wir die gleiche Instanz der Klasse ``ol.FeatureOverlay``
(mit spezifischen Stilangaben, die während Selektion und Modifikation wirksam 
sind) zu. Klickt man erneut, so wird das zunächst gewählte / editierte Feature
wieder im Stil des Layers gezeichnet.
