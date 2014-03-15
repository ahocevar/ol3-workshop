.. _openlayers.controls.select:

Features selektieren
====================

In den vorherigen Abschnitten zu Layern haben wir gelernt, das wir geographische
Entitäten (`Features`) als Vektoren in ol3 laden und dynamisch auf die Karte
zeichnen können. Einer der Vorteile von Vektordaten, ist die einfache 
Möglichkeit zur Interaktion mit jenen im Kartenclient. Wir wollen nun einen
Vektorlayer erzeugen, in dem Benutzer Features selektieren können und
Informationen zum ausgewählten Objekt erhalten.


Vektorlayer und ``Select``-Interaktion erzeugen
```````````````````````````````````````````````

.. rubric:: Übungen

#.  Wir beginnen mit dem lauffähigen Beispiel aus dem
    :ref:`vorherigen Abschnitt <openlayers.layers.vector>`. Öffnen Sie die Datei
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
              var map = new ol.Map({
                interactions: ol.interaction.defaults().extend([
                  new ol.interaction.Select({
                    featureOverlay: new ol.FeatureOverlay({
                      style: new ol.style.Style({
                        image: new ol.style.Circle({
                          radius: 5,
                          fill: new ol.style.Fill({
                            color: '#FF0000'
                          }),
                          stroke: new ol.style.Stroke({
                            color: '#000000'
                          })
                        })
                      })
                    })
                  })
                ]),
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
                    source: new ol.source.GeoJSON({
                      url: 'data/layers/7day-M2.5.json'
                    }),
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
    
    Klicken Sie mit der linken Maustaste auf ein Feature um es zu selektieren.

    .. figure:: select1.png
   
       Featureselektion mittels einer ``Select``-Interaktion.