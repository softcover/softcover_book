# Introduction to Markdown
\label{cha:introduction_to_markdown}

As noted in Chapter~\ref{cha:getting_started}, the default input format for Softcover is *Markdown*, a lightweight markup language designed to be human-friendly and easily convertible to HTML.[^markdown_origins] Unfortunately, by itself Markdown is a little *too* light\-weight, and the original "vanilla" Markdown is generally inadequate for producing professionally typeset documents. (For example, vanilla Markdown is unable to produce numbered footnotes.[^footnote_example]) Softcover therefore supports a *superset* of vanilla Markdown, including select [*kramdown*](http://kramdown.gettalong.org/) extensions, GitHub-style [*fenced code blocks*](https://help.github.com/articles/github-flavored-markdown#fenced-code-blocks), and embedded \LaTeX\@. The Softcover dialect of Markdown is, to our knowledge, the most powerful one available, with support for figures, tables, code listings, and mathematical equations (all with numbered, linked cross-references).

The rest of this chapter includes a quick tutorial on vanilla Markdown. Readers who already know vanilla Markdown can skip to Chapter~\ref{cha:softcover_flavored_markdown} for coverage of the custom Softcover extensions.

Being productive in Markdown requires only a subset of the full language, and the following material is an opinionated survet of Mardown's most useful features. For a comprehensive treatment of Markdown syntax, see the [syntax page](http://daringfireball.net/projects/markdown/syntax) by John Gruber (Markdown's principal creator).

## Headings

At the highest level, Markdown documents are structured like HTML, with a convenient syntax for defining the equivalent of HTML headings (`h1`, `h2`, etc.). There are actually several equivalent syntaxes, but my favorite is simply to use the pound character `#`:

```text
# Top-level heading (h1)

Lorem ipsum

## Second-level heading (h2)

Dolor sit amet

### Third level (h3)

Consectetur

#### Fourth (h4)

dipisicing elit

```

\noindent Softcover currently supports headings down to `####` (`h4`, corresponding to a "subsubsection" in \LaTeX).

## Text formatting

Markdown supports *italicized* text using _two_ different formats (asterisks or underscores):

```text
Markdown supports *italicized* text using _two_ different formats
```

\noindent It also supports **boldface** via double asterisks:

```text
It also supports **boldface** via double asterisks
```

\noindent The two formats can be nested using a combination of asterisks and underscores, yielding _**boldface italic**_:

```text
yielding _**boldface italic**_
```

### Blockquotes
\label{sec:blockquotes}

*Blockquotes* are supported using right angle brackets, which is inspired by the format of quoted replies in plain-text email clients:

> Il semble que la perfection soit atteinte non
> quand il n'y a plus rien à ajouter,
> mais quand il n'y a plus rien à retrancher.
>
> —Antoine de Saint-Exupéry, *Terre des hommes*

\noindent This quote[^quote_translation] is produced by the code in Listing~\ref{code:blockquote}.

\begin{codelisting}
\label{code:blockquote}
\codecaption{Typesetting a blockquote. Compare with Listing~\ref{code:latex_quote}.}
```text
> Il semble que la perfection soit atteinte non
> quand il n'y a plus rien à ajouter,
> mais quand il n'y a plus rien à retrancher.
> —Antoine de Saint-Exupéry, *Terre des hommes*
```
\end{codelisting}


