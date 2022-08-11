Beyond spellchecking - What else can we check automatically
===========================================================


.. class:: title-slide-info

    By Tibs / Tony Ibbs (they / he)

    .. raw:: pdf

       Spacer 0 30

    Slides and accompanying material at https://github.com/tibs/beyond-spellchecking

.. footer::

   *tony.ibbs@aiven.io* / *@much_of_a*

   .. Add a bit of space at the bottom of the footer, to stop the underlines
      running into the bottom of the slide
   .. raw:: pdf

      Spacer 0 5

.. |cross| image:: images/green-cross.png
.. |think| image:: images/thinking-face-emoji.png

Introduction
------------

Writing documentation is hard, and spotting errors in that documentation is
harder. What can we do to help?

* "Linting"
* A review of checks we might make
* Adopt existing checks or grow your own
* Some tools
* Plumbing checks into docs-as-code
* What we do at Aiven

"linting"
---------

``lint`` was a program written in 1978 to find common errors and stylistic
problems in C code

``lint`` = bits of fluff on fabric, hence *linting*, to remove them

Simple checks, that can be fast, and give good results

But remember, text is not as restricted as code

Types of check
--------------

Not an exhaustive list, but with example messages


|cross| Spelling
----------------

``'arglebargle' does not seem to be a word``

.. raw:: pdf

   Spacer 0 30

Ignores numbers, some punctuation...

|think| But I really did mean ``arglebargle``
---------------------------------------------

Sometimes, a mistake isn't a mistake::

  'arglebargle' is not actually a word.

* Put up with it
* Annotate in the text (``.. lint: off`` / ``.. lint: on``)
* Configure to ignore ``arglebargle``
* Configure to ignore ``arglebargle`` *in this file / location*

|cross| Repetition
------------------

``'the' is repeated``

::

    The cat
    and the
    the dog

|cross| Don't say that
----------------------

``Consider not using 'it is obvious that'``

|cross| Use *this* instead of *that* - errors
---------------------------------------------

``Use 'and' instead of 'adn'``

``Use 'supersede' instead of 'supercede'``

``Use 'Aiven for Redis' instead of 'Aiven Redis'``

|cross| Use *this* instead of *that* - suggestions
--------------------------------------------------

``Consider using 'flink' instead of 'flick'``

``Consider using 'for instance' instead of 'e.g.'``

|think| Errors versus warnings
------------------------------

An error must be fixed, and will cause a build to fail

A warning is just a warning

What do you do with a warning?

|think| Create tests you need, retire them when not
---------------------------------------------------

if the person who mistypes ``adn`` leaves the team

you probably don't still need the check for ``"adn" should be replaced by "and"``

|think| Against auto-correction
-------------------------------

Written text is complicated, and linting will sometimes find false positives

Auto-correction can lead to unexpected results

The final decision should be with a human

|cross| Too many / too few
--------------------------

``More than 3 commas in sentence``

|cross| One or the other, not both
----------------------------------

``Inconsistent spelling of 'center' and 'centre'``

|cross| If *this* is present, then we need *that*
-------------------------------------------------

``WHO has no definition``

``At least one use of 'PostgreSQL' must be marked as ®``

|think| word versus token versus ...
------------------------------------

``word`` - like in a dictionary

``token`` - like in a parser - more general

``expression`` - like a regular expression, a pattern to match

|think| scope
-------------

"Scope" - some part of a document

``Thing`` must be used with ® in the first *title* to use the name

``Thing`` must be used with ® in the first non-title to use the name

First use of ``Thing`` *must* be with ®, regardless

|cross| Capitalisation
----------------------

``'Badly Capitalised Heading' should be in sentence case``

But consider carefully:

``iPhone prices``

``The importance of NASA``

``Remembering Terry Jones``


|cross| Looking at the raw text
-------------------------------

Checking reStructuredText:

``One backtick without a role becomes italics``

``Use reStructuredText link format, not markdown``

Checking markdown:

``Two backticks is redundant - did you mean just one?``


|think| Checking for absence
----------------------------

For instance, that all images have ``alt`` text

``Image is missing alt text``

Not the same as "is zero length" - we want *structural element* occurs zero times

|cross| Arbitrary metrics
-------------------------

``Try to keep the Flesch-Kincaid grade level (12) below 8``

|cross| Sentence analysis
-------------------------

NLP (Natural Language Processing)

