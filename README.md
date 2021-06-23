# The `example_sentences.sty` package for typesetting linguistic examples


Linguists (and scholars in related fields) use example sentences in 
their  writing. There is a fairly accepted standard format for 
presenting those: Example sentences are numbered continuously throughout 
a document, and optionally can have subexamples, as in (2) below.

![An example of an example](http://www.sven-lauer.net/files/examples/simple_example.png?new)

The platonic ideal of a LaTeX package for typesetting such examples 
would provide an environment that behaves as much as possible as LaTeX's
built-in `enumerate` lists (because, at heart, example lists *are* just
ordered lists). So the above examples would be typeset as:

```latex
\begin{examples}
  \item This is an example.
  \item \begin{examples}
          \item This a sub-example.
          \item This is another sub-example.
        \end{examples}
\end{examples}
```

The existing packages that I know of fall short of this ideal in several 
ways. 
While [linguex.sty](https://www.ctan.org/tex-archive/macros/latex/contrib/linguex?lang=en)
has a concise syntax that is very readable (which is why I favored it 
for a long time), it betrays its TeX roots through various quirks. For 
example, it has very  particular whitespace requirements, which can make
working with it a hassle. The same is true for 
[expex.sty](https://www.ctan.org/pkg/expex?lang=en), 
which also features a rather exotic syntax that I've always found too 
difficult to remember. And then there is 
[gb4e.sty](https://www.ctan.org/pkg/gb4e?lang=en), which is more LaTeXy
than the other ones, but which has a number of strange inconsistencies.

There is an old style file called `examples.sty` (written by Alexander 
Holt at the University of Edinburgh) floating around that provides 
something closer to the ideal, but it has a number of features that I 
disliked, so I decided to create my own.

- [Installation](#installation)
- [Basic Usage: Zero learning curve](#basic-usage-zero-learning-curve)
- [Syntactic sugar: Some enhancements for `\item`](#syntactic-sugar-some-enhancements-for-item)
- [Individual examples with `\begin{example} ... \end{example}`](#individual-examples-with-beginexample--endexample)
- [Referencing examples in the text: `\ref{...}`, `\ex{...}`, and `\exref{...}`](#referencing-examples-in-the-text-ref-ex-and-exref)
- [Typesetting dialogues with `\begin{conversation} ... \end{conversation}`](#typesetting-dialogues-with-beginconversation--endconversation)
- [Glosses](#glosses)
- [Dependencies](#dependencies)
- [Customization](#customization)
- [Package Options](#package-options)
- [Compatibility with `beamer`](#compatibility-with-beamer)
  + [`beamer`'s example(s)-environment](#beamers-examples-environment)
  + [Using `beamerarticle.sty`](#using-beamerarticlesty)
- [Known issues](#known-issues)
  + [Beginning an example with [...] or (...)](#beginning-an-example-with--or-)
  + [Overlays with `beamer`](#overlays-with-beamer)
  + [Diacritics with `cgloss4e`](#diacritics-with-cgloss4e)
  + [Footnotes](#footnotes)

## Installation

As usual, simply place `example_sentences.sty` some place where TeX can 
find it (the directory where your tex files live will work if all else 
fails).

Then you can load in the usual way, by placing the following in the 
preamble of your document:
```latex
\usepackage{example_sentences}
```

There are a number of [package options](#package-options), described 
below.

## Basic Usage: Zero learning curve

If you know how to use the `enumerate` environment LaTeX provides, you 
already know how to use `example_sentences`. All that changes is that
you use the `examples` environment instead of `enumerate`:

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
anything that `example_sentences` can produce. The only additional thing
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
unless your LaTeX installation is truly ancient. (You need to have 
`xparse.sty` available, which is contained in 
[l3packages](https://www.ctan.org/pkg/l3packages), and should be 
contained in most recent-ish LaTeX-Installations).

### Diacritics with `\item<>`

All variants of `\item` (including the ones described below) allow for 
an optional argument in angle brackets containing a 
diacritic/acceptability mark:

```latex
\begin{examples}
    \item<*> Bad sentence this sounds.
\end{examples}
```

This renders as

![Example with diacritic](http://www.sven-lauer.net/files/examples/diacritic-example.png)

If available, this style is preferred over the explicit use of 
`\diacritic{}`.

**Note:** Writing this documentation, it occurs to me that this style 
will likely wreak havoc when `example_sentences` is used with the 
`beamer` package to produce slide shows. So there is a chance this will 
change in the future.

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

## Individual examples with `\begin{example} ... \end{example}`

Some users (including me on certain days) will feel uncomfortable with 
the environment name `examples` in case there is only one `\item`. For 
those users, there is a synonym `example`, so you can write:
```latex
\begin{example}
    \item This is an example.
\end{example}
```
(You will still have to type `\item`, though. This may change in future 
releases.)

This is a true alias of the `examples` environment, so nothing prevents 
multiple examples in an `example` environment.

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
However, the package provides two handy shortcut for example references. 
Instead of writing `(\ref{harlem})`, you can simply write either:
```latex
The sentence in \exref{harlem} is curious in that ...
```
or:
```latex
The sentence in \ex{harlem} is curious in that ...
```

And, as an added nicety, you can provide arbitrary material that is 
appended to the example label as an optional argument:
```latex
\ex{imperatives}[a-f] show the varied uses of imperatives.
```
(The latter form will only work if `xparse.sty` is available. Otherwise, 
you can use `\ex[a-f]{imperatives}`, which will always work.)

## Typesetting dialogues with `\begin{conversation} ... \end{conversation}`

Semanticists and pragmaticists often need to typeset short dialogues, 
like this:

![Example of a conversation](http://www.sven-lauer.net/files/examples/example-conversation.png)

If you load `example_sentences` with the `conversations` option, a new 
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
By default, the width of the labels is determined by the widest label. 
Configuration options are described in 
[the file README-conversations.md](README-conversations.md)

## Glosses

Currently, the package does not provide its own support for aligned 
glosses. However, you can use `cgloss4e.sty` from 
[gb4e](https://www.ctan.org/tex-archive/macros/latex/contrib/gb4e) together with `example_sentences`.
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

## Dependencies

- [enumitem](https://www.ctan.org/pkg/enumitem) is the only required 
   dependency.

  (In fact, the main part of `example_sentences` is just a paper-thin wrapper 
  around `enumitem`'s functionality.]
- The `xparse` package (available as part of
  [l3packages](https://www.ctan.org/pkg/l3packages)) is not required, 
  but if it is present, it is used to provide some nice syntactic sugar 
  for the `\item` commands.
  
Both of these are available in CTAN and should be part of most 
recent-ish TeX installations, as far as I know.

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
To make it easier to overwrite single settings `example_sentences` adds 
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

### Short form commands with `shortform`

If you cannot live without typing `\ex` in example environments, you can
load the package as `\usepackage[shortform]{example_sentences}`. This 
enables the short names `exe` (for the environment) and `\ex` (for the 
`\item` command). So the initial example can then be rewritten as:

```latex
\begin{exe}
  \ex \begin{exe}
    \ex An example with sub-examples.
    \ex \begin{exe}
        \ex And sub-sub examples, like this one.
        \ex And a second one.
    \end{exe}
  \end{exe}
\end{exe}
```
`\ex` supports all the niceties of the `\item` command (provided 
`xparse` is available), so all of `\ex<*>`, `\ex(label)<\#>`,  and 
`\ex[(2)]<*>` are valid.

For obvious reasons, the reference convenience command `\ex` will not be 
work within example environments in `shortform` mode (it continues
to work outside of the environment). You can use its alias `\exref` 
instead.

The commands introduced by `shortform` only are synonyms for the longer 
commands, which remain in place. So long and short form commands can be
mixed freely.

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
(i.e., `\usepackage[normalitem]{example_sentences}`) tells the package 
to not touch the `\item` command. In this case, the command `\exitem` 
can be used in place of `\item`, which has all of the advanced behavior
of `\item` described above.

#### Turning off reference commands with the `noexref` and `noex` options

LaTeX offers various packages (such as 
[cleverref](https://www.ctan.org/pkg/cleveref?lang=en)) that improve on 
reference handling. If you use such a package, you likely have no use 
for the convenience commands `\exref` and `\ex` to produce reference to
examples in the running text. You can turn them off by loading the 
package as:
```latex
\usepackage[noexref,noex]{example_sentences}
```
Indeed, if you are using another reference package, it is recommended 
that you use this way of loading `example_sentences`, to avoid 
cluttering the LaTex namespace.

#### Turning off all enhancements: `noexitem` and `compat`

If, for some reason, even the `\exitem` command creates 
incompatibilities, you can turn off its functionality with the option 
`noexitem`.

This means the invocation
```latex
\usepackage[normalitem,noexref,noex,noexitem]{example_sentences}
```
would leave you with an `examples`-environment that behaves in all ways
like the standard `enumerate`-environment (besides being customizable 
via `enumitem`'s options). If you desire this, there is a shortcut, 
which also prevents some internal processing that might lead to 
incompatibilities:
```latex
\usepackage[compat]{example_sentences}
```
Note that `compat` implies `normalitem,noexref,noex,noexitem`.


### The `enumitemize` option

Standardly, `example_sentences` loads the `enumitem` package with the 
`loadonly` option, which ensures that the standard LaTeX lists 
(`enumerate`, `itemize` and `description`) are not enhanced by 
`enumitem`.

If you want to change this, pass the `enumitemize` option to 
`example_sentences`.

### Other options

All options besides the one described above will be passed through to 
`enumitem`.

## Compatibility with `beamer`

`beamer` is a powerful package for typesetting slide-style 
presentations. As is the case with many packages, a few things have to 
be kept in mind when using `example_sentences` with `beamer`.

### `beamer`'s `example(s)`-environment

By default, `beamer` defines two environments called `example` and 
`examples`, which typeset their contents in a block with the heading 
"Example(s)". `example_sentences` **overwrites** these environments.

To use the beamer-style example-environments when `example_sentences` 
is loaded,  you can use the aliases `beamerexample` and 
`beamerexamples`:

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

To make sure that `example_sentences` plays nicely with 
`beamerarticle.sty`, you have two options:

1. (recommended) Load `beamerarticle.sty` **before** `example_sentences.sty`. Then everything will 
    work as usual.
2. Load `beamerarticle.sty` with the `notheorems` option, like so: 
   `\usepackage[notheorems]{beamerarticle}`.

   This means you have to manually declare any theorem-style 
   environments that you want to use. However, `beamer` will still apply 
   its enhancements (e.g., boxes) to such manually-defined environments.


## Known issues
### Beginning an example with [...] or (...)

Many linguists like to start their examples with a description of the 
context, enclosed in square brackets [...] or, more rarely, parenthes 
(...).  This is fine, but you have to take extra care if you are using 
`\item` without any kind of argument (as you also have to do with 
`enumerate` lists).

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

To prevent this behavior, simply put a pair of braces around the context
description:
```latex
\begin{examples}
  \item {[This is a description of the context]}\\
        This is the example.
\end{examples}
```

Alternatively (recommended) make sure to give all your examples a label 
via the [convenient `\item(...)` command](assigning-labels-with-item). 
The following will work without problems:
```latex
\begin{examples}
  \item(mylabel) [This a description of the context]\\
        This is the example.
\end{examples}
```

### Overlays with `beamer`
There currently is an issue with overlay
specifications in the `beamer` package: Since overlays cause a `frame` 
to be typeset more than once, example numbers will be increased with 
each overlay.

The only work-arounds at present are (i) avoiding overlays or (ii) 
specifying manual labels via `\item[(label)]`.

This problem will likely be fixed in the next release by making 
`example_sentences` overlay-aware.

(Thanks to Shane Steinert-Threlkeld for pointing out this issue.)


### Diacritics with `cgloss4e`

In version 0.5.0, this package does not correctly typeset diacritics 
when used together with `cgloss4e.sty`. The only workaround is either to
use [Alexis Dimitriadis's `cgloss.sty`](http://www.let.uu.nl/~Alexis.Dimitriadis/personal/latex/cgloss.sty) in place of `cgloss4e`, or to include the diacritic manually
in the glossed sentence, like so:
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

This is difficult to achieve in the general case. `example_sentences` 
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