.. _config.resources:

Weitere ol3 Ressourcen
======================

ol3 stellt bereits ein große Menge an Funktionalität zur Verfügung. Obwohl die 
ol3-Entwickler hart gearbeitet haben, um Beispiele für viele der 
Funktionalitäten zu entwickeln, und der Code so auch vor dem Hintergrund 
einfacher Nachvollziehbarkeit organisiert und geschrieben wurde, haben
viele Entwickler zunächst Schwierigkeiten auf ol3 umzusteigen.

Hier werden zukünftige Versionen von ol3 sicherlich ansetzen müssen, um noch
bessere Handreichungen zu geben.


Lernen am Beispiel
------------------

Die meisten neuen Nutzer von ol3 lernen vermutlich am einfachsten die Verwendung
der Bibliothek durch das Nachvollziehen von Beispielen. Hier kann man meist
kleine spezifische Aspekte in Aktion betrachten und mit dem Code *spielen*, um
ihn zu verstehen. 

 * http://ol3js.org/en/master/examples/


Thematische Dokumentation
-------------------------

Zu speziellen Themen gibt es bereits Prosa-Dokumentation. Die Anzahl an
behandelten Themen wird sicherlich noch wachsen.

 * http://ol3js.org/en/master/doc/quickstart.html
 * http://ol3js.org/en/master/doc/tutorials
 

API-Dokumentation
-----------------

Nachdem die Basiskomponenten, die eine ``ol.Map`` beeinflussen und ausmachen
verstanden sind, hilft ein Blick in die aus dem Quelltext generierte
API-Dokumentation. Hier finden sich Details zu Klassen, Methoden-Signaturen oder
Objekteigenschaften etc.

 * http://ol3js.org/en/master/apidoc/
 
Die API-Dokumentation ist noch im Aufbau und nicht zu 100% vollständig. Auch
hieran arbeiten die ol3-Entwickler fieberhaft.


Teil der Community werden
-------------------------

ol3 wird entwickelt und gewartet von einer Gemeinschaft aus Programmierern und
Benutzern wie Ihnen! Unabhängig davon, ob Sie Fragen stellen oder Code
beitragen wollen, die einfachste und erste Form direkter Partizipation ist
sicherlich die Registrierung auf der (englischsprachigen) Mailingliste

 * https://groups.google.com/forum/#!forum/ol3-dev

Hier ist jeder neue Nutzer willkommen und keine Frage zu grundlegend!

Fehler melden
-------------

Wir freuen uns über Feedback auch insbesondere, wenn Sie glauben einen Bug
gefunden zu haben.

Um Fehler zu melden, ist es wichtig, dass Sie die verschiedenen *Varianten*
von ol3 kennen: 

 * ``ol.js`` - Diese Datei wird durch den *Google Closure Compiler* (im
   *Advanced Mode*) erstellt und ist für Menschen i.d.R. nicht lesbar.
 * ``ol-simple.js`` - Diese Datei wird ebenfalls durch den 
   *Google Closure Compiler* (jedoch im *Simple Mode*) erstellt und ist bereits
   eher les- und verstehbar (vgl. auch 
   https://developers.google.com/closure/compiler/faq)
 * ``ol-whitespace.js`` - Menschenlesbare Version von ol3, die zum Debuggen &
   Fehler melden verwendet werden sollte.

Wenn Sie einen Fehler feststellen, ist es wichtig, dass die Meldung des Fehlers
auf Basis der ``ol-whitespace.js``-Datei geschieht. Wenn möglich, sollte auch
der zum Fehler führende *stack trace* Teil der Fehlermeldung sein. Ein solcher
*stack trace* lässt sich etwa mittels verschiedener Browserwerkzeuge (etwa
*Chrome developer Tools*) generieren.

Um dies zu testen wollen wir in ``map.html`` einen Fehler produzieren. Ändern
Sie hierzu den Layertyp von ``ol.layer.Tile`` zu ``ol.layer.Image``. Auf der
Konsole müssten Sie die folgende Fehlermeldung erhalten:

``Uncaught TypeError: Object #<yc> has no method 'tb'``

Niemand kann Ihnen hierzu bei Fragen weitere Informationen oder Hilfe geben.

Wenn statt ``ol.js`` jedoch ``ol-whitespace.js`` eingebunden wird, so hält der
JavaScript-Debugger des Browser nach einem Neuladen der Seite an der kritischen
Stelle die Ausführung des Codes an, und ein Debugging ist deutlich vereinfacht.


.. figure:: debugger.png

    Der *stack trace* im Debugger. Mittels der rechten Maustaste kann jener
    kopiert werden.

