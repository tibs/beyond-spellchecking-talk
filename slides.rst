Beyond spellchecking - What else can we check automatically
===========================================================


.. class:: title-slide-info

    By Tibs / Tony Ibbs (they / he)

    .. raw:: pdf

       Spacer 0 10

    Presented at `Write the Docs Prague 2022`_

    .. raw:: pdf

       Spacer 0 10

    Written in reStructuredText_, converted to PDF using rst2pdf_.

    .. raw:: pdf

       Spacer 0 10

    Slides and accompanying material at https://github.com/tibs/beyond-spellchecking

.. footer::

   *tony.ibbs@aiven.io* / *@much_of_a*

   .. Add a bit of space at the bottom of the footer, to stop the underlines
      running into the bottom of the slide
   .. raw:: pdf

      Spacer 0 5


Here be slides
--------------

And this is not the first of them

Note to self: first pass at slide content, by copying the current ToC from the
notes document (!)

Let's not take that too seriously...

Introduction
------------

Origins of "linting"
--------------------

``lint`` was the name of a program written in 1978 to find common errors and
stylistic problems in C code, and it is indeed named in analogy with pulling
bits of fluff off fabric.

We're after simple checks, that can be fast, and give good results.

But rememember, text is not as restricted as code.

Types of check
--------------

Let's have a run through what we might do given that definition...

Note: the messages described are not necesssarily from any program, and may be
made up...

Spelling
--------

* ``'Arglebargle' does not seem to be a word``

Aside: word versus token versus ...
-----------------------------------


Repetition
----------

* ``'the' is repeated``

::

    The cat
    and the
    the dog

Don't say that
--------------

* ``Consider not using 'it is obvious that'``

Use *this* instead of *that*
----------------------------

Errors:

* ``Use 'and' instead of 'adn'``
* ``Use 'supersede' instead of 'supercede'``
* ``Use 'Aiven for Redis' instead of 'Aiven Redis'``

Use *this* instead of *that*
----------------------------

Suggestions:

* ``Consider using 'flink' instead of 'flick'``
* ``Consider using 'for instance' instead of 'e.g.'``

Aside: Create tests you need, retire them when they're not
----------------------------------------------------------

If the person who mistypes ``adn`` leaves the team

you probably don't still need the check for ``"adn" should be replaced by "and"``

Aside: Why auto-correction is not (generally) a (good) thing
------------------------------------------------------------


Too many / too few
------------------

* ``More than 3 commas in sentence``

One or the other, not both
--------------------------

* ``Inconsistent spelling of 'center' and 'centre'``

If this is present, then that must also be present
--------------------------------------------------

* ``WHO has no definition``
* ``At least one use of 'PostgreSQL' must be marked as ®``

Aside: scope
------------

We would often like to be more specific:

* ``Thing`` must be used with ® in the first *title* to use the name
* ``Thing`` must be used with ® in the first non-title to use the name
* First use of ``Thing`` *must* be with ®, regardless

Capitalisation
--------------

* ``'Badly Capitalised Heading' should be in sentence case``

But consider carefully:

* ``iPhone prices``
* ``The importance of NASA``
* ``Remembering Terry Jones``


Aside: Looking at the raw text
------------------------------

Checking reStructuredText:

* ``One backtick without a role becomes italics``
* ``Use reStructuredText link format, not markdown``

Checking markdown:

* ``Two backticks is redundant - did you mean just one?``


Aside: Checking there is alt text on images
-------------------------------------------

* ``Image is missing alt text``

Arbitrary metrics
-----------------

* ``Try to keep the Flesch-Kincaid grade level (12) below 8``

NLP (Natural Language Processing) sentence analysis
---------------------------------------------------

* ``Did you mean "cars are" instead of "car's are"``
* ``Don't use "like" as an interjection``

Arbitrary script / plugin
-------------------------

Pre-built or hand-designed
--------------------------

What it checks
--------------

Errors versus warnings
----------------------

Plumbing in to CI
-----------------

Tools for spellchecking
-----------------------

But a brief overview...

* alex
* Vale
* textlint
* proselint
* redpen
* LanguageTool and LTeX

Use in Aiven's developer documentation
--------------------------------------

We use Vale
-----------

The checks we use
-----------------

Use at the command line
-----------------------

Use in CI
---------



.. raw:: pdf

    PageBreak twoColumnNarrowRight

Fin
---

Slides and accompanying material at https://github.com/tibs/beyond-spellchecking

Written in reStructuredText_, converted to PDF using rst2pdf_

|cc-attr-sharealike| This slideshow and its related files are released under a
`Creative Commons Attribution-ShareAlike 4.0 International License`_.

.. image:: images/qr_beyond_spellchecking.png
    :align: right
    :scale: 90%

.. And that's the end of the slideshow

.. |cc-attr-sharealike| image:: images/cc-attribution-sharealike-88x31.png
   :alt: CC-Attribution-ShareAlike image
   :align: middle

.. _`Creative Commons Attribution-ShareAlike 4.0 International License`: http://creativecommons.org/licenses/by-sa/4.0/

.. _`Write the Docs Prague 2022`: https://www.writethedocs.org/conf/prague/2022/
.. _reStructuredText: http://docutils.sourceforge.net/docs/ref/rst/restructuredtext.html
.. _rst2pdf: https://rst2pdf.org/
