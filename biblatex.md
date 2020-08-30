# Introduction to BibLaTeX
The package `biblatex` is currently the most powerful tool
for managing citations and reference lists in LaTeX.

## Contents
* [The basics](#the-basics)
  * [Compilation](#compilation)
* [BibLaTeX vs. BibTeX](#biblatex-vs-bibtex)
* [Filling the `.bib` file](#filling-the-bib-file)
  * [MathSciNet](#mathscinet)
  * [arXiv](#arxiv)
  * [Oria](#oria)
  * [Google Scholar](#google-scholar)
  * [Journal sites](#journal-sites)
* [Citation notes](#citation-notes)
  * [Notes in optional arguments](notes-in-optional-arguments)
* [Citation commands](#citation-commands)
* [Cite multiple sources](#cite-multiple-sources)
* [Styles](#styles)
* [Sorting schemes](#sorting-schemes)
* [Shorthand](#shorthand)
* [Further customization](#further-customization)
* [Showkeys](#showkeys)
* [Crediting without BibLaTeX](#crediting-without-biblatex)
* [Further reading](#further-reading)

## The basics
In addition to the `.tex` file,
you will need a `.bib` file.
The `.bib` file is your personal database
of metadata for sources that you might cite at some point.

Say you create a file `bibliography.bib`.
The content of `bibliography.bib` looks something like this:
```latex
@article
{
    key1,
    author = {...},
    title  = {...},
    ...
}

@book
{
    key2,
    author = {...},
    title  = {...},
    ...
}
```

The entry for a source starts with `@type`.
The type determines how the entry is formatted in the reference list.
Next in the entry is the citation key.
The citation key is similar to a label key;
it is used internally to refer to the specific entry
and it does not appear in the final `.pdf`.
The citation keys can be whatever you want them to be,
but it is wise to name them systematically.
A recommended naming scheme is the [alphabetic style](#styles).
Following the citation key is a number of optional metadata fields.
The field names are not case sensitive,
so `author = {...}`, `Author = {...}` and `AUTHOR = {...}` are all valid.

In the `.tex` file,
you must specify the name of the `.bib` file using `\addbibresource`.
Then you can cite an entry in the `.bib` file using `\cite{key}`.
The macro `\printbibliography` prints a reference list
containing only the entries that have been cited.
The `.tex` file now looks something like this:
```latex
\documentclass{memoir}

\usepackage[backend = biber]{biblatex}
\addbibresource{bibliography.bib}

\begin{document}

    Some text and a citation \cite{key1}.
    More text and a new citation \cite{key2}.

    \printbibliography

\end{document}
```

### Compilation
The `backend` option passed to `biblatex`
specifies the external program used to compile the reference list.
There are three possible values for the backend:
* `biber`, which is written for `biblatex`,
* `bibtex`, which is written for the older package `bibtex`,
* `bibtex8`, which is an 8 bit reimplementation of `bibtex`.

Run the backend between two ordinary compilations.
If you want the reference list to appear in the table of contents,
compile twice after running the backend.

**Compilation sequence:**
```latex
pdflatex filename.tex
biber filename
pdflatex filename.tex
```

## BibLaTeX vs. BibTeX
The next section is about gathering metadata from databases.
Often,
these databases has an "export to `bibtex`" button,
not "export to `biblatex`".
The package `bibtex` is an older bibliography management tool
that is still used by many.
Both `bibtex` and `biblatex` read `.bib` files,
so the metadata exported from the databases
can be used out of the box for `biblatex` as well.

There are many advantages to `biblatex` over `bibtex`.
For instance,
`biblatex` supports UTF-8,
it has a reference type `@online` (alias `@wwww`) for web pages,
and it is easier to customize.
Moreover,
`biblatex` handles multiple reference lists in the same document
and automatic language translation with `babel`.
This is possible with `bibtex`,
but requires additional packages.

A major advantage to `bibtex`over `biblatex`
is that its use is currently more widespread.

## Filling the `.bib` file
While you can enter the metadata for each entry in the `.bib` file manually,
it is easier to copy it from a database.
The metadata from databases sometimes contain errors,
so you should always proofread the reference list.
Below we showcase five sources of metadata.
The first two sources are great mathematical subjects,
and they have the most trustworthy metadata.
The next two are general sources,
but the metadata contain errors more frequently.
Finally,
we end with an example of a journal site that exports metadata.
See the [subject pages](https://www.ub.uio.no/english/subjects/naturalscience-technology/)
at the University of Oslo Library for more databases.

You can copy the metadata directly from one of the databases
and paste it into your `.bib` file.
It is a good idea to change to automatically generated citation key
into something that you will remember.

### MathSciNet
Once you have selected the desired entry in [MathSciNet](http://ams.org/mathscinet),
click "select alternative format" and then "BibTeX".
Sometimes the metadata from MathSciNet contain undefined macros
for special characters.
These macros are defined by `\usepackage{mathscinet}`.

![Image of MathSciNet](https://i.imgur.com/58OwS6Q.png)
![Image of MathSciNet](https://i.imgur.com/wqtVhWy.png)

### arXiv
Once you have selected the desired entry on [arXiv](https://arxiv.org),
there are two ways to export metadata.
One exports with `@misc` and the other with `@article`.
The type `@article` is intended for published papers,
so it is not suitable for the preprints on arXiv.
It is therefore better to use the `@misc` version,
or change the type to `@online`.

For `@misc`,
click on "Export citation".

![Image of "Export citation"](https://i.imgur.com/XaSjKxT.png)

For `@article`,
click on "NASA ADS" and "Export Citation".

![Image of "NASA ADS"](https://i.imgur.com/IgJKGBo.png)
![Image of "NASA ADS"](https://i.imgur.com/1ELg87e.png)

### Oria
Once you have selected the desired entry in [Oria](http://oria.no),
click "Export BibTeX".

![Image of Oria](https://i.imgur.com/O9sfFzF.png)

### Google Scholar
Once you have selected the desired entry in [Google Scholar](https://scholar.google.com),
click the quotation marks and then "BibTeX".

![Image of Google Scholar](https://i.imgur.com/8DBtWJr.png)

![Image of Google Scholar](https://i.imgur.com/YxB9vDB.png)

### Journal sites
If you are lucky,
the journal offers export capabilities.
We illustrate this with [ScienceDirect](https://www.sciencedirect.com) by Elsevier.
Once you have selected the desired entry,
click "Export" and then "Export citation to BibTeX".

![Image of ScienceDirect](https://i.imgur.com/jYER32T.png)

![Image of ScienceDirect](https://i.imgur.com/07DZVLX.png)

## Citation notes
The `\cite` command accepts two optional arguments,
called prenote and postnotes.
These are small remarks added before and after the citation,
respectively.
Typically,
the postnote is used to specify which part of the source
you are citing,
either by giving the relevant page, section or theorem number.
The syntax is as follows:
```latex
\cite[postnote]{key1}           % Only postnote
\cite[prenote][postnote]{key2}  % Both pre- and postnote
\cite[prenote][]{key3}          % Only prenote
```

If the postnote contains only a number, a list of numbers or a range of numbers
– either in Arabic or Roman numerals – 
`biblatex` assumes that the postnote specifies page numbers
and inserts "p." and "pp." as needed.

### Notes in optional arguments
The optional argument in a theorem-like environment
can be used to specify the origin of the result:
```latex
\begin{theorem}[\cite{key}]
    ...
\end{theorem}
```

However,
if you add a postnote to the citation,
you get a compilation error:
```latex
\begin{theorem}[\cite[Theorem~1.2]{key}]
    ...
\end{theorem}
```
The solution to this error is to **add braces {} around the entire citation**:
```latex
\begin{theorem}[{\cite[Theorem~1.2]{key}}]
    ...
\end{theorem}
```

## Citation commands
The follow commands work the same way as `\cite`,
but with different output:
* `\parencite` – Cite in parentheses.
* `\footcite` – Cite in footnote.
* `\authorcite` – Cite only author.
* `\titlecite` – Cite only title.
* `\yearcite` – Cite only year.
* `\urlcite` – Cite only URL.


## Cite multiple sources
You can cite multiple sources at once
by separating the citation keys with comma:
```latex
\cite{key1, key2, key3}
```

You can ensure that the multiple citations
are printed in the same order as they appear in the bibliography
by adding `sortcites = true` to `\usepackage{biblatex}`.

The downside of
```latex
\cite{key1, key2, key3}
```
is that you cannot add individual pre- and postnotes
to the different sources.
If you need individual pre- and postnotes,
use:
```latex
\cites[prenote][postnote]{key1}[prenote][postnote]{key2}
```
The `\cites` command is not affected by `sortcites = true`.

## Styles
You can change the appearance of citations and the reference list
via the optional argument `style`:
```latex
\usepackage[style = alphabetic]{biblatex}
```

Below are some built-in values for `style`
and an example of how a citation looks:
* `numeric` – [1]
* `alphabetic` – [Har77]
* `authoryear` – Hartshorne 1977
* `authortitle` – Hartshorne, Algebraic geometry

More styles can be found hidden [here](https://ctan.org/topic/biblatex).

## Sorting schemes
You can change the order in which the reference list is sorted
via the optional argument `sorting`:
```latex
\usepackage[sorting = nty]{biblatex}
```

Below are different values for `sorting`:
* `nty` – Sort by name, title, year.
* `nyt` – Sort by name, year, title.
* `nyvt` – Sort by name, year, volume, title.
* `anyt` – Sort by alphabetic label, name, year, title.
* `anyvt` – Sort by alphabetic label, name, year, volume, title.
* `ynt` – Sort by year, name, title.
* `ydnt` – Sort by year (descending), name, title.
* `none` – Do not sort at all. All entries are processed in citation order.

## Shorthand
Sometimes you may want to override the citation style,
presumably to make it easier for the reader to recognize the source.
For instance,
*Éléments de géométrie algébrique* by Alexandre Grothendieck
is well-known among algebraic geometers as EGA.
Hence a reader may recognize the source without checking the reference list
if the citation reads "[EGA]" rather than "[Gro67]".
Likewise,
when you are citing software,
it is more likely that the reader will recognize the software name
rather than a citation based on the authors' names.
To overrule the citation style,
specify an alternative citation
using the `shorthand` field in the `.bib` entry:
```latex
@misc
{
    M2,
    shorthand    = {Macaulay2},
    author       = {Grayson, Daniel R.
                    and
                    Stillman, Michael E.},
    title        = {Macaulay2},
    howpublished = {Available at \url{http://www.math.uiuc.edu/Macaulay2/}}
}
```

## Further customization
You can remove DOIs, ISBNs and URLs from the reference list
by passing `doi = false`, `isbn = false` and `url = false` to `biblatex`.
Note that `url = false` does not remove the URL
from sources with the entry type `@online`.

The package option `giveninits = true` ensures that initials are used
for all given names in the reference list.

You can specify how many author names are printed
before they are replaced with "et al." in a citation
with `maxcitenames = n`.
Likewise,
use `maxbibnames = m` to specify the number of author names
in the reference list.

Add the following lines to the preamble
to print surnames before given names in the reference list:
```latex
\DeclareNameAlias{sortname}{family-given}
\DeclareNameAlias{default}{family-given}
```

## Showkeys
The package `showkeys` displays cite keys (and label keys)
in the margin of the `.pdf`.
This allows you to find a key
– if you have already used it –
without searching through the `.bib` file.

![Image of showkeys](https://i.imgur.com/aDkmN4F.png)

In addition to printing the labels next to the reference list,
`showkeys` prints the label above each citation and cross reference.
If you find this to be excessive,
disable the latter features using the package options `notcite` and `notref`.

Once you are done writing,
disable `showkeys` with the document class option `final`.

## Crediting without BibLaTeX
Mathematicians have adopted the convention
to join the names of authors with shared credit
using an **endash** (–).
For instance,
this is used in the "Navier–Stokes equations" and "Cauchy–Schwarz inquality".
The purpose is to distinguish between multiple authors and authors with hyphenated names.
Following the convention,
it is clear that the "Birch–Swinnerton-Dyer conjecture" is credited to two people,
Birch and Swinnerton-Dyer.

Use double hyphens `--` to write endash:
```LaTeX
Navier--Stokes
Cauchy--Schwarz
Birch--Swinnerton-Dyer
```

## Further reading
Sorted by length.
* Clea Rees: [BibLaTeX Cheat Sheet](http://tug.ctan.org/info/biblatex-cheatsheet/biblatex-cheatsheet.pdf)
* Knut Hegna: [BibLaTeX – course notes](http://www.ub.uio.no/fag/informatikk-matematikk/informatikk/kursmateriell/biblatex/biblatexbooklet.pdf)
* Dag Langmyhr and Knut Hegna: [Local guide to BibLaTeX](http://dag.at.ifi.uio.no/latex-links/biblatex-guide.pdf)
* Philip Kime, Moritz Wemheuer and Philipp Lehman: [BibLaTeX manual](http://mirrors.ctan.org/macros/latex/contrib/biblatex/doc/biblatex.pdf)
