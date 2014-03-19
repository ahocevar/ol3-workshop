.. _openlayers.vector.basics:

Mit Vektorlayern arbeiten
=========================

Mit ``ol.layer.Vector`` steht in ol3 ein durchaus recht komplexer Layertyp zur
Verfügung. Standardmäßig werden beim Verwenden dieses Layers von ol3 keine
Annahmen dazu gemacht, woher dieser Layer seine Daten bezieht, hierum kümmert
sich die Klasse ``ol.source.Vector``. Wie man Vektorlayer ausgestaltet,
beschreibt :ref:`einer der nächsten Abschnitte <openlayers.vector.style-intro>`.
Hier werden wir uns eine zunächst einge Grundlagen zu Vektordaten erarbeiten.


.. _openlayers.vector.basics.format:

``ol.format``
-------------

In ol3 kümmern sich die ``ol.format``-Klassen um das Parsen der von einem Server
gelieferten Daten, die Features enthalten. Meist werden Sie jedoch nicht die
``ol.format``-Klassen **direkt** verwenden, sondern eher spezialisierte 
``source``-Klassen (wie etwa ``ol.source.KML``). Formate transformieren rohe
Daten in echte ``ol.Feature``-Objekte.

Vergleichen Sie bitte die beiden Datenblöcke weiter unten. Beide repräsentieren
das gleiche ``ol.Feature``-Objekt (Es handelt sich um einen Punkt in Barcelona).
Der erste Block stellt dieses Feature mittels GeoJSON dar (vgl. 
``ol.format.GeoJSON``). Der zweite Block dagegen enthält
:abbr:`KML (OGC Keyhole Markup Language)` (vgl. ``ol.format.KML``)


GeoJSON-Beispiel
````````````````

.. code-block:: json

    {
        "type": "Feature",
        "id": "OpenLayers.Feature.Vector_107",
        "properties": {},
        "geometry": {
            "type": "Point",
            "coordinates": [-104.98, 39.76] 
        }
    }


KML-Beispiel
````````````

.. code-block:: xml

    <?xml version="1.0" encoding="utf-8"?>
    <kml xmlns="http://earth.google.com/kml/2.2">
      <Placemark>
        <Point>
          <coordinates>-104.98,39.76</coordinates>
        </Point>
      </Placemark>
    </kml>
