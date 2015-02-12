.. Synth-golang documentation master file, created by Lucio M. Tato
.. highlight:: go

Welcome to Synth-golang (alpha)
===============================

Synth-golang is an experimental transpiler (currently Version 0.0.0-ALPHA)

Synth-golang takes some inspiration from Clojure and Python.

The objective is to provide higher-level features within the golang ecosystem
as Clojure does for Java.

Synth-golang *will* provide: (once leaving alpha-status)

.. pull-quote::

 * Immutable values by default (as Clojure)
 * Python-like code structure (using indentation)
 * Macros (Lisp-like power macros)
 * Generic Programming 

while relaying on: golang's simplicity, powerfull concurrency primitives, golang's own
concepts as "structs" and "interface", golang's packages and libraries.

Readability
-----------

.. epigraph::

   When you program, you spend more time reading code than writing it.

   -- Paul Graham, The Python Paradox 

Synth language philosophy **emphasizes code readability**. 
Design trade-offs are always solved favoring readability. 

Synth-golang has:

* Consistent syntax
* Clean visual layout, using indentation to delimit blocks (as Python)

Contents:
---------

.. toctree::
   :maxdepth: 2

   core-concepts


   compiler
   
Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`

.. include-readme

