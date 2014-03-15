.. _openlayers.layers.cached:

Vorberechnete Kacheln (`tiles`)
===============================

Standardmäßig fragt der ``Tile``-Layer Bilder in 256 x 256 Pixel Größe an, um
den Kartenviewport (und einen kleinen Bereich darüber hinaus) zu füllen. Während
die Karte verschoben oder gezoomt wird, gehen weitere Anfragen in eben dieser
Größe an den Server, um jeweils den aktuellen Bereich auszufüllen. Einige Bilder
wird der Browser lokal vorhalten (*cachen*) trotzdem ist der
Prozessierungsaufwand auf dem Server unter Umständen hoch, da die Bilder 
dynamisch berechnet werden.

Da die letztlich generierten Anfragen in einem regulären Grid deterministisch
sind, kann ein Server die Anfragen ggf. vorberechnen und bei einer Anfrage nur
ein hoffentlich bereits bestehendes Bild ausliefern. Üblicherweise führt dies zu
einer Performancesteigerung sowohl auf der Client- als auch auf der Serverseite.


.. _openlayers.layers.cached.xyz:

Die Klasse ``ol.source.XYZ``
----------------------------

Die *Web Map Service*-Spezifikation räumt anfragenden Clients eine große
Freiheit ein, die letztlich in unendlich vielen verschiedenen Requests münden.
Ohne weitere Einschränkungen ist es in der Praxis daher sehr schwierig (wenn
nicht unmöglich), diese Anfragen zu *cachen*.

Das gegenteilige Extrem stellt ein Service dar, der ausschließlich eine fixe
Anzahl an Zoomstufen unterstützt, und in jenen auch nur solche, die in ein
gewisses Grid passen. Solche Dienste lassen sich in einer XYZ-Quelle
generalisiseren; `X` und `Y` repräsentieren dann Spalten und Zeilen in jenem
Grid und `Z` steht für die Zoomstufe.


.. _openlayers.layers.cached.osm:

Die Klasse ``ol.source.OSM``
----------------------------

Das `OpenStreetMap (OSM) <http://www.openstreetmap.org/>`_\ -Projekt stellt
vielfaltige & weltweite Geodaten frei zur Verfügung, die von Freiwilligen
gesammelt und erhoben wurden. OSM stellt verschiedene gerenderte Kachel-Sets
dieser Daten zur Verfügung. Da jene Kacheln analog zum
:ref:`XYZ-Grid <openlayers.layers.cached.xyz>` organisiert sind, kann man sie
in ol3 nutzen. Die Klasse ``ol.source.OSM`` greift auf diese *tiles* zu.


.. _openlayers.layers.cached.example:

.. rubric:: Übungen

#.  Öffen Sie die Datei ``map.html`` vom
    :ref:`vorherigen Abschnitt <openlayers.layers.wms>` in einem Texteditor und
    ändern Sie die Karten-Initialisierung wie folgt:
    
    .. code-block:: html

        <script>
          var map = new ol.Map({
            target: 'map',
            renderer: 'canvas',
            layers: [
              new ol.layer.Tile({
                source: new ol.source.OSM()
              })
            ],
            view: new ol.View2D({
              center: ol.proj.transform([-93.27, 44.98], 'EPSG:4326', 'EPSG:3857'),
              zoom: 9
            })
          });
        </script>

#.  Im ``<head>`` Bereich des Dokumentes fügen Sie bitte einige
    CSS-Deklarationen für die Copyright-Angaben (*attribution*) von ol3 hinzu.
    
    .. code-block:: html
    
        <style>
            #map {
                width: 512px;
                height: 256px;
            }
            .ol-attribution ul,
            .ol-attribution a,
            .ol-attribution a:not([ie8andbelow]) {
                color: black !important;
            }
        </style>

#.  Speichern Sie Ihre Änderungen und laden Sie die Seite im Browser
    neu: @workshop_url@/map.html

.. figure:: cached1.png

    Eine Karte, die einen gekachelten Layer aus der OpenStreetMap Quelle
    darstellt.


Einige Details
~~~~~~~~~~~~~~

Projektionen
````````````

