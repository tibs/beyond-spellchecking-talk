=============================================================
Beyond spell checking - what else can we check automatically?
=============================================================

(Was "Linting your docs")

Talk to be given at WtD Prague 2022

.. contents::

Proposal
========

Abstract
--------

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
-----------

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
-----------------

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
==========================

The first proper day of WtD Prague is normally a "Writing" day, where people
can collaborate on tasks, or work on individual tasks (in company).

It's probably a good idea to try to have a vale or lint-the-docs "table" at
the writing day, and work on some rules, or perhaps even some of the vale
issues I want to work on. ("Having a table" just means suggesting it on the
day.)


Early Notes
===========

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
==============

Quick (very quick) history of the term linting

Benefits of simple checks, that can be fast, and give good result


Text is *not* code - code has rigorous restrictions that do not apply
to text. However, that doesn't mean that we can't take the idea of
"simple checks applied to great benefit" - the trick is in working
out the limits of "simple checks" and "great benefit".

* Spelling

  * This is not a recognised word
  * ``adn`` -> ``and``, ``supercede`` -> ``supersede`` simple N distance suggestions
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

  * Catch use of markdown style links::

       [words](url)

    in a reStructuredText document - suggest::

       `words <url>`_

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

Hmm. Running a checker *after* writing (or in CI) versus having it run as you
type. Pros and cons. Certain sorts of check could be very irritating (I'm
thinking the ® check, perhaps) if they're run during typing. Not all tools
support being run as-you-type if you're using a local editor. If you're in a
browser, is it using a local service, or a remote? - see comments on cloud and
privacy. Of course, not all tools can necessarily be (easily) run in CI.
Running in CI means that not everyone needs to setup the checking - this is
actually necessary if you're going to allow people to make contributions via
(for instance) the GitHub web interface. And if you're going to run it in CI,
then it is really optional whether people run it locally. Although, turn and
turn again, that brings us back to the warning/error discussion - what should
even *show up* in CI. It also allows domain experts to fix things - this can
be important for some things (the ® check again).

Arguably, having to write one's own configuration (beyond basic spelling and
maybe some very general rules) is always going to be a requirement - only you
can know what sorts of mistake occur within the particular domain, and with
the particular people, you're working with.

For instance, for us it's worth having a rule to suggest replacing ``flick``
with ``Flink``, because (a) we're very unlikely to use the word ``flick``,
(b) we do use the product name ``Flink`` and (c) we've observed this
particular misspelling more than once in practice.

For reference: particular tools
===============================

What vale provides
------------------

In the following, "token" means a word, phrase or regular expression.

The documentation (https://vale.sh/docs/topics/styles) doesn't always
list all of the Keys that apply to each style, so the following is
likely to be incomplete on that.

``existence``

  Look to see if particular tokens exist. Supports exceptions.

  "Consider not using 'bad phrase'"

``substitution``

  Looks for token A and suggests token B instead. Supports exceptions.

  "Consider using 'B' instead of 'A'"

  *We use this*

``occurrence``

   Enforces minimum or maximum times a token appears. Supports scope
   - e.g., ``sentence``

   "More than 3 commas in sentence"

``repetition``

   Looks for repetition of its tokens.

   "'the' is repeated"

``consistency``

   Ensures key and value do not occur in the same scope.

   "Inconsistent spelling of 'center'"

``conditional``

  Ensures that if token A is present, then so it token B. Supports exceptions, scope.

  Terminology on this one is a bit confusing.

  "WHO has no definition"

  "At least one 'PostgreSQL' must be marked as ®"

  *We use this*

``capitalization``

  Checks that the text in the specified scope is capitalized according to the chosen scheme.
  Supports exceptions, scope.

  "'Badly Capitalised Heading' should be in sentence case"

  *We use this*

  Note: The capitalization metrics are *not* necessarily as simple as one might expect.
  For instance, ``$sentence`` isn't just "first word must start with a capital, rest
  must not". This is a Good Thing in practice, if harder to explain.

``metric``

  Calculates one of various arbitrary metrics and reports if it is exceeded.

  "Try to keep the Flesch-Kincaid grade level (%s) below 8"

``spelling``

  Looks up words in one or more Hunspell-compatible dictionaries. Supports filters
  and a file of words to ignore.

  "'Arglebargle' does not seem to be a word"

  *We use this*

  Note: uses the dictionary as a word list, but doesn't support all Hunspell
  capabilities. For instance, it doesn't support ``KEEPCASE`` (and ``/K``).

``sequence``

  Allows rules that specify a sequence of NLP tokens that may or may not form
  (be part of?) a sentence.

``script``

  Write a rule using arbitrary Go code (well, a Go-like scripting language)

There's also a parallel accept/reject mechanism, which allows listing tokens
to accept (add to the exception lists for all styles above) or reject (just
complain about immediately). This *looks* as if it is a good alternative to
dictionaries, but actually isn't for "reasons" (mainly that "adds to the
exception list for all styles", which is a bit of a broad brush).

Some notes on what Grammarly provides
-------------------------------------

* Spelling and grammar checking.

  * grammar mistakes
  * suggested spelling corrections
  * suggested punctuation corrections
  * with premium, word choice, tone and more.

* Plagiarism check

* Suggestions for synonyms to give better reading

* Tonal analysis (how your text may "sound" to readers)

* Rules for term usage, company name spelling/presentation, etc.

* Snippet library

* Analytics

I spent a little bit of time looking to see if I could find out how to
define rules for use in Grammarly, and couldn't find anything.

https://geediting.com/grammarly-review-how-good-is-it-an-editor-weighs-in/
seems to suggest that there's broad-scope customisation per document (to
give a general idea of what kind of feedback is wanted for that document).