``Did you mean "cars are" instead of "car's are"``

``Don't use "like" as an interjection``

|cross| Just let me code
------------------------

Writing a plugin with access to knowledge of the document structure

Pre-built or hand-designed
--------------------------

Adopt an existing package - Microsoft or Google styles

Do that and add customisations

Start from scratch and specify everything yourself

...but if you do your own checks, consider contributing back to the community

Available tools
---------------

Just a brief overview...

* Vale
* LTeX and LanguageTool
* alexjs
* proselint
* redpen
* textlint

((**Do I want one slide per tool, or just to talk over the list?**))

Vale
----

Vale_ is a command line tool, open source, offline for privacy

Various packaged rulesets are available

Rules ("Styles") are specified via YAML files that build on existing concepts,
or (less often) via code in a Go-like language

.. _Vale: https://vale.sh

LTeX and LanguageTool
---------------------

LTeX_ provides offline grammar checking of various markup languages using
LanguageTool_

BibTeX, ConTeXt, LaTeX, Markdown, Org, reStructuredText, R Sweave, and XHTML

New rules for LanguageTool are stored as XML files

.. _LTeX: https://valentjn.github.io/ltex/
.. _LanguageTool: https://languagetool.org/

alexjs
------

alexjs_ is designed to "Catch insensitive, inconsiderate writing" in markdown documents

.. _alexjs: https://alexjs.com/

proselint
---------

proselint_

Rules are written as plugins using Python

.. _proselint: http://proselint.com/

redpen
------

redpen_

Custom validators can be written as plugins in Java or JavaScript

.. _redpen: https://redpen.cc/


textlint
--------

textlint_

Rules are written as plugins using JavaScript

.. _textlint: https://textlint.github.io/

Plumbing checks into docs-as-code
---------------------------------

Local checks
------------

In the editor - display messages as you're typing, or on saving

At the command line - run a command to make the checks

Checks before commit
--------------------

Don't allow ``commit`` if there are errors

*This may be a bit extreme?*

Checks before review
--------------------

Run checks when change are pushed for review

The reviewers can see the results

Forbid merging if there are errors?

*Seems more reasonable*

On GitHub, use workflows for this

Checks before deployment
------------------------

Don't deploy if there are errors

*Probably a good idea* - **if** the previous stages mean this essentially
never happens


Plumbing in to CI (continuous integration)
------------------------------------------

Run the checks automatically when a review is requested (GitHub: PR) or before
deploying the documentation

No errors before deployment...

What we do at Aiven
-------------------

We lint Aiven's developer documentation

https://developer.aiven.io/ and https://github.com/aiven/devportal

We use Vale
-----------

It's a small program, it's fast, it's portable, it's very configurable

Development is ongoing, the code is readable, the author fixes bugs quickly

It's well known in the WtD community

But we did (do) need to configure it, and it's a relatively small project

The checks we use
-----------------

* ``spelling`` - Spell checking - the default US-en dictionary, plus our own
* ``capitalization`` - Capitalisation in headings
* ``substitution`` - Use *this* instead of *that*
* ``conditional`` - If *this* then *that*, for `®` checking

At the command line
-------------------

``make spell``

In CI (continuous integration)
------------------------------

We use `vale-action`_, the official GitHub action for Vale

We run checks:

* For a PR (pull request)
* When pushing to ``main`` (in theory...)

.. _`vale-action`: https://github.com/errata-ai/vale-action


|think| What have we learnt?
----------------------------

* We can check things beyond spelling
* Relatively simple techniques can be useful
* But don't check for the sake of it
* There is a good choice of tools available
* You don't have to build it yourself
* You can check as part of your docs-as-code toolchain



.. -----------------------------------------------------------------------------

.. raw:: pdf

    PageBreak twoColumnNarrowRight

Fin
---

Come join us on `Write the Docs slack`_ channel `#testthedocs`_

Slides and accompanying material at https://github.com/tibs/beyond-spellchecking

Written in reStructuredText_, converted to PDF using rst2pdf_

|cc-attr-sharealike| This slideshow is released under a
`Creative Commons Attribution-ShareAlike 4.0 International License`_

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
.. _Aiven: https://aiven.io/
.. _`Write the Docs slack`: https://writethedocs.slack.com
.. _`#testthedocs`: https://writethedocs.slack.com/archives/CBWQQ5E57
