# Remarks on common mistakes
This text presents the proper treatment of mistakes that are common among LaTeX novices.

## Paragraphs
A very common issue is the presence of vague paragraphs.  
They look like this.
The previous line ends before the margin,
but the next line is neither indented nor preceded by additional vertical space.
This effect is probably achieved using `\\`.
There should never be a need to end normal text with `\\`.

A new paragraph is initialised by the use of two line breaks in the source code:
```latex
This is the first paragraph.

This is the second paragraph.
```
A single line break in the source code is interpreted as a space:
```latex
This is the first paragraph.
This is also the first paragraph.
```
This is also true of display style mathematics environments:
```latex
This is the first paragraph.
\begin{align}
    ...
\end{align}

This is the second paragraph.
```

```latex
This is the first paragraph.
\begin{align}
    ...
\end{align}
This is also the first paragraph.
```

It is highly recommended that you keep the default settings
where the first line of a paragraph is indented.
Without the indentation,
it is ambiguous if the first line of a new page starts a paragraph or not.
If you absolutely want to start paragraphs without indentation,
we describe how to achieve it without `\\`.
The indentation at the beginning of a paragraph is governed by the length `\parindent`.
Similarly,
the space between two paragraphs is set by `\parskip`.
The following removes the indentation,
but adds more space between paragraphs:
```latex
\setlength{\parindent}{0pt}
\setlength{\parskip}{1.5ex plus 0.5ex minus 0.2ex}
```
Apart from in the document class `memoir`,
the two lines above can be replaced with the package `parskip`.
In `memoir`,
the latter line can be replaced by `\nonzeroparskip`.

## Margins
Changing the margins without the necessary typographical knowledge is ill-advised.
A lot of people think that the default margins in LaTeX are too large,
thus wasting too much of the paper.
However,
margin size is not a relevant parameter for evaluating the readability of a text.
What matters is the average number of characters per line, including spaces.
For optimum reading speed, the ideal number is 66 characters per line,
but anything in the 60–80 range is fine.
The default line width in LaTeX is optimised for the default font and font size.
If the number of characters per line is too large,
the text is perceived as a more difficult read.
The reader, who has as little typographical knowledge as the author,
will assume that this is due to bad writing.

It is perfectly acceptable to make the margins smaller if you compensate by making the font bigger.
In the document class `memoir`,
changing the font size will also change the margins accordingly.

## Citations
When restating a result from a source,
it is cleaner to add the citation as part of the optional argument of the `theorem` environment,
than it is to put the citation at the end of the statement or in the `proof` environment.
That is,
you should opt for
```latex
\begin{theorem}[{\cite[Theorem~1]{key}}]
    Statement.
\end{theorem}
```

rather than
```latex
\begin{theorem}
    Statement. \cite[Theorem~1]{key}
\end{theorem}
```

or
```latex
\begin{theorem}
    Statement.
\end{theorem}

\begin{proof}
    See \cite[Theorem~1]{key}.
\end{proof}
```

When citing a web page,
always specify when you visited the page.
This is done by adding the information field `urldate` to the `.bib` entry:
```latex
@online
{
    key,
    author  = {Optional information},
    editor  = {Optional information},
    title   = {...},
    date    = {...},
    url     = {...},
    urldate = {yyyy-mm-dd},
}
```

## Quotation marks
A repeat offender is the use of `"quotation marks"`.
However,
it should be `` `quotation marks´ ``, ` ``quotation marks´´ ` or `<<quotation marks>>`,
depending on the language.
Many standard LaTeX editors can be configured so that the symbols `"..."`
are automatically replaced by the corresponding set of `` `...´ ``, ` ``...´´ ` or `<<...>>`.

The TeXnical solution is to use the package `csquotes` together with `babel`.
This defines the command `\enquote`,
which is a nestable command that prints the correct quotation marks according to the specified language.
With the language option `british`,
the following
```latex
\enquote
{
    This is an
    \enquote{inner quote}
    inside an outer quote
}
```
produces ``` `This is an ``inner quote´´ inside an outer quote´ ```.
It is possible to define symbols that activate the same functionality as `\enquote`.
This is done either with
```latex
\MakeOuterQuote{<symbol 1>}
\MakeInnerQuote{<symbol 2>}
```
or
```latex
\MakeAutoQuote{<symbol 1>}{symbol 2>}
```

## Microtype
The modern compiler `pdflatex` serves two purposes:
First,
it outputs `.pdf`,
saving you a conversion from `.dvi`.
Second,
it extends the compiler `latex` with microtypographical abilities.
The interface for the microtypography is the package `microtype`.
The package makes subtle changes to the space between words.
The reader will not notice these changes without looking closely,
but the total effect is a reduction in the number of words that break across lines
and the number of occurrences of overfull hbox errors.
All documents should load `microtype`. 
Without `microtype`,
there is no real advantage in using `pdflatex` over `latex`.

## Display style mathematics
Display style environments for mathematics,
such as `\[...\]`, `equation`, `align` and `gather`,
are generally underused.
Display style need not be reserved for expressions you want to highlight or refer to later. 
It can also be used to add more space to a paragraph that is full of symbols,
making it a less dense read.
In particular,
this is the case if an expression or calculation covers more than half a line.
Moreover,
if an in-line mathematics expression is split across two lines or causes an overfull hbox warning,
then you should either try to restructure the text or change the in-line expression to a display style expression.
The golden rule about display style is that too much is better than not enough.

For display style – unlike in-line expressions – punctuation marks have to be placed inside the mathematics environment.
Writing
```latex
\begin{align}
    x + y = 3
