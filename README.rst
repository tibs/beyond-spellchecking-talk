============================================================
Beyond spellchecking - what else can we check automatically?
============================================================

This is (will be) a talk for presentation at `Write the Docs Prague 2022`_,
11th - 13th September 2022.

It will be presented as a video - the link to that video will be added here
when it is available.

It will (probably) also be presented beforehand as a live talk at CamPUG_
on 6th September 2022 - a link to the meetup page will also be added when
possible.

.. _`Write the Docs Prague 2022`: https://www.writethedocs.org/conf/prague/2022/
.. _CamPUG: https://www.meetup.com/CamPUG/


The slides
~~~~~~~~~~

The slides are (unsurprisingly) written using reStructuredText_, and thus
intended to be readable as plain text.

The sources for the slides are in `<slides.rst>`_.

(Note that github will present the ``.rst`` files in rendered form as HTML,
albeit using their own styling (which is occasionally a bit odd). If you want
to see the original reStructuredText source, you have to click on the "Raw"
link at the top of the file's page.)

The PDF slides at 16x9 aspect ratio (`<rst-slides-16x9.pdf>`_) are stored here
for convenience.

The PDF files may not always be as up-to-date as the source files, so check
their timestamps.

The slide notes
~~~~~~~~~~~~~~~

There are also notes for the slides. The intent is that these are readable
as a stand-alone document - we'll see how that goes...

The sources for the notes are in `<notes.rst>`_

(Note that github will present the ``.rst`` files in rendered form as HTML,
albeit using their own styling (which is occasionally a bit odd). If you want
to see the original reStructuredText source, you have to click on the "Raw"
link at the top of the file's page.)

For convenience, there will also be a PDF rendering of the notes,
`<notes.pdf>`_

Making the PDF files
~~~~~~~~~~~~~~~~~~~~
You can use the Makefile to create the PDF files.
For instance::

  $ make pdf

to make them all.

For what the Makefile can do, use::

  $ make help

I use poetry_ to manage the dependencies needed to build the PDFs, and
rst2pdf_ and its dependencies to do the actual work.

.. _poetry: https://python-poetry.org/
.. _rst2pdf: https://rst2pdf.org/

You will also need an appropriate ``make`` program if you want to use the
Makefile.

.. _CamPUG: https://www.meetup.com/CamPUG/
.. _reStructuredText: http://docutils.sourceforge.net/rst.html

--------

  |cc-attr-sharealike|

  This talk and its related files are released under a `Creative Commons
  Attribution-ShareAlike 4.0 International License`_.

.. |cc-attr-sharealike| image:: images/cc-attribution-sharealike-88x31.png
   :alt: CC-Attribution-ShareAlike image

.. _`Creative Commons Attribution-ShareAlike 4.0 International License`: http://creativecommons.org/licenses/by-sa/4.0/
