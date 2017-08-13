# example_sentences
A LaTeX package for (better) linguistic examples

## Motivation

Linguists (and scholars in related fields) use a lot of example sentences in
their writing. There is a fairly accepted standard for format for presenting 
those: Example sentences are numbered continuously throughout a document, and
optionally can have subexamples, as in (2).

(1) This is an example.

(2) a. This is the first sub-example.
    b. This is the second sub-example.

There exist a number of (La)Tex packages for typesetting such examples, but
each has a number of drawbacks: 
[linguex.sty](https://www.ctan.org/tex-archive/macros/latex/contrib/linguex?lang=en)
has a very concise syntax that is almost WYSIWYG, which is why I favored
it for a long time. But it betrays its TeX roots, and has very particular 
whitespace requirements, which can make working with it a hassle. The
same true for [expex.sty](https://www.ctan.org/pkg/expex?lang=en), which also
features a rather exotic syntax that I find difficult to remember. And then
there is [gb4e.sty](https://www.ctan.org/pkg/gb4e?lang=en), which is more
LaTeXy then the other ones, but which has a number of strange inconsistencies.

To my mind, he platonic ideal of a linguistic examples package for LaTeX should 
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

There is an old style file called `example.sty` floating around that provides
something like that, but it has various features I didn't like. So I made my
own.

## Dependencies

- `example_sentences` depends only on the 
  [enumitem]{https://www.ctan.org/pkg/enumitem} package.[^ In fact, the main
  part of `example_sentences` is just a paper-thin wrapper around `enumitem`.]
- The `xparse` package (available in 
  [l3packages](https://www.ctan.org/pkg/l3packages)) is not required, but if
  it is present, it is used to provide some nice syntactic sugar for the `\item` 
  commands.