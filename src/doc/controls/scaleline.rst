.. _openlayers.controls.scaleline:

Einen Maßstabsbalken einblenden
===============================

Auf Karten werden nicht selten Maßstabsbalken zur besseren Orientierung
angezeigt. Mit ol3 ist dies auch möglich: die Klasse ``ol.control.ScaleLine``
kann hierzu verwendet werden.

Erzeugung des Maßstabsbalkens
-----------------------------

.. rubric:: Übungen

#.  Öffnen Sie ``map.html`` im Texteditor.

#.  Fügen Sie nachfolgenden Code irgendwo innerhalb der ``ol.Map``-Konfiguration
    hinzu:

    .. code-block:: javascript

        controls: ol.control.defaults().extend([
          new ol.control.ScaleLine()
        ]),
    
#.  Speichern Sie Ihre Änderungen und laden Sie die Seite im Browser
    neu: @workshop_url@/map.html
    
    .. figure:: scaleline1.png
    
    Der Maßstabsbalken ohne Anpassungen.
    


Anpassen der ``ScaleLine``-Control
----------------------------------

Vermutlich finden auch Sie, dass der Maßstabsbalken schlecht zu lesen ist, wenn
er auf der Weltkarte angezeigt wird. Man kann verschiedenen Dinge unternehmen,
um die Lesbarkeit zu verbessern. Vielleicht reicht es, wenn  wir den CSS-Stil
anpassen? Wir wollen zunächst eine andere Hintergrundfarbe festlegen und auch
etwas mehr Innenabstand zuweisen:

    .. code-block:: html

        .ol-scale-line,
        .ol-scale-line:not([ie8andbelow]) {
          background: black;
          padding: 5px;
        }


Aus didaktischen Gründen wollen wir aber nun annehmen, dass Ihr Karten-``div``
sowieso schon überfüllt ist, und Sie daher entschieden haben, dass der
Maßstabsbalken in ein separates ``<div>``-Element außerhalb der Karte plaziert
werden sollte.

Hierzu werden wir zunächst ein solches Element dem HTML hinzufügen, und
anschließend der ``ol.control.ScaleLine`` mitteilen, **wo** sie gerendert werden
soll.

.. rubric:: Übungen

#.  Erzeugen sie ein neues ``<div>``-Element auf Ihrer HTML-Seite. Um den
    Zugriff auf das Element zu vereinfachen, geben Sie dem ``<div>`` bitte die
    ``id="scale-line"``. Fügen Sie z.B. den nachfolgenden Code ein:

    .. code-block:: html
    
        <div id="scale-line" class="scale-line"></div>

#.  Anschließend müssen wir den Code modifizieren, der die ``ScaleLine`
    erzeugt. Beachten Sie vor allem den Schlüssel ``target``:
    
    .. code-block:: javascript
   
        controls: ol.control.defaults().extend([
          new ol.control.ScaleLine({
            className: 'ol-scale-line', 
            target: document.getElementById('scale-line')
          })
        ]),

#.  Speichern Sie Ihre Änderungen und laden Sie die Seite im Browser
    neu: @workshop_url@/map.html
    
#.  Was ändern die folgenden Zeilen CSS? 
    
    .. code-block:: html
    
        .scale-line {
          position: absolute;
          top: 350px;
        }
        .ol-scale-line { 
          position: relative;
          bottom: 0px;
          left: 0px;
        }

#.  Speichern Sie Ihre Änderungen erneut und laden Sie die Seite im Browser
    neu: @workshop_url@/map.html

    .. figure:: scaleline2.png
   
       Ein Maßstabsbalken in separatem ``<div>``-Element.

.. note::

    Um eigene `controls` zu erzeugen sei auf
    http://ol3js.org/en/master/examples/custom-controls.html verwiesen.