\end{align}.
```
causes the full stop to appear on the first line after the equation.

## Token not allowed in a PDF string
This is a common error message issued when a cross reference, citation or complicated symbol
is placed inside a sectioning command like `\chapter{}` or `\section{}`.
The error is raised because the `.pdf` navigation menu
is not capable of displaying all symbols that are available to LaTeX.
The solution is to specify two strings,
a normal one for LaTeX and a simpler, hard-coded one for the navigation menu using `\texorpdfstring{TeX string}{PDF string}`.
Here is the same section header with and without `\texorpdfstring`:
```latex
\section{The \(\boldsymbol{f}\)-vector}

\section{The \texorpdfstring{\(\boldsymbol{f}\)}{f}-vector}
```
This makes a significant difference to the `.pdf` navigation menu:

![Examples of tokens not allowed in PDF string](https://i.imgur.com/I3l0Bdu.png)

## Colons
Inside a mathematics environment,
the colon character `:` on the keyboard is a *relation symbol*,
with even spacing to the left and right of the symbol.
For function definitions,
the colon should instead be used as a *punctuation mark*,
with more whitespace to the right of the symbol than to the left of it.
This is achieved by replacing `:` with `\colon`.
Some people like to rename this macro:
```latex
\newcommand{\from}{\colon}
```

Renaming enables the seductive construction
```latex
\(f \from X \to Y\)
```

The colon character `:` is suitable,
for example,
as a set builder, as the index of a subgroup, or to separate homogeneous coordinates.

For definitions,
the colon should be vertically centred with respect to the equal sign.
Hence you should use the macros `\coloneqq` and `\eqqcolon` from the package `\mathtools`,
not `:=` and `=:`.

## Set builders
The vertical bar character `|` is not a relation symbol,
so it adds too little space to be used as a set builder or for conditional probabilities.
The correct command is `\mid`.
See the difference:

![Image of set builders](https://i.imgur.com/sovCbZj.png)

When the expressions on either side of the set builder involves many vertical bars,
such as |x|,
then it is better to use a colon as the set builder for distinction.

## Endash
The *endash* – is named so because it is as long as a lower case n is wide.
In LaTeX,
endash is accessed through the use of double hyphens `--`.
Ranges are typeset using an endash,
not a hyphen.
Thus the following is correct:
"Leonhard Euler (1707–1783)" and "see [1, pp. 23–27]",
but "Leonhard Euler (1707-1783)" and "see [1, pp. 23-27]" is not.

The mathematical community has adapted the practice of joining the names of people that share credit with an endash.
That way,
it is unambiguous whether or not it is two people or one person with a hyphenated name.
Hence it should be "Gauss–Jordan elimination",
not "Gauss-Jordan elimination".

## Ellipses
Typing three full stops `...` into the source code does not produce correctly spaced ellipses.
There is an array of specialised commands for various ellipses out there,
but lowered dots `\ldots` and centered dots `\cdots` suffice for most uses.

## Percentages
When trying to typeset "50%",
one discovers that
```latex
50\%
50 \%
```
adds too little or too much space between the number and the percent sign.
The package `siunitx` provides the command `\SI` for typesetting units.
Either of the lines below solve our spacing issue:
```latex
\SI{50}{\percent}
\SI{50}{\%}
```

## End of proof
Leaving a blank line at the end of the `proof` environment
causes the QED symbol to be placed a line below where it should:
```latex
\begin{proof}
    This line is the end of the proof.
    It should contain the QED symbol.
\end{proof}
```

![Image of correct QED placement](https://i.imgur.com/yDHnZd0.png)

```latex
\begin{proof}
    This line is the end of the proof.
    It should contain the QED symbol.

\end{proof}
```

![Image of incorrect QED placement](https://i.imgur.com/y5GXpBd.png)
