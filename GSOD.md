<!-- # Improving feature discoverability by standardizing documentation of
"implicit" types -->
<!--
.. Comments from discussion with Hannah
.. And you had mentioned (I think on a previous call at some point?) that you
   had done some work on something like a "type theory for matplotlib in Haskell".
   That's exactly what I was hoping to propose! Even if Python doesn't have a
   strong enough type system for us to specify the new API completely, the central
   thesis of my proposal was intended to be that....if we documented matplotlib as
   if we did have a strong enough type system, it would naturally lead to us having
   good documentation for a lot of these key concepts in the matplotlib taxonomy
   which are currently only documented "implicitly"
.. Propose alternate path here, instead of explicitly classes, propose we
   incorporate our own "numpy-alike" types, like "color_like", "array_like",
   etc. for types that don't exist yet. Then these can link to pages where we
   document what each of these can be.
.. Initial impression: including validation here as well as documentation is
   maybe scope creep.
-->

## Summary

*Matplotlib* is the *de facto* standard library atop which the bulk of the
Python visualization ecosystem is built, including such popular packages as
`cartopy`, `pandas`, `seaborn`, and many others.

Over the years, *Matplotlib* has evolved to contain a stunning array of
featuresr. However, discoverability remains a central problem for our
documentation.  Existing documentation can be broken up largely into
**tutorials**, **API documentation**, and **examples**, (ignoring "meta"
documentation, e.g. installation how-to's, contribution guides, etc.).
However, because there is little to no organizational structure to guide
someone to the correct example or tutorial page, the best way to figure out the
"latest and greatest" way to accomplish a task is often to just ask the
community, even for core developers.

In order to remedy this problem, I propose to integrate the API documentation
with a large swath of existing tutorials and examples by attempting to
systematically document what I will call *Matplotlib*'s "implicit types".
These are a set of core concepts which, due to their "duck type"-heavy nature,
have not yet merited their own proper class, but nonetheless are commonly
accepted as inputs to a variety of *Matplotlib* routines internally, and whose
documentation is currently scattered throughout docstrings, tutorials, and
sometimes even just in examples.

<!--
The goal is to create a path to discoverability of *Matplotlib*'s core features
for one of our largest user groups: the so-called "copy/paste/edit" developer.
These users rely on Googling to find an example of a plot that does something
similar enough to what they want that they can edit the code to satisfy their
own visualization needs. These users discover features by simply looking at the
API documentation for only the API surface used by the examples they find, and
they tend to report that *Matplotlib* is not "capable" or "useful" for tasks
that they do not find
-->

These *Matplotlib*'s API is designed to making simple things simple and complicated things possible, so a single argument to a function often has a "beginner friendly" type (for example, "r" for color, or "-." for linestyle) along with a  this means that this type of user is unlikely to stumble into the tutorials that cover more

Documenting the "implicit types" of *Matplotlib* would help remedy this by making

Currently, the documentation of each "implicit type" is repeated ad hoc at each
point in the documentation that needs to talk about it. This leads to users
having very different understandings of a given *Matplotlib* feature can do
depending on where in the docs they stumble into it. For example, trying to
learn how to use all but the most basic colors is impossible starting from the
documentation for our most-used function,
[`plt.plot`](https://matplotlib.org/3.2.2/api/_as_gen/matplotlib.pyplot.plot.html), however

I propose the

 Sphinx/Readthedocs infrastructure by simply
creating a new reStructuredText
[role](https://docutils.sourceforge.io/docs/ref/rst/roles.html) `:type:` which
can be used in place of the usual numpydoc type syntax when appropriate.  For
example, in `Line2D`:

```python
"""
color : :type:`Color`, default: :rc:`lines.edgecolor`
"""
```

would link to a comprehensive "Color" tutorial (currently links to
`Line2D.set_color`).

The goals of this change are to

1. Make it to find the "common"/"easiest" option for a parameter (preferably in zero clicks).
2. Make it easy to "scroll down" to see more advanced options (preferably with one click, max).
3. Provide a centralized strategy for linking top-level "API" docs to the relevant "tutorials".
4. Avoid API-doc-explosion, where scanning through the many possible options to each parameters makes individual docstrings unwieldy.

## User research

Because matplotlib users are such a diverse group, creating documentation that
works for everyone---from those for whom matplotlib is their introduction to
Python to seasoned data scientists and library writers hoping to wrap
matplotlib into their own packages---is a herculean task. In order to identify
what unmet needs are most relevant for different kinds of users, I watched
several matplotlib users undertake assigned tasks using the library, where the
users were split into three different categories of experience with matplotlib
and Python:

1. Experienced with matplotlib and Python
2. Experienced with Python, but not plotting or matplotlib specifically
3. Experienced with design, but with neither Python nor matplotlib

TODO: Fill in this section with full write-up.

The main takeaway was that both types 1 and 2 are "copy/paste/edit" developers
who do the majority of their editing with the docs pulled up, and only discover
features by googling in plain english when they have a specific task they want
to accomplish that they don't immediately found in the API docs (or in the docs
for the example they are copying from).


<!--
## Implicit types

There is currently a family of matplotlib concepts whose documentation is
contained almost exclusively within the docstrings of functions which take
arguments of that type (or sometimes in tutorials). Common examples are
``linestyle``, ``joinstyle/capstyle``, and ``bounds/extents`` (for a full list,
see below). I will call these not-quite-types "implicit types".

As some of these implicit types became used by more and more functions, their
documentation has become fractured across the various files that use them. For
example, the ``linestyle`` parameter is accepted in many places, including
the Line2D constructor, Axes methods, various `~.matplotlib.collections`
classes, and of course in rc. While `~.matplotlib.lines.Line2D` fully documents
it only some Axes methods link to this documentation, others simply hint at the
available options.

Input checking for these implicit types tends to be repeated across many files and
is too easy to do incorrectly or inconsistently. For example, the ``joinstyle``
and ``capstyle`` parameters have validators in ``rcsetup.py``. However, while
these are used in ``patches.py`` and ``collections.py``, they are not used in
``markers.py``, and ``backend_bases.py`` calls ``cbook._check_in_list`` with its
own list of possible valid joinstyles.

In order to prevent further fragmentation of docs (and validation), I propose that
each such concept , where we can centralize its
documentation. All functions that accept such an argument will then easily be
able to link to it using the standard numpydoc syntax in their docstrings, and
the description of these parameters can instead be changed to point to relevant
tutorials, instead of an ad-hoc rehashing of already existing documentation.
Error checking would be centralized to that class instead of being scattered
throughout several `cbook._check_in_list` calls that are liable to become stale. -->


### Motivation

Historically, matplotlib's API has relied heavily on string-as-enum
"implicit types". Besides mimicking matlab's API, these parameter-strings allow the
user to pass semantically-rich values as arguments to matplotlib functions
without having to explicitly import or verbosely prefix an actual enum value
just to pass basic plot options (i.e. ``plt.plot(x, y, linestyle='solid')`` is
easier to type and less redundant than something like ``plt.plot(x, y,
linestyle=mpl.LineStyle.solid)``).

Many of these string-as-enum implicit types have since evolved more
sophisticated features. For example, a ``linestyle`` can now be either a string
or a 2-tuple of sequences, and a MarkerStyle can now be either a string or a
``matplotlib.path.Path``. While this is true of many implicit types, MarkerStyle
is the only one (to my knowledge) that has the status of having been upgraded to
a proper Python class.

Because these implicit types are not classes in their own right, Matplotlib has
historically had to roll its own solutions for centralizing documentation and
validation of these implicit types (e.g. the ``docstring.interpd.update`` docstring
interpolation pattern and the ``cbook._check_in_list`` validator pattern,
respectively) instead of using the standard toolchains provided by Python
classes (e.g. docstrings and the validate-at-``__init__`` pattern,
respectively).

While these solutions have worked well for us, the lack of an explicit location
to document each implicit type means that the documentation is often difficult
to find, large tables of allowed values are repeated throughout the
documentation, and often an explicit statement of the *scope* of an implicit
type is completely missing from the docs. Take the ``plt.plot`` docs, for
example: in the "Notes", a description of the matlab-like format-string styling
method mentions ``linestyle``, ``color``, and ``markers`` options. There are
many more ways to pass these three values than are hinted at, but for many
users, this is their only source of understanding about what values are possible
for those options until they stumble on one of the relevant tutorials. A the
table of ``Line2D`` attributes is included in an attempt to show the reader what
options they have for controlling their plot. However, while the ``linestyle``
entry does a good job of linking to ``Line2D.set_linestyle`` (two clicks
required) where the possible inputs are described, the ``color`` and ``markers``
entries do not. ``color`` simply links to ``Line2D.set_color``, which fails to
offer any intuition for what kinds of inputs are even allowed.

It could be argued that this is something that can be fixed by simply tidying up
the individual docstrings that are causing problems, but the issue is
unfortunately much more systemic than that. Without a centralized place to find
the documentation, this will simply lead to us having more and more copies of
increasingly verbose documentation repeated everywhere each of these implicit
types is used, making it especially more difficult for beginner users to simply
find the parameter that they need. However, the current system, which forces
users to slowly piece together their mental model of each implicit type through
wiki-diving style traversal throughout our documentation, or piecemeal from
StackOverflow examples, is also not sustainable.

### End Goal

Ideally, any mention of an implicit type should link to a single page that
describes all the possible values that type can take, ordered from most simple
and common to most advanced or esoteric. Instead of using valuable visual
space in the top-level API documentation to piecemeal enumerate all the possible
input types to a particular parameter, we can then use that same space to give a
plain-word description of what plotting abstraction the parameter is meant to
control.

To use the example of ``linestyle`` again, what we would want in the
``LineCollection`` docs is just:

1. A link to complete docs for allowable inputs (a combination of those found in
   ``Line2D.set_linestyle`` and the [linestyle
   tutorial](https://matplotlib.org/3.2.2/gallery/lines_bars_and_markers/linestyles.html)).
2. A plain words description of what the parameter is meant to accomplish. To
   matplotlib power users, this is evident from the parameter's name, but for
   new users this need not be the case.

The way this would look in the actual ``LineCollection`` docs is just
```python
"""
linestyles: `LineStyle` or list thereof, default: :rc:`lines.linestyle` ('-')
    A description of whether the stroke used to draw each line in the collection
    is dashed, dotted or solid, or some combination thereof.
"""
```
where the ``LineStyle`` type reference would be resolved by Sphinx to point
towards the a single, authoritative, and complete set of documentation for how
Matplotlib treats linestyles.

### Benefits

Some powerful features of this approach include

1. Making the complete extent of what each function is capable of obvious in
   plain text (with zero clicks required).
2. Making the default option visible (with zero clicks). Seeing default option
   is often enough to jog the memory of returning users.
3. Make a complete description of the "most common" and "easiest" options for a
   parameter easily available when browsing (with a single click).
4. Make the process of discovering more powerful features and input methods as
   easy as "scroll down" to see more advanced options (with still only one
   click).
3. Provide a centralized strategy for linking top-level "API" docs to the relevant "tutorials".
4. Avoid API-doc-explosion, where scanning through the many possible options to each parameters makes individual docstrings unwieldy.

Other benefits of this approach over the current docs are:

1. Docs are less likely to become stale, due to centralization.
2. Canonicalization of many of matplotlib's "implicit standards" (like what is a
   "bounds" versus an "extents") that currently have to be learned by reading
   the code.
3. The process would highlight issues with API consistency in a way that
   can be more easily tracked via the Github issues tracker, helping with the
   process of improving our API.
4. Faster doc build times, due to significant decreases in the amount of
   text needing to be parsed.

### Implementation

The improvements described above will require two major efforts for which a
dedicated technical writer will be invaluable. The first is to create one
centralized "tutorial" page per implicit type. This will require working with
the core developer team to identify a concrete list of implicit types whose
documentation would be valuable to users (typically, because they contain
powerful, hidden features of our library whose documentation is currently only
found in difficult-to-stumble-across tutorials). For each implicit type, I will
then synthesize the various relevant tutorials, API docs, and example pages into
a single authoritative source of documentation that can be linked to anywhere
that particular type is referenced.

Once the centralized documentation for a given implicit type is complete, the
second major effort begins: replacing existing API documentation with links to
the new documentation, with an eye towards making the experience of actually
using this new documentation as easy as possible, both for those using Python's
built-in ``help()`` utility and for those browsing our documentation online.

While the exact format of the documentation proposed here is subject to change
as this project evolves, I have worked with the *Matplotlib* core team during
their weekly "dev calls" to establish a consensus that the strategy proposed
here is the most expedient, useful, and technically tractable approach to begin
documenting these "implicit types" (notes on these
calls are available [on hackmd](https://hackmd.io/MNcskNfZSIOoLb50GqVIGg?view)).
I will use the existing "tutorials" infrastructure for the initial stages of
creating the centralized documentation for each implicit type, allowing me to
easily reference these pages as follows, without having to create any new public
classes (again, using the ``LineCollection`` docs as an example):

```python
"""
linestyles: LineStyle or list thereof, default: :rc:`lines.linestyle` ('-')
    A description of whether the stroke used to draw each line in the collection
    is dashed, dotted or solid, or some combination thereof. For a full
    description of possible LineStyle's, see :doc:`tutorials/types/linestyle`.
"""
```

Moving forward, we could then easily change how these references are spelled
once the core developer team agrees on the best long-term strategy for
incorporating our new "types" documentation into bona fide Python classes, for
example as proposed in the [Matplotlib Enhancement Proposal
30](https://github.com/matplotlib/matplotlib/pull/17446).

Finally, the preliminary list of implicit types that I propose documenting
during this Google Season of Docs are:

1. ``capstyle``
2. ``joinstyle``
3. ``bounds``
4. ``extents``
5. ``linestyle``
6. ``colors``/``lists of colors``
7. ``colornorm/colormap``
8. ``tick formatters``

A living version of this document can be found [on our
Discourse](https://discourse.matplotlib.org/t/improving-feature-discoverability-by-centralizing-documentation-of-implicit-types/21263).
