Beyond spell checking - what else can we check automatically?
=============================================================


(Was "Linting your docs")

.. contents::

Targets
-------

* WtD Prague 2022

Abstract
~~~~~~~~

(= 178 words)

Writing documentation is hard, and spotting errors in that documentation is
harder. Luckily, if we're working in a docs-as-code environment, we can apply
some of the techniques that programmers have been using for a long time, in
particular *linting*, the automated checking for common errors or stylistic
problems.

In this talk, I shall go through some of the types of check it is possible to
make, including spell checking (it's still important), suggesting replacements
for words/phrases (``multi-cloud`` instead of ``multicloud``), "if this then
that" rules (remember to expand an acronym on first use) and targeting checks
more closely using simple NLP (Natural Language Processing)

I'll cover the use of pre-prepared styles, and I'll give some
consideration as to how linting can be "plumbed in" to the review process.

At the end I'll demonstrate how to implement some of these techniques, using
the example of Aiven's open source developer documentation. I'll make sure to
include my favourite check, for the correct usage of `®` on product names.

Who and Why
~~~~~~~~~~~

(= 239 words)

As a software developer until the end of 2021, I've been used to both
automated checking of source code (linting) and also code review. Both are
valuable in different ways, neither is a substitute for the other.

I'm sure that all documentarians understand the value of having another
person read over their text and make comments. I want to show that there is
value in automatic checking as well, beyond spell checking and "grammar
checking".

The thing that programmers learnt was that surprisingly simple rules can give
useful results. I'll look at a sequence of such relatively simple rules that
are applicable to text, and how they can be used to detect common problems.

By the end, at least one of my example use cases should cause each audience
member to go "Aha! that sounds useful".

I shall definitely be pointing out that it's not necessarily a requirement to
write your own rule sets - that there are typically pre-bundled sets of rules
one can adopt, and for different purposes.

While we use vale as our text linting tool, the talk is not about vale, but
about the general techniques of linting documentation, and the types of check
that one might make. Vale will only be relevant torwards the end, when talking
about how we use these techniques in our own environment, and specifically in
our github review process.

Other Information
~~~~~~~~~~~~~~~~~

(= 134 words)

I've been a software developer since the 1980s, and some form of documentarian
almost as long (albeit without the use of the term). I used to recommend TeX,
but have been enthusing about reStructuredText since it was created. I gave a
talk on the history of markup languages at WtD Prague 2018.

Since the start of 2022 I've been a Developer Educator at Aiven
(https://aiven.io), and one of my first tasks was to learn about and extend
our use of vale (https://vale.sh) which we use for linting our open source
developer documentation. A particular challenge was writing the rules for
appropriate use of `®` marks, as it turned out that there was a bug in the
relevant part of vale, now fixed after my first PR to the project.


Write the Docs Writing Day
--------------------------

The first proper day of WtD Prague is normally a "Writing" day, where people
can collaborate on tasks, or work on individual tasks (in company).

It's probably a good idea to try to have a vale or lint-the-docs "table" at
the writing day, and work on some rules, or perhaps even some of the vale
issues I want to work on. ("Having a table" just means suggesting it on the
day.)


My Notes
--------

Why this is a good thing to do, and lots of examples of how doing simple
things can give good results.

Use our own vale setup as examples where appropriate, but **don't** turn this
into a vale advocacy piece.

Usefulness of being able to run in CI as well as at the command line.

Try to give a "hierarchy" of complexity, including:

* spell checking
* suggested replacements
* "if this then that" rules
* restrictions to certain parts of a document
* simple NLP
* looking at the raw markup

(use vale for inspiration here, but try and find other tools and consider what
they can do)

Consider:

* checking the markup directly
* checking a derivative of the markup (e.g., HTML produced from it) - this
  allows handling more formats at cost of being removed from the original
* marking parts of the text as "do not check" - is this a good idea, a
  sometimes good idea, a useful compromise, or just awful?
* defining and using "styles" - allowing one to share what is checked

We work in reStructuredText and in markdown. If one switches back and forth,
it's very easy to use the wrong notation. So useful rules might be:

* using the wrong sort of inline link text - ``[text](link)`` in reST, for instance
* using the wrong number of backticks for literal text - reStructuredText wants them paired
  (and uses single backticks for more specialised purposes)
* markdown doesn't support list items with alphabetic "numbering" (``a.``),
  but reStructuredText does

Maybe something on limitations, as well:

* Linting ``someone@place.io`` and:

  * vale uses ``rst2html.py`` to produce what it lints
  * sphinx produces different HTML from the same reStructuredText source

  So debugging why ``support@aiven.io`` complains that ``aiven`` should be ``Aiven``
  isn't quite as simple as it might be.

  Regardless, the *solution* probably needs a rule that looks at the raw
  markup (which I hope is reStructuredText and not HTML!)

-------

``lint`` was the name of a program written in 1978 to find common errors and
stylistic problems in C code, and it is indeed named in analogy with pulling
bits of fluff off fabric. Classically, linting programs don't actually
*understand* the programming language they're analysing - they use a set of
heuristics and rules to recognise common patterns that are likely to be mistakes.
That same approach can be applied to our documentation, and it can be
surprisingly powerful.

-------

Specific notes
--------------

Quick (very quick) history of the term linting

Benefits of simple checks, that can be fast, and give good result


Text is *not* code - code has rigorous restrictions that do not apply
to text. However, that doesn't mean that we can't take the idea of
"simple checks applied to great benefit" - the trick is in working
out the limits of "simple checks" and "great benefit".

* Spelling

  * This is not a recognised word
  * ``adn`` ``and``, ``supercede`` -> ``supersede`` simple N distance suggestions
  * anything beyond that is probably best thought of under the other sections

* ...

* If this, then must be that:

  * ``WHO`` needs an occurrence of ``WHO (World Health Organization)``

    * bonus points if can say

      * just one occurrence of the "explanation"
      * explanation must come first

  * Thing needs an occurrence of Thing®

    * bonus points if can say

      * must be used with ® in the first *title* to use the name
      * must be used with ® in the first non-title to use the name
      * first use of name *must* be with ®, regardless

    * also probably want to be able to say that if Thing® occurs, then
      **after that** in the document there must be the text "Thing® is a
      registered tradmark of Thing industries."

* ...

* Document structure

  * Only perform this check on *headings*

* NLP - allow limiting checks to particular parts of speech, etc.

  * This is when it might be possible to distinguish ``they're`` / ``their`` / ``there``
  * Harder to quantify and think about

* Complexity metrics

  * Counting word length distribution, sentence length distribution, etc.

* Original markup

  * Catch use of markdown style links (``[words](url)``) in a reStructuredText
    document (suggest ``\`words <url>\`_``)

* "Canned" styles, providing a curated set of checks

  * For instance, Google and Microsoft style guides, accessability style guides

* Errors versus warnings

* The problem of false positives

  * Should one mark, in the text, that this is not an error?
  * If one does that too much, then surely the rule is not useful
  * Possible difficulty of fine-grained "ignore this" markup - not so good
    if it's paragraph level
  * Is one saying "ignore all checks", or "ignore specific checks"

  Programming linters don't have so much problem with this - marking up a
  line to ignore is already fairly fine grained in most programming languages.
  And the tests are generally hard-coded in the linter, so generally have an
  id, and it's possible to say "ignore just this specific test".

  That's a bit harder if we're using a *framework* to define new tests.

* Problems / implementation difficulties

  * How to deal with All the markups

    * Render into HTML and check that
    * Problem examplar:
      reStructuredText -> HTML with ``rst2html`` (standalone), ``docutils``
      (more hands on), but the problem is that Sphinx has extra roles and
      directives, which rst2html/docutils doesn't recognise, and one can't
      run Sphinx on just selected files

* vale is a framework that comes with some predefined checks, and the
  ability to load packages of existing checks, but also allows you to
  define your own (and maybe release them as a package). So you get
  all the power of that approach, and also the need to mend it yourself
  if your self-written checks don't work.

* Pros and cons of commercial and open source systems, and so on.

  Warning: contains vast generalisations!

  * Commercial systems tend to come with pre-setup checks, so
    that they work "out of the box". However, that may come at
    the expense of flexibility.

    They may also need to send the text to tbe checked out into
    the cloud (where someone else's computer can do powerful stuff
    that yours might not be able to), with all the security implications
    that this implies.

  * Open source systems are more likely to come as a toolkit that
    you have to assemble yourself to get any sophisitication.
    Although pre-packaged setups may be available. It is, however,
    more likely that you'll be able to make them do new things that
    no-one else has tried. It's also likely to be easier to contribute
    if the tool doesn't do quite what you want (normal open source project
    caveats apply)

  * There must surely be closed source but free options? I suppose
    the spelling and "grammar" checking you get bundled with
    things like Word probably sort-of counts, as it's not something
    you pay extra for.

    And browser tools may even simple stuff for you...
    (that's getting a bit fuzzy)
