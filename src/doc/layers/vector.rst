.. _openlayers.layers.vector:

Vektorlayer
===========

Vektorlayer werden durch die Klasse ``ol.layer.Vector`` bereitsgestellt. Dieser
Layertypus kümmert sich um die Darstellung von Vektordaten auf der Client-Seite.

Derzeit unterstützt in ol3 ausschließlich der `Canvas`-Renderer die Verwendung
von ``ol.layer.Vector``.


Vektorfeatures gerendert im Browser
-----------------------------------

Wir kehren zunächst mit unsere karte zurück auf das Erste Beispiel mit einer
Ansicht der gesamten Welt. Hierauf wollen wir anschließend Vektoren darstellen.

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

.. rubric:: Übungen

#.  Öffnen Sie die Datei ``map.html`` in einem Texteditor und fügen Sie obigen
    Code ein.

#.  Speichern Sie ihre Änderungen in der Datei ``map.html`` und testen Sie das
    Ergebnis im Browser: @workshop_url@/map.html

#.  Im Initialisierungscode der Karte fügen Sie bitte nach dem ``Tile``-Layer
    einen weiteren Layer hinzu. Kopieren Sie am einfachsten hierzu die folgenden
    Zeilen, die einen Vektorlayer definieren, der seine Daten aus einer lokalen
    GeoJSON-datei bezieht:

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
   
    Eine Weltkarte mit weißen Punkten, die Erdbeben repräsentieren.

.. note::

    Da die Daten in der GeoJSON-Datei in ``EPSG:4326`` vorliegen, und der
    ``view`` der Karte auch diese Projektion verwendet, müssen wir die Daten
    nicht reprojizieren. Falls die Datenprojektion nicht übereinstimmt, kann
    man bei der Konstruktion der ``source`` einen ``projection``-Key im
    Konfigurationsobjekt übergeben.


Details des Beispiels
`````````````````````

Wir schauen uns nun die Erzeugung des Vektorlayer im Detail an um zu verstehen,
was im Beispiel passiert.

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

Der Layer wird mit dem Titel (``title``) `Earthquakes` und einigen weiteren
Optionen initialisisert. Insbesondere von Relevanz ist der ``source``-Schlüssel,
welchem wir eine Instanz von ``ol.source.GeoJSON`` zuweisen. Diese ``source``
verweist auf die URL zur GeoJSON-Datei.

.. note::

    Wollten wir die Features des Layers attributiv ausgestalten, so könnten wir
    statt einer Instanz von ``ol.style.Style`` auch eine Stylefunktion
    übergeben. 


.. rubric:: Zusatzaufgaben

#.  Jeder weiße Kreis repräsentiert ein ``ol.Feature`` Objekt des
    ``ol.layer.Vector``, und jedes Feature hat die Attribute ``summary`` &
    ``title``. Registrieren Sie eine Funktion, die bei jedem
    ``singleclick``-Event, welcher auf der ``ol.Map`` gefeuert wird, die
    Funktion ``forEachFeatureAtPixel`` der ``map`` aufruft und gegebenenfalls
    weitere Erdbebeninformationen ausgibt.

#.  Die hier verwendeten Daten entstammen der Seite 
    http://earthquake.usgs.gov/earthquakes/catalogs/ des USGS. Recherchieren Sie
    dort, ob Sie weitere geodaten in einem von ol3 unterstützten Format finden
    und versuchen Sie, jene in ``map.html`` zu integrieren.

