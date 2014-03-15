.. _openlayers.style:

Vektorlayer ausgestalten
========================


.. rubric:: Tasks

#.  Wir beginnen mit einem lauffähigen Beispiel welches Gebäudegrundrisse in
    einem Vektorlayer darstellt. Öffnen Sie die Datei ``map.html`` im Texteditor
    und fügen Sie den nachfolgenden Code ein:

    .. code-block:: html

        <!doctype html>
        <html lang="en">
          <head>
            <link rel="stylesheet" href="ol3/ol.css" type="text/css">
            <style>
            #map {
              background-color: gray;
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
                  new ol.layer.Vector({
                    title: 'Buildings',
                    source: new ol.source.KML({
                      url: 'data/layers/buildings.kml'
                    }),
                    style: new ol.style.Style({
                      stroke: new ol.style.Stroke({color: 'red', width: 2})
                    })
                  })
                ],
                view: new ol.View2D({
                  projection: 'EPSG:4326',
                  center: [-122.791859392, 42.3099154789],
                  zoom: 16
                })
              });
            </script>
          </body>
        </html>

#.  Speichern Sie Ihre Änderungen und laden Sie die Seite im Browser
    neu: @workshop_url@/map.html
    
    Sie sollten rot umrandete Gebäudegrundrisse sehen.

#.  Nachdem uns :ref:`Styling in ol3 <openlayers.vector.style-intro>`, nicht
    mehr fremd ist, können wir eine Stylefunktion erzeugen, die je nach der
    Größe des Grundrisses die Features einfärbt. Ersetzen Sie die
    ``style``-Konfigurationsoption des Layers mit folgenden Zeilen:
    
    .. code-block:: javascript

            style: (function() {
              var defaultStyle = [new ol.style.Style({
                fill: new ol.style.Fill({color: 'navy'}),
                stroke: ol.style.Stroke({color: 'black', width: 1})
              })];
              var ruleStyle = [new ol.style.Style({
                fill: new ol.style.Fill({color: 'olive'}),
                stroke: new ol.style.Stroke({color: 'black', width: 1})
              })];
              return function(feature, resolution) {
                if (feature.get('shape_area') < 3000) {
                  return ruleStyle;
                } else {
                  return defaultStyle;
                }
              };
            })()

#.  Speichern Sie Ihre Änderungen und laden Sie die Seite im Browser
    neu: @workshop_url@/map.html

    .. figure:: style1.png

       Die Grundrisse werden nun nach Grundfläche ausgestaltet.


#.  Schließlich wollen wir unsere Features nun noch mit einem textlichen *Label*
    versehen:

    .. code-block:: javascript

            style: (function() {
              var stroke = new ol.style.Stroke({
                color: 'black'
              });
              var textStroke = new ol.style.Stroke({
                color: '#fff',
                width: 3
              });
              var textFill = new ol.style.Fill({
                color: '#000'
              });
              return function(feature, resolution) {
                return [new ol.style.Style({
                  stroke: stroke,
                  text: new ol.style.Text({
                    font: '12px Calibri,sans-serif',
                    text: feature.get('key'),
                    fill: textFill,
                    stroke: textStroke
                  })
                })];
              };
            })()

#.  Speichern Sie Ihre Änderungen und laden Sie die Seite im Browser
    neu: @workshop_url@/map.html

    .. figure:: style2.png

       Die Gebäude werden nun auch mit einem *Label* gerendert.
