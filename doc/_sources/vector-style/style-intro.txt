.. _openlayers.vector.style-intro:

Styling Grundlagen
==================

Wenn man HTML-Elemente ausgestaltet, kann man zum Beispiel ein CSS wie das
folgende verwenden:

.. code-block:: css

    .someClass {
        background-color: blue;
        border-width: 1px;
        border-color: olive;
    }

``.someClass`` ist ein Selektor (in diesem Fall einer, der jedwedes Element mit
der Klasse ``"someClass"`` selektieren würde) und der mit geschwungen Klammern
umschlossene Block stellt eine Gruppe von Schlüssel-Wert-Paaren dar: die 
eigentlichen Stildeklarationen.


``ol.feature.StyleFunction``
----------------------------

Ein Vektorlayer akzeptiert als Wert für die ``style``-Konfigurationsoption eine
Funktion, in welcher ein unterschiedlicher Stil anhand eines Featureattributes
zurückgegeben werden kann. Der Funktion werden zwei Argumente übergeben: Das zu
stylende ``feature`` und die aktuelle ``resolution``. Nehmen wir an,
Sie wollten alle Features, die ein Attribut ``class`` mit Wert ``someClass``
besitzen, speziell stylen, so könnte Ihre Stylingfunktion wie folgt aussehen:


.. code-block:: javascript

    style: function(feature, resolution) {
      if (feature.get('class') === 'someClass') {
        // return the style
      }
    }

Stildeklarationsböcke: Symbolizer
---------------------------------

Das Äquivalent zu einem Block von Stildeklarationen aus CSS sind in ol3 
sogenannte *symbolizer*. Üblicherweise handelt es sich hierbei um Instanzen der
Klassen im Namensraum ``ol.style``. Um Polygon-Features etwa mit blauem
Hintergrund und einem 1-Pixel breiten Rahmen in olivgrün zu zeichnen, würde man
zwei *symbolizer* verwenden können:


.. code-block:: javascript

    new ol.style.Style({
      fill: new ol.style.Fill({
        color: 'blue'
      }),
      stroke: new ol.style.Stroke({
        color: 'olive',
        width: 1
      })
    });


Je nach Geometrietyp können verschiedene *symbolizer* verwendet werden. Linien
verhalten sich weitestgehend wie Polygone, akzeptieren jedoch keine Füllung.
Punkte können derzeit mittels ``ol.style.Circle`` (für Kreissymbole) oder
``ol.style.Icon`` (z.B. für png-Bilder) ausgestaltet werden. Schauen wir uns ein
Beispiel mit Kreissymbolen an:

.. code-block:: javascript

    new ol.style.Circle({
      radius: 20,
      fill: new ol.style.Fill({
        color: '#ff9900',
        opacity: 0.6
      }),
      stroke: new ol.style.Stroke({
        color: '#ffcc00',
        opacity: 0.4
      })
    });

``ol.style.Style``
------------------

Jedes ``ol.style.Style``-Objekt hat vier Schlüssel: ``fill``, ``image``,
``stroke`` und ``text``. Optional existiert eine ``zIndex``-Eigenschaft. Die
Stylefunktion gibt ein Array von ``ol.style.Style``-Objekten zurück.

Angenommen alle Features sollten rot gezeichnet werden, außer denjenigen, die
ein ``class``-Attribut mit dem Wert ``"someClass"`` haben (und diese sollen
wie oben blau gefüllt sein und einen 1-Pixel breiten Rand in oliv haben), so
könnte die Funktion wie folgt aussehen. 


.. code-block:: javascript

    style: (function() {
      var someStyle = [new ol.style.Style({
        fill: new ol.style.Fill({
          color: 'blue'
        }),
        stroke: new ol.style.Stroke({
          color: 'olive',
          width: 1
        })
      })];
      var otherStyle = [new ol.style.Style({
        fill: new ol.style.Fill({
          color: 'red'
        })
      })];
      return function(feature, resolution) {
        if (feature.get('class') === "someClass") {
          return someStyle;
        } else {
          return otherStyle;
        }
      };
    }())


Wenn möglich, sollten die tatsächlichen Stil-Objekte außerhalb der
Funktion möglichst nur einmal erzeugt werden, und in der Funktion nur
Referenzen hierauf zurückgegeben werden (Bessere Perfomance). Im obigen Beispiel
wird hierzu eine `closure` verwendet.


.. note ::

    Auch Features akzeptieren in ihrer ``style``-Konfigurationsoption eine
    Funktion. Jene wird mit der aktuellen ``resolution`` aufgerufen und erlaubt
    ein sehr indivuelles Stylen je Feature und Maßstab.


Pseudoklassen
-------------

CSS kennt das Konzept sogenannter Pseudoklassen, die die Anwendung von
Stildeklaration weiter einschränkt um spezielle Kontexte abzubilden, die nicht
oder nur schwerlich über *klassische* Selektoren abgebildet werden können (z.B.
`:hover` oder `:active`).

In ol3 ist ein in mancher Hinsicht ähnliches Konzept mittels der
``style``-Option von ``ol.FeatureOverlay`` umgesetzt. Um selektierte Features
einer ``ol.interaction.Select`` zu visualisieren, wird ein solcher
*feature overlay* verwendet, dessen ``style`` das Aussehen bestimmt.

Ein Beispiel wäre etwa:

.. code-block:: javascript

    var select = new ol.interaction.Select({
      featureOverlay: new ol.FeatureOverlay({
        style: new ol.style.Style({
          fill: new ol.style.Fill({
            color: 'rgba(255,255,255,0.5)'
          })
        })
      })
    });

Nachdem wir die Basiskonzepte nun kennen, können wir uns nun der konkreten
:ref:`Ausgestaltung von Vektorlayern <openlayers.style>` zuwenden.