\noindent Note the use of the Unicode [em dash](http://en.wikipedia.org/wiki/Dash#Em_dash) '—'; Softcover (but not vanilla Markdown) also supports \LaTeX-style triple dashes, with \verb+---+ being set as '---' (Section~\ref{sec:embedded_latex}).

I generally find Markdown's style of blockquote syntax fine when an email program automatically puts in the `>` brackets, but it's cumbersome to put them in by hand. Good text editors can make constructing blockquotes easier, but it still involves more friction than I'd like. I think \LaTeX's syntax is nicer (Section~\ref{sec:embedded_latex_commands}), especially since it can more easily be produced by a text-editor macro or tab trigger, but the default syntax may be more familiar.

### Source code

Markdown can also format source code and other verbatim text. Backticks indicate inline code, as in the `def` keyword:

```text
as in the `def` keyword
```

\noindent Code blocks can be set using four spaces of indentation, with


    def hello
      puts "hello, world!"
    end

\noindent being produced by

```text
    def hello
      puts "hello, world!"
    end
```

\noindent Note that this is *not* my preferred method for including source code, and I strongly encourage using GitHub-style code fencing (Section~\ref{sec:code_fencing}) or \LaTeX\ code environments (Section~\ref{sec:latex_code_formatting}) instead.


## Links and images
\label{sec:links_and_images}

Markdown supports hypertext links through a convenient format inspired by common usage in email, where you might write something like this as:

```text
Check out the Ruby on Rails Tutorial (http://railstutorial.org/)
```

\noindent Markdown adds one piece of syntax to resolve the ambiguity of exactly which text corresponds to the link, thus letting you check out the [Ruby on Rails Tutorial](http://railstutorial.org/) as follows:

```text
check out the [Ruby on Rails Tutorial](http://railstutorial.org/)
```

Images follow a similar syntax, with the text being preceded by an exclamation point. This allows you to embed images like so:

![Michael Hartl](images/2011_michael_hartl.png)

```text
This allows you to embed images like so:

![Michael Hartl](images/2011_michael_hartl.png)
```

\noindent Due to the details of how Softcover processes Markdown, unfortunately the bracketed text does *not* get used as the image alt text; instead, the filename (minus extension) gets used:[^latex_images]

```html
![Michael Hartl](images/2011_michael_hartl.png) ->
  <img alt="2011_michael_hartl" src="images/2011_michael_hartl.png" />
```

\noindent As a result, it's a good idea to use meaningful filenames for images for the sake of those using screen readers or other nonstandard browsers.

Images in vanilla Markdown are limited to embedding as above, but Softcover extends Markdown to provide a wide variety of other behavior, including captioned images, numbered figures, and numbered figures with captions (Section~\ref{sec:embedded_figures}).

### PDF/PNG images
\label{sec:pdf_png_images}

Because PDF and HTML treat images differently, sometimes it's useful to be able to include PDF images in PDF documents and PNG images in HTML/EPUB/MOBI. Softcover supports this automatically via a simple convention: if you include an image with a `.pdf` extension, the corresponding `.png` file will be used in the HTML version. For example, this Model-View-Controller image from the Ruby on Rails Tutorial:

![Model View Controller](images/figures/mvc_schematic.pdf)

\noindent is produced with the code

```text
![Model View Controller](images/figures/mvc_schematic.pdf)
```

\noindent which uses `mvc_schematic.pdf` in the PDF book and `mvc_schematic.png` in the HTML output. The only requirement is that both files exist in the correct location.


## Lists

Markdown supports both numbered and unnumbered lists, corresponding to HTML `ol` and `ul` environments, respectively.

### Numbered lists

Numbered lists are simple:

1. Foo
2. Baz
3. Quux

```text
Numbered lists are simple:

1. Foo
2. Baz
3. Quux
```

\noindent One counterintuitive aspect of numbered lists is that the numbering need not be sequential; instead, the numbering is handled automatically by the HTML `<ol>` tag. This means that the following list is effectively the same as the one above:

3. Foo
1. Baz
2. Quux

```text
This means that the following list is effectively the same as the one above:

3. Foo
1. Bar
2. Quux
```
 \noindent Though potentially confusing, this behavior is nice when you need to insert an element into the list but you don't want to have to renumber all the other elements by hand:

 1. Foo
 2. Bar
 2. Baz
 3. Quux

```text
you don't want to have to renumber all the other elements by hand:

1. Foo
2. Bar
2. Baz
3. Quux
```

### Unnumbered lists

Unnumbered lists are even easier than numbered lists:

* Foo
* Bar
* Baz

```text
Unnumbered lists are even easier than numbered lists:

* Foo
* Bar
* Baz
```

\noindent You can alternately uses minuses (or even pluses) instead of asterisks:

- One fish
- Two fish
- Red fish
- Blue fish

```text
You can alternately uses minuses (or even pluses) instead of asterisks:

- One fish
- Two fish
- Red fish
- Blue fish
```

Mixing different bullet-point styles is particularly nice when making nested lists:

* Foo
    - Bar
    - Baz
* Quux

```text
nice when making nested lists:

* Foo
    - Bar
    - Baz
* Quux
```

### Paragraphs in lists

In the case of both numbered and unnumbered lists, you can put a paragraph in a list element by indenting the desired paragraphs four spaces, like so:

1. Foo

    Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod
    tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam,
    quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo
    consequat.

1. Baz

1. Bar

    Duis aute irure dolor in reprehenderit in voluptate velit esse
    cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non
    proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

```text
you can put a paragraph in a list element by indenting the desired paragraphs
four spaces, like so:

1. Foo

    Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod
    tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam,
    quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo
    consequat.

1. Baz

1. Bar

    Duis aute irure dolor in reprehenderit in voluptate velit esse
    cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non
    proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
```

<!-- footnotes  -->

[^markdown_origins]: <http://daringfireball.net/2004/03/dive_into_markdown>

[^footnote_example]: Like this.

[^quote_translation]: Usually translated as "Perfection is achieved, not when there is nothing more to add, but when there is nothing left to take away."

[^latex_images]: The issue is that \LaTeX\ images have no notion of "alt text", so that information is lost in translation. (This is a slight disadvantage of converting Markdown to \LaTeX\ before converting to HTML, though the advantages more than compensate for it.)