.. _config.dissect:

Die Bestandteile der Karte
==========================

Wie in dem :ref:`vorherigen Abschnitt <config.map>` erwähnt wurde, besteht
unsere HTML-Karte aus :ref:`HTML-Markup (Auszeichnung) <config.dissect.markup>`,
:ref:`CSS-Deklarationen (Stil) <config.dissect.style>` und
:ref:`JavaScript-Initialisierungs-Code (Verhalten) <config.dissect.code>`. Wir
werden uns jeden dieser Teile nun ein wenig genauer betrachten.


.. _config.dissect.markup:

HTML-Markup (Auszeichnung)
--------------------------

Im HTML ist das Markup für die Karte recht überschaubar; wir brauchen im Grunde
nur ein Elelemt:

.. code-block:: html

    <div id="map"></div>

Dieses ``<div>``-Element wird wie ein Container für den Karten-Viewport
fungieren. Wir verwenden ein ``<div>``-Element, aber es kann auch jedes andere
`block-level <https://developer.mozilla.org/en-US/docs/HTML/
Block-level_elements>`_-Element
verwendet werden. 

Wir vergeben ein ``id``-Attribut, dadurch können wir es als ``target`` unserer
Karte verwenden.


.. _config.dissect.style:

CSS-Deklarationen (Stil)
------------------------

Zu ol3 gehört auch ein default-Stylesheet, welches festlegt, wie die einzelnen
Kartenelemente gestylt werden sollen. Wir haben in unserem HTML dieses
Stylesheet mittels eines ``<link>``-Elements eingebunden: 

.. code-block:: html

    <link rel="stylesheet" href="ol3/ol.css" type="text/css">


ol3 macht keine Annahmen zur Größe der Karte (bzw. des Elementes welches die
Karte enthalten wird). Genau aus diesem Grunde fügen wir exakt eine weitere
eigene Style-Deklaration hinzu, die die Dimensionen des Karten-``<div>``\ s
bestimmt:

.. code-block:: html

    <link rel="stylesheet" href="ol3/ol.css" type="text/css">
    <style>
      #map {
        height: 256px;
        width: 512px;
      }
    </style>

Wir verwenden als CSS-Selektor ``#map`` (dies war ja die id des Ziel-``<div>``)
und setzen die Breite (``512px``) und Höhe (``256px``) des Containers.

Diese Regel ist direkt im ``<head>``-Element innerhalb eines 
``<style>``-Elementes angegeben. Oft werden Sie später solche Angaben jedoch
auch in einem seperaten Stylesheet notieren.


.. _config.dissect.code:

JavaScript-Initialisierungs-Code (Verhalten)
--------------------------------------------

Der nächste Schritt zur Karte besteht aus JavaScript-Code zum Initialisieren
der Karte. Wir haben hier den gesamten Code innerhalb eines
``<script>``-Elements ganz am Ende des HTML ``body``-Tag eingefügt:


.. code-block:: html

    <script>
      var map = new ol.Map({
        target: 'map',
        renderer: 'canvas',
        layers: [
          new ol.layer.Tile({
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

.. note::

    Die Reihenfolge dieser Schritte ist sehr wichtig. Bevor unser
    benutzerdefiniertes Skript ausgeführt werden kann, muss die ol3-Bibliothek
    geladen werden. In unserem Beispiel wird die ol3 im ``<head>`` unseres
    Dokuments mit ``<script src="ol3/ol.js"></script>`` geladen.
    
    Analog hierzu kann auch der oben aufgeführte Karten-Initialisierungs-Code
    erst dann erfolgreich ausgeführt werden, wenn das Element, welches später
    die Karte enthalten wird (``<div id="map"></div>``) bereits "bereit"
    (sprich gerendert und Teil des Dokuments) ist. Dadurch, dass wir den
    ``<script>``-Tag erst direkt vor dem schließenden ``</body>`` notieren,
    stellen wir sicher, dass der Karten-Container zur Verfügung steht.

Schauen wir uns die weniggen Zeilen JavaScript genauer an, um zu verstehen, wie
wir die Karte erzeugen. Unser Script erzeugt eine Instanz der Klasse ``ol.Map``
mit einigen Konfigurations-Optionen:


``ol.Map``-Konfigurationsoption ``target``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: javascript

    target: 'map'

Wir verwenden den Wert des ``id``-Attributs unseres Karten-Containers um dem
Map-Konstruktor mitzuteilen, in welches Element wir die Karte gerendert haben
möchten. Wir verwenden also dewn String-Wert ``"map"``. Diese Syntax ist
eine bequeme Kurzform. Statt eines Strings kann auch eine Referenz zu dem
Element angegeben werden (Solch eine Referenz kannn man etwa über 
``document.getElementById("map")`` erhalten). 


``ol.Map``-Konfigurationsoption ``renderer``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Die Option ``renderer`` legt fest, welcher `Renderer` von ol3 verwendet werden
soll. Derzeit gibt es drei verschiedene renderer-Typen:

* den DOM-Renderer,
* den Canvas-Renderer und
* den WebGL-Renderer.

Wir verwenden den Canvas-Renderer. Da die Bilder des WMS von einer anderen
Domain (also nicht ``localhost``) kommen, können wir den WebGL-Renderer nicht
verwenden; dies verbietet die `Same-Origin-Policy <https://developer.mozilla.org
/en-US/docs/Web/JavaScript/Same_origin_policy_for_JavaScript>`_. 


.. code-block:: javascript

    renderer: 'canvas'



``ol.Map``-Konfigurationsoption ``layers``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Die ``layers`` Konfiguration erwartet eine Liste (alls JavaScript-Array) aller
Kartenthemen, die wir auf der Karte dargestellt haben wollen.

.. code-block:: javascript

    layers: [
      new ol.layer.Tile({
        source: new ol.source.TileWMS({
          url: 'http://maps.opengeo.org/geowebcache/service/wms',
          params: {LAYERS: 'bluemarble', VERSION: '1.1.1'}
        })
      })
    ],

Auf die Syntax zur Erzeugung von Layern werden wir in den folgenden Kapiteln
noch näher eingehen. Lassen Sie sich also nicht von der konkreten Syntax
erschrecken.

Um eine Karte sehen zu können, benötigen wir wenigstens einen Layer.


``ol.Map``-Konfigurationsoption ``view``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Als letztes wollen wir den ``view`` (etwa `Kartenausschnitt`) konfigurieren. Wir
spezifizieren eine Projektion (``projection``), ein Kartenzentrum (``center``)
und den initialen Zoomlevel (``zoom``). Außerdem legen wir eine
``maxResolution`` fest, so dass wir keine Kartenausschnitte anfordern, die von
dem entfernten Dienst (in disem Falle ein GeoWebCache) nicht geliefert werden
können.


.. code-block:: javascript

    view: new ol.View2D({
       projection: 'EPSG:4326',
       center: [0, 0],
       zoom: 0,
       maxResolution: 0.703125
    })


herzlichen Glückwuunsch: Sie haben soebnen erfolgreich Ihre erste
Kartenanwendung mit ol3 seziert und analysiert!

Um erfolgreich weiterarbeiten zu könne, schaen wir uns
:ref:`weitere Ressourcen <config.resources>` an, die Sie als ol3-Entwickler
benötigen werden.

