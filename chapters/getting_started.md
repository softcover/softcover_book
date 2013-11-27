# Getting started
\label{cha:getting_started}

This is [*The Softcover Book*](http://manual.softcover.io/book)---the manual for *Softcover*, a digital publishing platform for technical authors.[^online_version] Softcover consists of two principal parts: a state-of-the-art [open-source ebook typesetting system](https://github.com/softcover/softcover) (Section~\ref{sec:softcover_system}), and an [online platform](http://www.softcover.io/) for publishing, marketing, and selling ebooks and other digital goods (Section~\ref{sec:softcover_website}).

Details on the typesetting system, including CLI, and website integration.

This chapter goes through the default steps to generate a book, build ebooks, and upload them to the Softcover website. More details in subsequent chapters.

Box on uses (Box~\ref{aside:softcover_uses}).


\begin{aside}
\label{aside:softcover_uses}
\heading{How to use Softcover}

Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod
tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam,
quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo
consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse
cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non
proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

\end{aside}


Based on the technology and business model used in the [Ruby on Rails Tutorial](http://ruby.railstutorial.org/) by [Michael Hartl](http://www.michaelhartl.com/), Softcover can be used for many different purposes, but it is mainly designed to help authors make the transition from "writing a book" to "building a product empire".

Broad outline:

1. Make ebooks, screencasts, etc.
2. Upload to Softcover and assemble into different product bundles
3. Profit!!1!

\noindent Steps 2--3 are not strictly necessary, though, and even by itself the \softcover\  typesetting system is very useful.

**Note:** Softcover is currently in private beta. Visit [Softcover.io](http://www.softcover.io/) to request an invitation.

## The \softcover\ typesetting system
\label{sec:softcover_system}

In this section, we'll cover the basics of \softcover, an open-source ebook typesetting system for technical authors. Its [*raison d'\^{e}tre*](http://www.merriam-webster.com/dictionary/raison%20d'etre) is producing professional-grade multi-format ebooks from a common set of source files. In particular, \softcover\  accepts input in *Markdown*, a lightweight markup language, or *\PolyTeX*, an easy-to-learn subset of the powerful \LaTeX\ typesetting language, and outputs ebooks as HTML, EPUB, MOBI, and PDF. The \softcover\  system also comes with a local server that automatically rebuilds a book's HTML output when the source files change, so that your favorite text editor and web browser combine to form a real-time development environment for writing ebooks.[^ipad_trick] Finally, authors can use \softcover\ to upload ebooks and other media files to the [Softcover website](http://www.softcover.io/) with a single command, thereby dramatically lowering the barrier to publishing, updating, and selling digital information products.

Originally developed under the name *\PolyTeXnic*[^poly_pronunciation] to write the [*Ruby on Rails Tutorial* book](http://railstutorial.org/book) and [*The Tau Manifesto*](http://tauday.com/tau-manifesto), \softcover\  has been rewritten and expanded as part of developing the [Softcover publishing platform](http://www.softcover.io/). True to its origins, \softcover\  supports a wide range of features useful for writing technical books, including mathematical typesetting (Eq.~\eqref{eq:maxwell})[^maxwell] and syntax-highlighted source code listings (Listing~\ref{code:eval}). It is also well-suited to writing non-technical books, with support for chapters, sections, cross-references, footnotes, lists, figures, tables, etc.---the only requirement is that the *author* be technical. (If you know how to use a command line and have a favorite text editor, you are technical enough to use \softcover.)

\begin{equation}
\label{eq:maxwell}
\left.\begin{aligned}
\nabla\cdot\mathbf{E} & = \rho \\
\nabla\cdot\mathbf{B} & = 0 \\
\nabla\times\mathbf{E} & = -\dot{\mathbf{B}} \\
\nabla\times\mathbf{B} & = \mathbf{J} + \dot{\mathbf{E}}
\end{aligned}
\right\}
\quad\text{Maxwell's equations}
\end{equation}

\begin{codelisting}
\label{code:eval}
\codecaption{``\href{http://www.michaelnielsen.org/ddi/lisp-as-the-maxwells-equations-of-software/}{Maxwell's equations of software}'' in \href{https://en.wikipedia.org/wiki/Scheme_(programming_language)}{Scheme}.}
```scheme
;; Implements Lisp in Lisp.
;; Alan Kay called this feat "Maxwell's equations of software", because just
;; as Maxwell's equations contain all of electrodynamics, `eval` and `apply`
;; contain all of computing.

;; Evaluates an arbitrary Scheme S-expression.
(define (eval exp env)
  (cond ((self-evaluating? exp) exp)
        ((variable? exp) (lookup-variable-value exp env))
        ((quoted? exp) (text-of-quotation exp))
        ((assignment? exp) (eval-assignment exp env))
        ((definition? exp) (eval-definition exp env))
        ((if? exp) (eval-if exp env))
        ((lambda? exp)
         (make-procedure (lambda-parameters exp)
                         (lambda-body exp)
                         env))
        ((begin? exp)
         (eval-sequence (begin-actions exp) env))
        ((cond? exp) (eval (cond->if exp) env))
        ((application? exp)
         (apply (eval (operator exp) env)
                (list-of-values (operands exp) env)))
        (else
         (error "Unknown expression type -- EVAL" exp))))

;; Applies a Scheme procedure to arbitrary arguments.
(define (apply procedure arguments)
  (cond ((primitive-procedure? procedure)
         (apply-primitive-procedure procedure arguments))
        ((compound-procedure? procedure)
         (eval-sequence
           (procedure-body procedure)
           (extend-environment
             (procedure-parameters procedure)
             arguments
             (procedure-environment procedure))))
        (else
         (error
          "Unknown procedure type -- APPLY" procedure))))
```
\end{codelisting}

Naturally, *The Softcover Book* itself is written using \softcover. Indeed, you can consider this manual the *de facto* [spec](http://en.wikipedia.org/wiki/Specification_(technical_standard)) for the system: essentially everything that \softcover\  can do, this document does. Because the [source code](http://github.com/softcover/softcover_book) of *The Softcover Book* is available online, you can learn how to typeset anything you see in this manual simply by referring to the corresponding place in the source.

### Installing \softcover

The \softcover\ system is distributed as a Ruby gem under the permissive [MIT License](http://opensource.org/licenses/MIT). To get started with \softcover, first [install Ruby](http://ruby.railstutorial.org/ruby-on-rails-tutorial-book#sec-install_ruby) and [install RubyGems](http://ruby.railstutorial.org/ruby-on-rails-tutorial-book#sec-install_rubygems) if you don't have them already. Getting \softcover\ is then a simple `gem install`:

```console
$ gem install softcover
```

\noindent This installs the `softcover` command-line interface (CLI) for creating new books, building ebooks, and publishing ebooks and other digital assets to the [Softcover website](http://www.softcover.io/).

To see the commands supported by `softcover`, run `softcover help` at the command line (Listing~\ref{code:softcover_help}).

\begin{codelisting}
\label{code:softcover_help}
\codecaption{Viewing available \texttt{softcover} commands with \kode{softcover help}.}
```console
$ softcover help
softcover build, build:all           # Build all formats
softcover build:epub                 # Build EPUB
softcover build:html                 # Build HTML
softcover build:mobi                 # Build MOBI
softcover build:pdf                  # Build PDF
softcover build:preview              # Build book preview in all formats
softcover config                     # View local config
softcover config:add key=value       # Add to your local config vars
softcover config:remove key          # Remove key from local config vars
softcover deploy                     # Build & publish book
softcover epub:validate, epub:check  # Validate EPUB with epubcheck
softcover help [COMMAND]             # Describe available commands...
softcover login                      # Log into Softcover account
softcover logout                     # Log out of Softcover account
softcover new <name>                 # Generate new book directory structure
softcover open                       # Open book on Softcover website (OS X)
softcover publish                    # Publish your book on Softcover
softcover publish:screencasts        # Publish screencasts
softcover server                     # Run local server
softcover unpublish                  # Remove book from Softcover
softcover version                    # Return the version number
```
\end{codelisting}



\noindent For convenience, \softcover\ also installs a shorter alias, `sc`, so you can get, e.g, the version number by typing

```console
$ sc -v
```

### Creating a Softcover book

We see from Listing~\ref{code:softcover_help} that the way to generate a new Softcover book is with `softcover new <name>`. Let's try it out and see what happens (Listing~\ref{code:new_example_book}). (Authors who prefer \LaTeX\ to Markdown should generate a \PolyTeX\ book instead; see Chapter~\ref{cha:polytex_documents} to learn how.)

\begin{codelisting}
\label{code:new_example_book}
\codecaption{Generating an example book with \kode{softcover new}.}
```console
$ softcover new example_book
Generating directory: example_book
Creating chapters
Creating epub
Creating epub/OEBPS
Creating epub/OEBPS/styles
Creating html
Creating html/jquery
Creating html/jquery/1.10.2
Creating html/stylesheets
Creating images
Creating images/figures
Creating .softcover-deploy
Creating example_book.tex
Creating Book.txt
Creating book.yml
Creating chapters/a_chapter.md
Creating chapters/another_chapter.md
Creating chapters/preface.md
Creating chapters/yet_another_chapter.md.
.
.
.
Creating README.md
Creating softcover.sty
Creating upquote.sty
Done. Please update book.yml
```
\end{codelisting}

We see from Listing~\ref{code:new_example_book} that `softcover new` generates a bunch of files, but the heart of it is the `chapters/` directory, which contains the Markdown source for the template book:

```console
$ cd example_book
$ ls chapters/
a_chapter.md           preface.md
another_chapter.md     yet_another_chapter.md
```

\noindent The order of these chapters is set by the file `Book.txt` (Listing~\ref{code:book_txt}).

\begin{codelisting}
\label{code:book_txt}
\codecaption{The default contents of \kode{Book.txt}.}
<<(example_book/Book.txt, lang: text)
\end{codelisting}

\noindent


Softcover books also come with a `book.yml` file containing some book metadata (Listing~\ref{code:book_yml}).

\begin{codelisting}
\label{code:book_yml}
\codecaption{The default contents of \kode{book.yml}.}
<<(example_book/book.yml, lang: yaml)
\end{codelisting}


```yaml
---
title: softcover_book
slug: softcover_book
filename: softcover_book
subtitle: Change-me
cover: images/cover.png
description: Change me.
copyright: 2013
uuid: b2bfdd92-e5f1-4dc6-b7ce-4999e3870a12
pdf_preview_page_range: 1..30
epub_mobi_preview_chapter_range: 0..1
```

```yaml
---
title: The Softcover Book
slug: softcover_book
filename: softcover_book
subtitle: The manual for Softcover.io
cover: images/cover.png
description: Covers the softcover CLI and the Softcover.io website
copyright: 2013
uuid: b2bfdd92-e5f1-4dc6-b7ce-4999e3870a12
pdf_preview_page_range: 1..30
epub_mobi_preview_chapter_range: 0..1
```



The default book format generated by `softcover new` is *Markdown*---or, rather, a superset of Markdown that includes extensions to make Markdown more suitable for writing long documents. This includes the [kramdown](https://github.com/gettalong/kramdown) extensions (footnotes, tables, etc.)\ and GitHub-style code fencing. Authors who want more fine-grained control over their documents (or who already know \LaTeX) can use `softcover new -p` to generate a \PolyTeX\ template instead; see Chapter~\ref{cha:polytex} for details.


### Softcover server

```console
$ softcover server
```

for short:

```console
$ sc s
```

Insert figure showing two terminal windows/tabs


### Building ebooks

Requires some dependencies. You will be prompted.

#### PDF

Nicest-looking. Requires \LaTeX.

```console
$ softcover build:pdf
```

Uses `xelatex`, dumps a lot to the screen, option to make it quiet. Also runs twice to ensure all cross-references are defined, so option to make it only run once. Lots of options.

```console
$ softcover help build:pdf
Usage:
  softcover build:pdf

Options:
  -d, [--debug]          # Run raw xelatex for debugging purposes
  -o, [--once]           # Run PDF generator once (no xref update)
  -f, [--find-overfull]  # Find overfull hboxes
  -q, [--quiet]          # Quiet output
  -s, [--silent]         # Silent output

Build PDF
```

Be quiet:

```console
$ softcover build:pdf --quiet
```

or silent

```console
$ softcover build:pdf --silent
```

Run only once. Especially useful when there's something hanging. General tip: type `x`.

```console
$ softcover build:pdf -o
```

Debug option mainly useful for \PolyTeX\ books (Chapter~\ref{cha:polytex}).

```console
$ softcover build:pdf --debug
```

#### EPUB

EPUB is basically zipped HTML.

```console
$ softcover build:epub
```

If you install EpubCheck, you can validate according to the EPUB3 standard with


```console
$ softcover epub:validate
```

#### MOBI

MOBI is the native format for Kindle. Can use `kindlegen`, but violates ToS to sell it. Alternate is `ebook-convert`, part of the Calibre command-line tools.

```console
$ softcover build:mobi
```

#### All formats

once you've installed all the dependencies, you can build all formats at once:

```console
$ softcover build:all
```

#### Previews

Can build previews

```yaml
---
.
.
.
pdf_preview_page_range: 1..30
epub_mobi_preview_chapter_range: 0..1
```

## Publishing to the Softcover website

Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod
tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam,
quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo
consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse
cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non
proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

### Publishing ebooks

Create account

Log in

```console
$ softcover login
```

Publish to the live site

```console
$ softcover build:all
$ softcover build:prevew
$ softcover publish
```

Open it (OS X)

```console
$ softcover open
```

Do everything at once:

```console
$ softcover deploy
```

Equivalent to

```console
$ softcover build:all
$ softcover build:prevew
$ softcover publish
```

but can override by editing config file `.softcover-deploy` in your home directory (Listing~\ref{code:deploy_config}).


\begin{codelisting}
\label{code:deploy_config}
\codecaption{The Softcover deployment configuration file. \\ \filepath{\$HOME/.softcover-deploy}}
```text
# Edit this file to customize your deployment steps with custom command options
# or additional commands.
#
# softcover build:all
# softcover build:preview
# softcover publish
```
\end{codelisting}


Skip preview

```text
softcover build:all
softcover publish
```








### Markdown basics

Basic Markdown syntax, with embedded \LaTeX.


[^online_version]: This document is available online at <http://manual.softcover.io/book>.

[^ipad_trick]: My favorite trick is to connect an iPad to the \softcover\  server's address on the local network, effectively using the iPad as an external monitor. Then, when I save a source file, the iPad's browser magically refreshes with the updated content.

[^poly_pronunciation]: \PolyTeXnic\ is pronounced exactly like the English word *polytechnic*.

[^maxwell]: If already know [Maxwell's equations](http://en.wikipedia.org/wiki/Maxwell's_equations), the fundamental equations of electrodynamics, Eq.~\eqref{eq:maxwell} may look a little unfamiliar. This is because it writes the equations in a system called *rationalized [MKS units](http://en.wikipedia.org/wiki/MKS_system_of_units)*, which set \( \mu_0=\epsilon_0=1 \). In my view, these are the units in which Maxwell's equations are the most beautiful. Moreover, since the speed of light is \( 1/\sqrt{\mu_0\epsilon_0} \), this choice of units gives us \( c = 1 \) for free.
