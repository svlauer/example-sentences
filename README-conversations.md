# The `conversations.sty` package for typesetting dialogues

Semanticists and pragmaticists often need to typeset short dialogues, like this:

![Example of a conversation](http://www.sven-lauer.net/files/examples/example-conversation.png)

With the `conversations.sty` the above can be typeset as:

```latex
[A country road. A tree.]
\begin{conversation}
    \item[Estragon:] Nothing to be done.
    \item[Vladimir:] I am beginning to come round to that opinion. All my 
                     life I've tried to put it from me, saying Vladimir, be 
                     reasonable, you haven't yet tried everything. And I 
                     resumed the struggle.

                     So you are here again. 
    \item[Estragon:] Am I? 
\end{conversation}
```

`conversations.sty` is a companion to `example_sentences.sty` (and distributed
with it), but it can also be used on its own, or in conjunction with other packages for typesetting examples, like `linguex.sty` or `gb4e.sty`.

## Installation 

As usual, simply place `conversations.sty` some place where TeX can find it
(the directory where your tex files live will work if all else fails).

Then you can load in the usual way, by placing the following in the preamble
of your document:
```latex
\usepackage{conversations}
```
If you are using the package together with `example_sentences`, you can also use:
```latex
\usepackage[conversations]{example_sentences}
```
In this case, `conversations.sty` will be loaded automatically, and the enhancements of the `\item`-command (e.g., using `<*>` to typeset diacritics)
will be available for the `conversations` environment as well.

## Dependencies

- [enumitem](https://www.ctan.org/pkg/enumitem) 
- [environ](https://www.ctan.org/pkg/environ?lang=en)
  
Both of these are available in CTAN and should be part of most 
recent-ish TeX installations, as far as I know.

## Customatization

### Changing the appearance of the conversation lists

The `conversation` environment is (largely) a clone of LaTeX's own 
`description` environment, created by `enumitem`. It takes an optional
argument in which the usual `enumitem` options can be passed on a one-off
basis.

This is particularly useful if you want to adjust the space reserved by the 
labels. By default, this space is width required to typeset widest speaker 
label (plus [`\conversationindent`](#adjusting-spacing-in-the-default-mode)). However, if you have partiuclarly long
speaker labels, it often looks better to set a fixed width, and use 
`enumitem`'s `nextline`-style. This can be achieved as follows:

```latex
\begin{conversation}[leftmargin=2cm,style=nextline]
    \item[Question:] What would you think it is worth telling [future 
                     generations]  about the life you've lived, and the lessons you've learned from it?

        \item[Bertrand, Third Earl Russel:] 
                      I should like to say two things, one illectual, and one moral.
\end{conversation}
```

The result will look like this:

![Example of a conversation with the nextline style](http://www.sven-lauer.net/files/examples/example-conversation-russel.png)

If you want to **globally** set options for all (subsequent) conversation 
lists, use the command `\setconversationlist`. So the above settings could 
also be passed as:

```latex
\setconversationlist{leftmargin=2cm,style=nextline}
```

### Adjusting the appearance of the labels

Global setting of parameters will be particularly useful in case you want to 
change the appearance of the speaker labels. The standard seems to be to set 
them in *italics*, but I  think they often look nicer in **boldface**. I can 
achieve this by putting the following in the preamble of my document:

```latex
\setconversationlist{conversationdefaults,font={\normalfont\bfseries}}
```

Here, including `conversationdefaults` makes sure that I only overwrite the 
`font`-setting (which, again, is a standard `enumitem` setting for 
`description`-like lists).

**Note:** You cannot use the standard `\setlist` command from `enumitem` to
set the properties of conversation lists. You *have* to use 
`\setconversationlist`. But its argument can contain anything that `\setlist`
understands.

### Adjusting spacing in the default mode

As mentioned above, by default, `conversation.sty` determines the width of
the widest speaker label, and sets up the spacing accordingly. Sometimes, you 
want that functionality, but you require some extra spacing (e.g., to typeset
a diacritic in the space between speaker label and the text).

For this, the package provides the length `\conversationindent` (default: 
0.5em) that is added. You may find it more convenient to change this length
instead of those provided by `enumitem`:
```latex
\begin{examples}
  \setlength{\conversationindent}{1em} 
  \item \begin{conversation}
        \item[Estragon:]<*> Be to nothing done.  
    \end{conversation}  
\end{examples}
```

If you do this like above, within an `examples`-environment, the change will only be active within this environment. If you want to change it globally, 
simply move the length declaration to the top-level (or prefix it with 
`\global`).