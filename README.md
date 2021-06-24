# The `example-sentences` package for typesetting linguistic examples


Linguists (and scholars in related fields) use example sentences in 
their  writing. There is a fairly accepted standard format for 
presenting those: Example sentences are numbered continuously throughout 
a document, and optionally can have subexamples, as in (2) below.

![An example of an example](http://www.sven-lauer.net/files/examples/simple_example.png?new)

The platonic ideal of a LaTeX package for typesetting such examples 
would provide an environment that behaves as much as possible as LaTeX's
built-in `enumerate` lists (because, at heart, example lists *are* just
ordered lists). So the above examples should be typeset as:

```latex
\begin{examples}
  \item This is an example.
  \item \begin{examples}
          \item This a sub-example.
          \item This is another sub-example.
        \end{examples}
\end{examples}
```

The existing packages fall short of this ideal in several ways: 

- [linguex](https://www.ctan.org/tex-archive/macros/latex/contrib/linguex?lang=en)
  has a concise WYSIWYGish syntax that is very readable (which is why I favored it 
  for a long time), but it betrays its TeX roots through various quirks. For 
  example, it has very  particular whitespace requirements, which can make
  working with it a hassle.
- The same is true for [expex](https://www.ctan.org/pkg/expex?lang=en), 
  which also features a rather exotic syntax that I've always found too 
  difficult to remember. 
- [gb4e.sty](https://www.ctan.org/pkg/gb4e?lang=en) is more LaTeXy
  than the other ones, but it has a number of strange inconsistencies.
  
As I was dissatisfied with this state of affairs, I decided to write my own package.

## Table of contents
* [Installation](#installation)
* [Basic Usage: Zero learning curve](#basic-usage-zero-learning-curve)
* [Syntactic sugar: Some enhancements for \item](#syntactic-sugar-some-enhancements-for-item)
    * [Diacritics with \item&lt;&gt;](#diacritics-with-item)
    * [Assigning labels with \item()](#assigning-labels-with-item)
ef{...}, \ex{...}, and \exref{...}](#referencing-examples-in-the-text-ref-ex-and-exref)
* [Typesetting dialogues withegin{conversation} ... \end{conversation}](#typesetting-dialogues-with-beginconversation--endconversation)
* [Glosses](#glosses)
* [Customization](#customization)
* [Package Options](#package-options)
    * [Compatibility options: Turning of convenience commands](#compatibility-options-turning-of-convenience-commands)
        * [Leaving `\item` alone: the `normalitem` option](#leaving-item-alone-the-normalitem-option)
        * [Turning off the short `\ex` reference command](#turning-off-the-short-ex-reference-command-with-the-noex-option)
        * [For the ascets: The `compat` option](#for-the-ascets-the-compat-option)
    * [The enumitemize option](#the-enumitemize-option)
    * [Other options](#other-options)
* [Dependencies](#dependencies)
* [Compatibility with beamer](#compatibility-with-beamer)
    * [beamer's examples-environment](#beamers-examples-environment)
    * [Using beamerarticle.sty](#using-beamerarticlesty)
* [Known issues](#known-issues)
    * [Beginning an example with [...] or (...)](#beginning-an-example-with--or-)
    * [Diacritics with cgloss4e](#diacritics-with-cgloss4e)
    * [Footnotes](#footnotes)

## Installation

As usual, simply place the style files some place where TeX can 
find them (the directory where your tex files live will work if all else 
fails).

Then you can load it in the usual way, by placing the following in the 
preamble of your document:
```latex
\usepackage{example-sentences}
```

There are a number of [package options](#package-options), described 
below.

## Basic Usage: Zero learning curve

If you know how to use the `enumerate` environment LaTeX provides, you 
already know how to use `example-sentences`:

```latex
\begin{examples}
    \item This is the first example.
    \item This is second example.
\end{examples}
```

This will produce:

![Two examples](http://www.sven-lauer.net/files/examples/two_examples.png)

Nesting works, of course, up to three levels deep:

```latex
\begin{examples}
    \item \begin{examples}
               \item An example with sub-examples.
               \item \begin{examples}
                       \item And sub-sub examples, like this one.
                       \item And a second one.
                     \end{examples}
           \end{examples}
\end{examples}
```

which will render as:

![Sub- and sub-sub examples](http://www.sven-lauer.net/files/examples/sub-examples.png)

And, as usual, you can give an optional argument to `\item` to manually
specify the label (e.g., if you are quoting an example, and want to use 
the original example number):

```latex
\begin{examples}
    \item[(23)] This example will be numbered (23).
\end{examples}
```

In fact, this is almost all you need to know in order to produce 
everyting that `example-sentences` can produce. The only additional thing
to know is how to typeset diacritic marks indicating acceptability and 
such. You can do this explicitly by using the `\diacritic{}` command:
```latex
\begin{examples}
    \item \diacritic{*} Bad sentence this sounds.
\end{examples}
```
Diacritics are typeset in the space between example number and example. 
So the above will render as:

![Example with diacritic](http://www.sven-lauer.net/files/examples/diacritic-example.png)

However, explicit use of the `\diacritic{}` command is discouraged. 
Instead, you should use the convenience macros described below.

## Syntactic sugar: Some enhancements for `\item`

A number of enhancements of the basic `\item` command are available, 
unless your LaTeX installation is truly ancient. 

### Diacritics with `\item<>`

All variants of `\item` allow for 
an optional argument in angle brackets containing a 
diacritic/acceptability mark:

```latex
\begin{examples}
    \item<*> Bad sentence this sounds.
\end{examples}
```

This renders as

![Example with diacritic](http://www.sven-lauer.net/files/examples/diacritic-example.png)

This style is preferred over the explicit use of `\diacritic{}`.

### Assigning labels with `\item()`

The basic `\label{}` command works as it always does. However, a more 
convenient way to assign labels to examples is provided. Simply supply 
the desired `label` in parentheses after `\item`:
```latex
\begin{examples}
    \item(harlem) If you want to go to Harlem, you have to take 
                  the A train.
\end{examples}
```
This is equivalent to:
```latex
\begin{examples}
    \item\label{harlem} If you want to go to Harlem, you have to take 
                        the A train.
\end{examples}
```
Of course, this style can be used with diacritics, `\item(labelname)<*>` is valid and 
produces the expected result. The same is true for `\item[exnumber]<*>`.

## Referencing examples in the text: `\ref{...}`, `\ex{...}`, and `\exref{...}`

The standard `\ref{}` command works for examples. However, it produces 
the (sub)example number without enclosing parentheses. This means that 
it would have to be used in running text with explicit parentheses: 
```latex
The sentence in (\ref{harlem}) is curious in that ...
```
This is by design, because it is sometimes necessary to access the 
example identifier itself. An example is if one wants to reference a 
range of sub-examples, as in:
```latex
(\ref{imperatives}a-f) show the varied uses of imperatives.
```
However, the package provides two handy shortcuts for example references. 
Instead of writing `(\ref{harlem})`, you can simply write either:
```latex
The sentence in \ex{harlem} is curious in that ...
```
or:
```latex
The sentence in \exref{harlem} is curious in that ...
```

And, as an added nicety, you can provide arbitrary material that is 
appended to the example label as an optional argument:
```latex
\ex{imperatives}[a-f] show the varied uses of imperatives.
```
## Typesetting dialogues with `\begin{conversation} ... \end{conversation}`

Semanticists and pragmaticists often need to typeset short dialogues, 
like this:

![Example of a conversation](http://www.sven-lauer.net/files/examples/example-conversation.png)

If you load `example-sentences` with the `conversations` option, a new 
environment  `conversations` becomes available, and the above can be 
typeset as:
```latex
\begin{examples}
    \item(godot) 
    [A country road. A tree.]
    \begin{conversation}
        \item[Estragon:] Nothing to be done.
        \item[Vladimir:] I am beginning to come round to that opinion. All my 
        life I've tried to put it from me, saying Vladimir, be reasonable, you haven't yet tried everything. And I resumed the struggle.

        So you are here again. 
        \item[Estragon:] Am I? 
    \end{conversation}
\end{examples}
```
Configuration options are described in 
[the file README-conversations.md](README-conversations.md)

## Glosses

Currently, the package does not provide its own support for aligned 
glosses. However, you can use `cgloss4e.sty` from 
[gb4e](https://www.ctan.org/tex-archive/macros/latex/contrib/gb4e) together with `example-sentences`.
Then you can use the `\gll` and `\glll` macros as usual:
```latex
\begin{examples}
    \item \gll Das ist kein Beispielsatz.\\
               This is no   {example sentence}\\
          \trans `This is not an example sentence.`
\end{examples}
```

**Note:** As of this writing, there is a problem with diacritic marks. 
A workaround is described 
[below under "Known Issues"](#diacritics-with-cgloss4e). Alternatively, 
you can use 
[Alexis Dimitriadis's `cgloss.sty`](http://www.let.uu.nl/~Alexis.Dimitriadis/personal/latex/cgloss.sty), 
which is a hacked version of `cgloss4e` that prevents the problem.

## Customization

The `examples` environment is highly configurable. It is simply a clone
of the standard `enumerate` environment, created by `enumitem`. As a 
result, all of the configuration options of that package are available.

For example, if you want to add a bit more space between example number 
for one particular example list (e.g., to accommodate and extra-long 
diacritic), you can simply pass configuration options to the example 
environment:

```latex
\begin{examples}[leftmargin=2.5\parindent]
    \item<*****> Sentence very a bad is.
\end{examples}
```

If you want to *globally* change how lists look, you can use 
`enumitem`'s `\setlist` command. Note that this command *replaces* the 
list configuration, rather than overwriting individual parameters. 
Hence, the following won't quite work, because it deletes the settings 
for how example numbers should be formatted, and so forth:
```latex
\setlist[examples,1]{leftmargin=2.5\parindent}
```
To make it easier to overwrite single settings `example-sentences` adds 
configuration options that hold the default values. So instead, write:
```latex
\setlist[examples,1]{ex1defaults,leftmargin=2.5\parindent}
```
`ex1defaults` holds the default settings for examples of level 1, 
`ex2defaults` and `ex3defaults` those for nested example lists. 
`exdefaults` holds the defaults for all lists.

For details about the many configuration options, see 
[the documentation of `enumitem`](http://mirrors.ctan.org/macros/latex/contrib/enumitem/enumitem.pdf).

## Package Options

### Compatibility options: Turning of convenience commands

One of the aims of this package is to be compatible with as many LaTeX 
packages as possible. This has limits, obviously, whenever one of the 
convenience commands are defined (or modified) by another package. For 
that reason, the convenience commands can be turned off individually, 
using package options.

#### Leaving `\item` alone: the `normalitem` option

To provide the extended behavior for `\item` described above, LaTeX's 
own implementation of this command needs to be overwritten (this happens
only in example lists, however). If you use any other packages that
modify this command (`beamer` comes to mind), there could be conflicts 
and other problems.

For this reason, the loading the package with the option `normalitem`  
(i.e., `\usepackage[normalitem]{example-sentences}`) tells the package 
to not touch the `\item` command. In this case, the command `\exitem` 
can be used in place of `\item`, which has all of the advanced behavior
of `\item` described above.

#### Turning off the short `\ex` reference command with the `noex` option

Since `\ex` is so short, it is not unlikely that other packages define a
command with this name. In this case, you might want to prevent
`example-sentences` from defining `\ex` as short way to create references
to examples, and use the longer name `\exref` instead (which always works).
```latex
\usepackage[noex]{example-sentences}
```
#### For the ascets: The `compat` option

If you pass the `compat` option to `example-sentences`, it will define the
new `examples`-environment, but do nothing else: No any enhancements to 
`\item`, no reference commands besides the standard `\ref`, not even `\exitem`.

This is mainly intended for cases where there are unexpected incompatibilities
with other packages, but it may be useful also in case you want to define your
own convenience commands.

### The `enumitemize` option

Standardly, `example-sentences` loads the `enumitem` package with the 
`loadonly` option, which ensures that the standard LaTeX lists 
(`enumerate`, `itemize` and `description`) are not enhanced by 
`enumitem`.

If you want to change this, pass the `enumitemize` option to 
`example-sentences`.

### Other options

All options besides the one described above will be passed through to 
`enumitem`.

## Dependencies

Unless you (La)TeX installation is truly ancient, you should be able to
use `example-sentences` without further ado. For the record, here is a
list of the required packages:

- [enumitem](https://www.ctan.org/pkg/enumitem) is the only required 
   dependency.
- The `xparse` package (available as part of
  [l3packages](https://www.ctan.org/pkg/l3packages)) is not strictly
  required, but the 
  [enhancements for the `\item` command described above](#syntactic-sugar-some-enhancements-for-item)
  will only be available if `xparse` is present in your LaTeX-installation.

## Compatibility with `beamer`

`beamer` is a powerful package for typesetting slide-style 
presentations. As is the case with many packages, a few things have to 
be kept in mind when using `example-sentences` with `beamer`.

### `beamer`'s `examples`-environment

By default, `beamer` defines an environment called `examples` and, 
which typeset their contents in a `block` with the heading 
"Examples". `example-sentences` **overwrites** this environment.

To use the beamer-style `examples`-environment when `example-sentences` 
is loaded,  you can use the alias `beamerexamples`:

```latex
\begin{beamerexamples}
  This is a example in the beamer-style.
\end{beamerexamples}
```

### Using `beamerarticle.sty`

`beamer` provides a style-file `beamerarticle.sty`, which enables the 
use  of various beamer commands in documents that are typeset with other
document classes, like `article`. This makes it easier to share code 
between beamer slidesand an article or handout.

To make sure that `example-sentences` plays nicely with 
`beamerarticle.sty`, you have two options:

1. (recommended) Load `beamerarticle.sty` **before** `example-sentences.sty`. Then everything will 
    work as usual.
2. Load `beamerarticle.sty` with the `notheorems` option, like so: 
   `\usepackage[notheorems]{beamerarticle}`.

   This means you have to manually declare any theorem-style 
   environments that you want to use. However, `beamer` will still apply 
   its enhancements (e.g., boxes) to such manually-defined environments.


## Known issues
### Beginning an example with [...] or (...)

Many linguists (semanticists and pragmaticists in particular) like to 
start their examples with a description of the context, enclosed in 
square brackets [...] or, more rarely, parenthes (...).  This is fine, 
but you have to take extra care if you are using `\item` without any 
kind of argument (as you also have to do with `enumerate` lists).

The problem is that, in the following, 
`[This is a description of the context]` will be interpreted as the 
optional argument of `\item` (since LaTeX ignores spaces before the 
first argument).
```latex
\begin{examples}
  \item [This is a description of the context]\\
        This is the example.
\end{examples}
```

The recommendes way to avoid this problem is to give all your examples
a label via the 
[convenient `\item(...)` command](assigning-labels-with-item).
The following will work without problems:
```latex
\begin{examples}
  \item(mylabel) [This a description of the context]\\
        This is the example.
\end{examples}
```

Alternatively, simply put a pair of braces around the context
description:
```latex
\begin{examples}
  \item {[This is a description of the context]}\\
        This is the example.
\end{examples}
```
### Diacritics with `cgloss4e`

As of this writing, `example-sentences` does not correctly typeset 
diacritics when used together with `cgloss4e.sty`. The best
workaround is to use
use [Alexis Dimitriadis's `cgloss.sty`](http://www.let.uu.nl/~Alexis.Dimitriadis/personal/latex/cgloss.sty) 
in place of `cgloss4e`.

Alternatively, include the diacritic manually in the glossed sentence, like so:
```latex
\begin{examples}
  \item 
        \gll  \diacritic{*} Ungrammatisch dieser ist Satz.\\
               {}           Ungrammatical this   is  sentence\\
        \trans (intended) `This sentence is ungrammatical.'
\end{examples}
```
With this, the diacritic will be typeset correctly (i.e., set in the
space between example and number). I am currently working on a better 
way around this problem.

### Footnotes

It is custom to number examples in footnotes differently from the main 
text: Level one examples have lowercase roman numerals as labels, with 
counting starting anew for each footnote.

This is difficult to achieve in the general case. `example-sentences` 
patches the standard LaTeX footnote commands (i.e., `\footnote` and 
`footnotetext`), for all other methods of typesetting notes, you will 
have to ensure yourself that the command `\footnotizeexamples` is called
at the beginning of each note (or before the first example) and 
`\unfootnotizeexamples` is called at the  end of each footnote (or after
the last example).

*Note on implementation*: The example environments used in footnotes is
in fact entirely different from the main `examples` (it is just mostly 
configured in  the same way). If you want to modify the appearance of 
examples in footnotes, you'll need to use `\setlist[fnexamples]{}` /
 `\setlist[fnexamples,1]{}`, etc.
Make sure that you do not use the 
`ex1defaults`/`ex2defaults`/`ex3defaults` keywords in this case, as this
will reuse the main counters. Instead, use
`fnex1defaults`/`fnex2defaults`/`fnex3defaults`.
