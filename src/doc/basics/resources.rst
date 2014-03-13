.. _config.resources:

ol3 Ressourcen
==============

ol3 stellt bereits ein große Menge an Funktionalität zur Verfügung. Obwohl die 
ol3-Entwickler hart gearbeitet haben, um Beispiele für viele der 
Funktionalitäten zu entwickeln, und der Code so auch vor dem Hintergrund 
einfacher Nachvollziehbarkeit organisiert und geschrieben wurde, haben
viele Entwickler zunächst Schwierigkeiten auf ol3 umzusteigen.

Hier werden zukünftige Versionen von ol3 sicherlich ansetzen müssen, um noch
bessere Handreichungen zu geben.


Learn by Example
----------------

New users will most likely find diving into the OpenLayer's example code and experimenting with the library's possible functionality the most useful way to begin.

 * http://ol3js.org/en/master/examples/


Browse the Documentation
------------------------

For further information on specific topics, browse the growing collection of OpenLayers  documentation.

 * http://ol3js.org/en/master/doc/quickstart.html
 * http://ol3js.org/en/master/doc/tutorials
 

Find the API Reference
----------------------

After understanding the basic components that make-up and control a map, search the API reference documentation for details on method signatures and object properties.

 * http://ol3js.org/en/master/apidoc/


Join the Community
------------------

OpenLayers is supported and maintained by a community of developers and users like you. Whether you have questions to ask or code to contribute, you can get involved by signing up for the mailing list.

 * https://groups.google.com/forum/#!forum/ol3-dev

Reporting issues
----------------
For reporting issues it is important to understand the several flavours in which the OpenLayers library is distributed:

 * ``ol.js`` - the script which is built using the Google Closure Compiler in advanced mode (not human readable)
 * ``ol-simple.js`` - the script which is built using the Google Closure compiler in simple mode (for more details see: https://developers.google.com/closure/compiler/faq)
 * ``ol-whitespace.js`` - human readable version which can be used for debugging issues

When you encounter an issue, it is important to report the issue using ``ol-whitespace.js``. Also include the full stack trace which you can find using Web Developer tools such as Chrome's Developer Tools. To test this out we are going to make a mistake in map.html by changing ``ol.layer.Tile`` into ``ol.layer.Image``. The error you will see is: ``Uncaught TypeError: Object #<yc> has no method 'tb'``. If you report this to the mailing list, nobody will know what it means. So first, we are going to change the script tag which points to ``ol.js`` to point to ``ol-whitespace.js`` instead. Reload the page. The debugger will now stop on the error, and we can see the full stack trace:

.. figure:: debugger.png

    The debugger showing the stack trace. Using the right mouse button we can copy the call stack to clipboard.