Big question - does it understand markup? Since it's basically catching
key events (what you type), it doesn't really sound like their sort of
thing.

Some notes on what LanguageTool provides
----------------------------------------

https://languagetool.org/

Source code at https://github.com/languagetool-org/languagetool

Multi-language

https://dev.languagetool.org/development-overview is the documentation
on how to write new error detection rules. They're stored as XML files.

As to checking with markup - https://github.com/languagetool-org/languagetool/issues/445
(closed in 2018) suggests it's not something they see as their business to do,
nor do they have the resources. The best suggestion looks to be "convert to
plain text and check that". But see LTeX_ below...

LTeX
~~~~

https://valentjn.github.io/ltex/ - Grammar/Spell Checker Using LanguageTool
with Support for LATEX, Markdown, and Others

https://github.com/valentjn/vscode-ltex

All in one solution, offline checking, LSP (language server protocol)
support. Does support reStructuredText, at "Good" level. Works with
Emacs, Vim, VS Code.

``brew install ltex-ls``

I think this looks like a viable way to use LanguageTool with markup.

Perhaps it compares with the vale server, in some ways, as well.

Other alternatives to vale
--------------------------

The vale documentation mentions ``textlint`` and ``RedPen`` as alternatives
that handle markdown and reStructuredText (and other things), and ``alex``
as just handling markdown. It also benchmarks vale as being faster than
its competitors.

See also https://lwn.net/Articles/822969/ (Tools to improve Englist text) from 2020.

* https://textlint.github.io/ - Rules are written as plugins using JavaScript.
* https://alexjs.com/ - "Catch insensitive, inconsiderat writing". There is a vale
  plugin for at least some of the same functionality
* http://proselint.com/ and https://github.com/amperser/proselint - Rules are written
  as plugins using Python
* https://redpen.cc/ (don't confuse with ``redpen.<anything-else>`` - for imstance,
  the ``.cc`` domain appears to use real people to do checking!) and
  https://github.com/redpen-cc/redpen/ - Looks as if custom validators can be
  added as plugins in Java or JavaScript


Possibly useful links
=====================

* https://passo.uno/prose-linters-implement-workplace-howto/
* https://www.kolide.com/blog/is-grammarly-a-keylogger-what-can-you-do-about-it
  (but also points out how valuable (something like) Grammarly is, and not to
  forget that. Links to LanguageTool_ as an alternative that can
  `run using a local server`_
* https://geediting.com/grammarly-review-how-good-is-it-an-editor-weighs-in/
  gives a counterpoint - this author is an enthusiactic user
* LanguageTool_ open source, by default uses the cloud, but can
  `run using a local server`_
* https://news.ycombinator.com/item?id=32236608 an interesting discussion of
  LanguageTool on HackerNews. Includes an example of writing rules for it,
  where the commentator says "The art is trying to writing a rule without too
  much false positives."
* I have the impression that people trying to enter this space are going for
  browser and cloud based solutions, and I can understand why, but it still
  always means privacy concerns. Plus not being able to work offline(!)
* https://opensource.com/article/20/3/open-source-writing-tools from 2020
  has some interesting suggestions for open source alternatives to Grammarly
  - basically ``flyspell`` in emacs, LanguageTool via its API integration
  with editors, and the Python ``proselint`` package for grammar advice
  and style checking.

.. _LanguageTool: https://languagetool.org/
.. _`run using a local server`: https://dev.languagetool.org/http-server
