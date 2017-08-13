# example_sentences
A LaTeX package for (better) linguistic examples

## Motivation

Linguists (and scholars in related fields) use a lot of example sentences in
their writing. There is a fairly accepted standard for format for presenting 
those: Example sentences are numbered continuously throughout a document, and
optionally can have subexamples, as in (2) below.

![An example of an example](http://www.sven-lauer.net/files/examples/simple_example.png)

The platonic ideal of a LaTeX package for typesetting such examples whould 
define an environment that behaves as much as possible as LaTeX's
built-in `enumerate` lists (because, at heart, example lists *are* just ordered
lists). So the above examples would be typeset as:

```latex
\begin{examples}
  \item This is an example.
  \item \begin{examples}
          \item This is the first sub-example.
          \item This is the second sub-example.
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