Schauen wir uns die ``view``-Definition genauer an:

.. code-block:: javascript

    view: new ol.View2D({
      center: ol.proj.transform([-93.27, 44.98], 'EPSG:4326', 'EPSG:3857'),
      zoom: 9
    })

Räumliche Daten können in verschiedenen räumlichen Koordinatensystemen
vorliegen. Während ein Datensatz zum Beispiel geographische Koordinaten
(Längen- und Breitengrade) verwendet, kann ein anderer Datensatz in lokaler
metrischer Projektion vorliegen. Eine vollständige Diskussion von
Koordinatenbezugssystemen liegt sicherlich außerhalb des Fokus dieses Workshops,
aber es ist wichtig, das wesentliche Konzept zu verstehen.

ol3 muss das Koordinatensystem Ihrer Daten kennen. Intern werden Projektionen
von der Klasse ``ol.proj.Projection`` abgebildet. Die ``transform`` Funktion 
im ``ol.proj`` Namensraum akzeptiert Auch *Strings* die das Koordinatensystem
repräsentieren (oben sind dies ``"EPSG:4326"`` und ``"EPSG:3857"``). 


Koordinatentransformierung
``````````````````````````

Die OpenStreetMap Kacheln, die wir verwenden, liegen in einer Merkator 
Projektion vor. Wir müssen daher auch das Kartenzentrum in Merkator-Koordinaten
angeben. Da es relativ einfach ist, geographischen Koordinaten zu einem gewissen 
Ort auf der Welt zu bekommen, nutzen wir ``ol.proj.transform`` um geographische
Koordinaten (``"EPSG:4326"``) in Merkator Koordinaten (``"EPSG:3857"``)
umzuwandeln.


Angepasste Karten-Optionen
``````````````````````````

.. note::

    Die Projektionen, die wir bislang verwendet haben, sind die einzigen, die
    ol3 per default kennt. Für andere Projektionen müssen wir etwas mehr Aufwand
    betreiben.

.. code-block:: javascript

    var projection = ol.proj.configureProj4jsProjection({
      code: 'EPSG:21781',
      extent: [485869.5728, 76443.1884, 837076.5648, 299941.7864]
    });

Des weiteren benötigen wir zwei zusätzliche ``<script>``-Tags:

.. code-block:: html

    <script type="text/javascript"
      src="http://cdnjs.cloudflare.com/ajax/libs/proj4js/1.1.0/proj4js-compressed.js">
    </script>
    <script type="text/javascript"
      src="http://cdnjs.cloudflare.com/ajax/libs/proj4js/1.1.0/defs/EPSG21781.js">
    </script>

Unter http://spatialreference.org/ oder http://epsg.io/ kann man die relevanten
Informationen nachschauen, sofern der EPSG-Code bekannt ist.


Erzeugung des Layers
````````````````````

.. code-block:: javascript

    layers: [
      new ol.layer.Tile({
        source: new ol.source.OSM()
      })
    ],

Wie zuvor erzeugen wir einen Layer und fügen ihn der Karten-Konfiguration hinzu.
Der Konstruktor ``ol.source.OSM()`` wird ohne Argument aufgerufen, daher gelten
für diese Instanz die ol3-Defaults.


Stil
````

.. code-block:: html

    .ol-attribution ul,
    .ol-attribution a,
    .ol-attribution a:not([ie8andbelow]) {
      color: black;
    }


Wie ol3-Controls zu handhaben sind, steht nicht im Fokus dieses Abschnitts. Als
kleine Vorschau sei jedoch hier bereits erwähnt, dass standardmäßig jede
``ol.Map`` eine ``ol.control.Attribution`` Control hat, die etwa
Copyright-Informationen zu den Kartenthemen darstellt.

Obige CSS-Angaben ändern das Aussehen dieser Angaben, welche auf der Karte im
unteren Bereich sichtbar sind.

Nachdem wir erfolgreich öffentlich verfügbare gekachelte *TileSets* verwendet
haben, schauen wir uns an, wie wir
:ref:`proprietäre Rasterthemen <openlayers.layers.proprietary>` einsetzen
können.
