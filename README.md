# example_sentences
A LaTeX package for (better) linguistic examples

## Motivation

Linguists (and scholars in related fields) use a lot of example sentences in
their writing. There is a fairly accepted standard for format for presenting 
those: Example sentences are numbered continuously throughout a document, and
optionally can have subexamples, as in (2) below.

![An example of an example](http://www.sven-lauer.net/files/examples/simple_example.png?new)

The platonic ideal of a LaTeX package for typesetting such examples whould 
define an environment that behaves as much as possible as LaTeX's
built-in `enumerate` lists (because, at heart, example lists *are* just ordered
lists). So the above examples would be typeset as:

```latex
\begin{examples}
  \item This is an example.
  \item \begin{examples}
          \item This a sub-example.
          \item This is another sub-example.
        \end{examples}
\end{examples}
```

The existing packages that I know of fall short of this ideal in several ways.
While [linguex.sty](https://www.ctan.org/tex-archive/macros/latex/contrib/linguex?lang=en)
has a very concise syntax that very readable (which is why I favored
it for a long time), it betrays its TeX roots through various quirks. For
example, it has very particular  whitespace requirements, which can make 
working with it a hassle. The same true for 
[expex.sty](https://www.ctan.org/pkg/expex?lang=en), which also
features a rather exotic syntax that I've always found too difficult to remember. 
And then there is [gb4e.sty](https://www.ctan.org/pkg/gb4e?lang=en), which is 
more LaTeXy then the other ones, but which has a number of strange 
inconsistencies.

There is an old style file called `examples.sty` (written by Alexander Holt at 
the University of Edinburgh) floating around that provides
something closer to the ideal, but it has a number of features that I disliked,
so I decided to create my own.

## Dependencies

- [enumitem](https://www.ctan.org/pkg/enumitem) is the only required dependency.

  (In fact, the main part of `example_sentences` is just a paper-thin wrapper 
  around `enumitem`'s functionality.]
- The `xparse` package (available as part of
  [l3packages](https://www.ctan.org/pkg/l3packages)) is not required, but if
  it is present, it is used to provide some nice syntactic sugar for the `\item` 
  commands.
  
Both of these are available in CTAN and should be part of most 
recent-ish TeX installations, as far as I know.

## Installation

As usual, simply place `example_sentences.sty` some place where TeX can find it
(the directory where your tex files live will work if all else fails).

## Usage

Load in the usual way, by placing the following in the preamble of your
document:
```latex
\usepackage{example_sentences}
```

There are some package options, which will be described later.

### Zero learning curve

If you know how to use the `enumerate` environment LaTeX provides, you already
know how to use `example_sentences`. All that changes for basic usage is that
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

And, as usual, you can give an optional argument to `\item` to manually specify
the label (e.g., if you are quoting an example, and want to use the original
example number):

```latex
\begin{examples}
    \item[(23)] This example will be numbered (23).
\end{examples}
```

In fact, this is almost all you need to know in order to produce anything that
`example_sentences` can produce. The only additional thing to know is how to
typeset diacritic marks indicating acceptability and such. You can do this 
explicitly by using the `\diacritic{}` command:
```latex
\begin{examples}
    \item \diacritic{*} Bad sentence this sounds.
\end{examples}
```
Diacritics are typeset in the space between example number and example. So the
above will render as:

![Example with diacritic](http://www.sven-lauer.net/files/examples/diacritic-example.png)

However, explicit use of the `\diacritic{}` command is discouraged. Instead,
you should use (if possible) the convenience macros described below.

### Syntactic sugar: Enhancements

If you have `xparse.sty` available, which is contained in 
[l3packages](https://www.ctan.org/pkg/l3packages), a number of enhancements
of the basic `\item` command are available.

#### Diacritics with `\item<>`

All variants of `\item` (including the ones described below) allow for an 
optional argument in angle brackets containing a diacritic/acceptability mark:

```latex
\begin{examples}
    \item<*> Bad sentence this sounds.
\end{examples}
```

Renders as

![Example with diacritic](http://www.sven-lauer.net/files/examples/diacritic-example.png)

If available, this style is preferred over the explicit use of `\diacritic{}`.

**Note:** Writing this documentation, it occurs to me that this style will
likely wreak havoc when `example_sentences` is used with the `beamer` package
to produce slide shows. So there is a chance this will change in the future.

#### Assigning labels with `\item()`

The basic `\label{}` command works as it always does. However, a more convenient
way to assign labels to examples is provided. Simply supply the desired `label`
in parentheses after `\item`:
```latex
\begin{examples}
    \item(harlem) If you want to go to Harlem, you have to take the A train.
\end{examples}
```
This is equivalent to:
```latex
\begin{examples}
    \item\label{harlem} If you want to go to Harlem, you have to take the A train.
\end{examples}
```

#### Cross-references with `\item{}`

Sometimes it is convenient to give one example the same number as a previous
one (for example, if the second example is a repetition of earlier one). I've
never missed this functionality with `linguex.sty`, but some people seem to 
like it. With `example_sentences`, you can do this by providing the `label`
of the previous example in curly braces.

So, a couple of pages after the previous example, you could use:
```latex
\begin{examples}
    \item{harlem} If you want to go to Harlem, you have to take the A train.
\end{examples}
```
And the result would be a repetition of the previous example number. In fact,
`\item{harlm}` is just a synonym for `\item[(\ref{harlem})]`. Consequently,
sub-example-references will work as expected.

**Note:** As a reader, I find this style confusing. Often, it is better to number 
repeated examples consecutively, and indicate that the
example is repeated in another fashion. 

### References to examples

The standard `\ref{}` command words for examples. However, it produces the
(sub)example number without enclosing parentheses. This means that it would
have to be used in running text with explicit parentheses: 
```latex
(\ref{harlem}) is curious in that ...
```
rather than
```latex
\ref{harlem} is curious in that ...
```
This is by design, because it is sometimes necessary to access the example 
identifier itself. An example is if one wants to reference a range of 
sub-examples, as in:
```latex
(\ref{imperative}a-f) show the varied uses of imperatives.
```
However, the package provides a handy shortcut for example references. Instead of 
writing `(\ref{harlem})`, you can simply write:
```latex
\ex{harlem} is curious in that ...
```
And, as an added nicety, you can provide arbitrary material that is appended to
the example label as an optional argument:
```latex
\ex{imperative}[a-f] show the varied uses of imperatives.
```
## Package Options

### Short form commands with `shortform`

If you cannot live without typing `\ex` in example environment, you can load
the package as `\usepackage[shortform][example_sentences]`. This enables the
short names `exe` (for the environment) and `\ex` (for the `\item` command).
So the initial example can then be rewritten as:

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
`\ex` supports all the niceties of the `\item` command (provided `xparse` is
available), so all of `\ex<*>`, `\ex(label)<\#>`, `\ex{label}<?>` and 
`\ex[(2)]<*>` are valid.

For obvious reasons, the reference convenience command `\ex` will not be 
available in within example environments in `shortform` mode (it continues
to work outside of the environment). You can use its alias `\exref` instead.