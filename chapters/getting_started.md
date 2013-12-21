# Getting started
\label{cha:getting_started}


<!--
    **Note:** Softcover is currently in private beta. Visit [Softcover.io](http://www.softcover.io/) to request an invitation.
-->

This is [*The Softcover Book*](http://manual.softcover.io/book)---the manual for *Softcover*, a publishing platform for technical authors. Softcover consists of two main parts: a state-of-the-art [open-source ebook typesetting system](https://github.com/softcover/softcover) (Section~\ref{sec:softcover_system}), and an [online platform](http://www.softcover.io/) for publishing, marketing, and selling ebooks and other digital goods (Section~\ref{sec:softcover_website}). Based on the technology and business model used by the [Ruby on Rails Tutorial](http://ruby.railstutorial.org/) by [Michael Hartl](http://www.michaelhartl.com/), Softcover is animated by the philosophy of *full author ownership*---own your content, own your production toolchain, own your traffic, own your customer list:

* **Own your content**: Authors retain copyright on all materials.
* **Own your production toolchain**: The Softcover typesetting system is 100% open-source, so you aren't locked into a proprietary toolchain.
* **Own your traffic**: Softcover supports custom domains.
* **Own your customer list**: Authors get all relevant contact information and never have to use an intermediary to communicate with their customers.

\noindent In short, Softcover is a publishing platform I would be happy to use even if I weren't one of the founders of the company. (As Softcover moves forward with its private beta, we'll determine the details of author compensation, but our plan is to pay royalties in the 90% range.)

A principal design goal of Softcover is to help authors make the transition from "writing a book" to "building a product empire," using the following [three-step plan](http://www.youtube.com/watch?v=tO5sxLapAts):

1. Make ebooks, screencasts, etc.
2. Release a free HTML book (*optional*) while selling ebooks and multiple product bundles
3. Profit!!1!one

\noindent Of course, it's not necessary to follow the Three Step Plan\texttrademark\ exactly, and Softcover can be used for many different purposes (Box~\ref{aside:softcover_uses}). In particular, as indicated by the "optional" note in Step 2, adopting the [*Ruby on Rails Tutorial* book](http://railstutorial.org/book)'s practice of releasing a free HTML version is not required. Softcover authors are certainly encouraged to make the HTML versions of their books free---both as a marketing tool and because it's [awesome](http://breadpig.com/products/awesomesauce)---but Softcover allows the HTML to be placed behind login- or paywalls as well.

\begin{aside}
\label{aside:softcover_uses}
\heading{How to use Softcover}

\noindent Softcover is designed as a flexible tool, so it has many potential uses. The first use represents Softcover's principal design goal, but the others are valid possibilities as well:


* **Make a free HTML book, bundle ebooks with other digital goods, and sell them from the [Softcover.io](http://softcover.io/) online storefront**
* Charge for all products (including the HTML book) at Softcover.io
* Produce ebooks with the \softcover\ typesetting system and give them away
* Produce ebooks with \softcover\ and sell them using the [Softcover.io](http://softcover.io/) online storefront
* Produce ebooks with \softcover\ and sell them from your own website
* Use \softcover\ to make ebooks out of technical documentation and host them on an internal website
* **(planned)** Sell *any* digital goods, not just ebooks made with \softcover

\end{aside}

In the rest of this chapter, we'll cover the steps needed to install and use the \softcover\ typesetting system, which includes a command-line interface for building and publishing ebooks. Subsequent chapters cover Markdown, the default input format for \softcover\ (Chapter~\ref{cha:markdown_tutorial}), and \PolyTeX, a more complicated but more powerful input format based on the \LaTeX\ typesetting language (Chapter~\ref{cha:polytex_tutorial}). (Additional chapters are in preparation as well.)

## The \softcover\ typesetting system
\label{sec:softcover_system}

In this section, we'll cover the basics of \softcover, an open-source ebook typesetting system for technical authors. Its [*raison d'\^{e}tre*](http://www.merriam-webster.com/dictionary/raison d'etre) is producing profes\-sional-grade multi-format ebooks from a common set of source files. In particular, \softcover\ accepts input in *Markdown*, a lightweight markup language, or *\PolyTeX*, a subset of the powerful \LaTeX\ typesetting language, and outputs ebooks as HTML, EPUB, MOBI, and PDF. The \softcover\  system also comes with a local server that automatically rebuilds a book's HTML output when the source files change, so that your favorite text editor and web browser combine to form a real-time development environment for writing ebooks. Finally, authors can use \softcover\ to upload ebooks and other media files to the [Softcover website](http://www.softcover.io/) with a single command (Section~\ref{sec:softcover_website}), thereby dramatically lowering the barrier to publishing, updating, and selling digital information products.

Originally developed under the name *\PolyTeXnic*[^poly_pronunciation] to write the [*Ruby on Rails Tutorial* book](http://railstutorial.org/book) and [*The Tau Manifesto*](http://tauday.com/tau-manifesto), \softcover\  has been rewritten and expanded as part of developing the [Softcover publishing platform](http://www.softcover.io/). True to its origins, \softcover\ supports a wide range of features useful for writing technical books, including mathematical typesetting (Eq.~\eqref{eq:maxwell})[^maxwell] and syntax-highlighted source code listings (Listing~\ref{code:eval}). It is also well-suited to writing non-technical books, with support for chapters, sections, cross-references, footnotes, lists, figures, tables, etc.---the only requirement is that the *author* be technical. (If you know how to use a command line and have a favorite text editor, you are technical enough to use \softcover.)

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
;; Alan Kay called this feat "Maxwell's equations of software", because just as
;; Maxwell's equations contain all of electrodynamics, `eval` and `apply`
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

Naturally, *The Softcover Book* itself is written using \softcover. Indeed, you can consider this manual the *de facto* [spec](http://en.wikipedia.org/wiki/Specification_(technical_standard)) for the system: essentially everything that \softcover\ can do, this document does. Because the [source code of *The Softcover Book*](http://github.com/softcover/softcover_book) is available online, you can learn how to typeset anything you see in this manual simply by referring to the corresponding place in the source.

### Installing \softcover

The \softcover\ system is open-source software, distributed as a Ruby gem under the permissive [MIT License](http://opensource.org/licenses/MIT). The `softcover` gem currently works with OS X and Linux, and we're looking for people to help us adapt it to other OSes. Join the [Softcover Google Group](https://groups.google.com/forum/#!forum/softcover-publishing) to be part of that effort.

To get started with \softcover, first [install Ruby](http://ruby.railstutorial.org/ruby-on-rails-tutorial-book#sec-install_ruby) (1.9.3 or later) and [install RubyGems](http://ruby.railstutorial.org/ruby-on-rails-tutorial-book#sec-install_rubygems) if you don't have them already. Once you've done so, getting \softcover\ is a simple `gem install`:

```console
$ gem install softcover
```

\noindent (On some systems, you may need to run `sudo gem install softcover` instead.) This installs the `softcover` command-line interface (CLI) for creating new books, building ebooks, and publishing ebooks and other digital assets to the [Softcover website](http://www.softcover.io/). On some systems, you may have to install extra libraries; for example, on Ubuntu I needed to install `ruby1.9.1-dev` to get the `nokogiri` gem to install.

To build the full set of output formats, \softcover\ requires some external dependencies, which you will be prompted to install at the appropriate time. For example, when you try to build a PDF, you'll be prompted to install \LaTeX. (That said, I recommend [starting to download LaTeX](http://latex-project.org/ftp.html) now, as the download is rather large. Type `which xelatex` at the command line to see if you already have \LaTeX\ installed.) These dependencies are also covered in the sections below, and Section~\ref{sec:all_dependencies} lists all the dependencies in one place in case you want to install them all at the same time.

To see the commands supported by `softcover`, run `softcover help` at the command line, as shown in Listing~\ref{code:softcover_help}.

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
softcover open                       # Open book on Softcover website
softcover publish                    # Publish your book on Softcover
softcover publish:screencasts        # Publish screencasts
softcover server                     # Run local server
softcover unpublish                  # Remove book from Softcover
softcover version                    # Return the version number (-v for short)
```
\end{codelisting}

\noindent For convenience, \softcover\ also installs a shorter alias, `sc`, so you can, e.g., get the current version number by typing any of the following:

```console
$ softcover version
$ softcover -v
$ sc version
$ sc -v
```

### Creating a Softcover book
\label{sec:softcover_new}

We see from Listing~\ref{code:softcover_help} that the way to generate a new Softcover book is with `softcover new <name>`. Let's try it out and see what happens---the results are shown in Listing~\ref{code:new_example_book}.

\begin{codelisting}
\label{code:new_example_book}
\codecaption{Generating an example book with \texttt{softcover new example\_book}.}
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

The default book format generated by `softcover new` is *Markdown*---or, rather, a superset of Markdown that includes extensions to make it more suitable for writing longer documents. This includes the [kramdown](https://github.com/gettalong/kramdown) extensions (numbered footnotes, tables, etc.),  GitHub-style code fencing, and embedded \LaTeX\  (Chapter~\ref{cha:markdown_tutorial}). Authors who want more fine-grained control over their documents (or who already know \LaTeX) can use `softcover new -p <name>` to generate a \PolyTeX\ template instead; see Chapter~\ref{cha:polytex_tutorial} for details.

We see from Listing~\ref{code:new_example_book} that `softcover new` generates a bunch of files, but the heart of it is the `chapters/` directory, which contains the Markdown source for the template book:

```console
$ cd example_book
$ ls chapters/
a_chapter.md           preface.md
another_chapter.md     yet_another_chapter.md
```

\noindent The names of the template form a progression ("A chapter", "Another chapter", "Yet *another* chapter"), but their order is not set by this progression. Rather, it is specified in the file `Book.txt` (Listing~\ref{code:book_txt}).

\begin{codelisting}
\label{code:book_txt}
\codecaption{The default contents of \texttt{Book.txt}.}
<<(example_book/Book.txt, lang: text)
\end{codelisting}

\noindent Authors are encouraged to use the template files as a model but to change their names so that they are better tailored to each book's content. For the purposes of this overview, though, we'll stick with the defaults.

Softcover books also come with a `book.yml` configuration file containing some book metadata (Listing~\ref{code:book_yml}). You should generally change the title, author, subtitle (if any), and description before publishing the book to Softcover (Section~\ref{sec:softcover_website}). The [slug](https://en.wikipedia.org/wiki/Clean_URL#Slug) represents the last part the book's URL at Softcover.io and should rarely need editing. See Section~\ref{sec:preparing_for_publication}[^undefined_xref] to learn how to put the finishing touches on before publication.

\begin{codelisting}
\label{code:book_yml}
\codecaption{The default contents of \texttt{book.yml}.}
<<(example_book/book.yml, lang: yaml)
\end{codelisting}

<!--
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
-->


### HTML and the Softcover server
\label{sec:html_softcover_server}

To get started writing the example book, we'll first build an HTML version:

```console
$ softcover build:html
```

\noindent The result is a separate HTML file for each chapter in the `html/` directory.

Let's take a look at the HTML for the first chapter (located in the file `html/a_chapter.html`) by opening it in a browser. On most systems, this can be accomplished by using a filesystem viewer to navigate to the `html/` directory and double-clicking on `a_chapter.html`. On Macintosh OS X, we can accomplish the same thing at the command line using the `open` command, which opens the given file using the default application for that file type:

```console
$ open html/a_chapter.html
```

\noindent (Linux users can get the same result with using `xdg-open` instead.) On my system, the default application for `.html` files is Chrome, so I get the result shown in Figure~\ref{fig:chrome_html}.

![Viewing the HTML file for the first chapter.\label{fig:chrome_html}](images/figures/chrome_html.png)

Although inspecting HTML files is sometimes useful for debugging purposes, the best way to develop Softcover books is to use the local Softcover server, which
detects when the source files have changed and automatically refreshes the browser.[^multiple_browsers] Let's open up a new terminal tab (Figure~\ref{fig:softcover_server}), navigate to the book directory, and fire up `softcover server`:[^sc_s]

```console
$ softcover server
Building...Done. (0.6s)
Running Softcover server on http://localhost:4000
== Sinatra/1.4.4 has taken the stage on 4000 for development with backup from
Thin
Thin web server (v1.6.1 codename Death Proof)
Maximum connections set to 1024
Listening on 0.0.0.0:4000, CTRL+C to stop
```

\noindent (You may have to install a [JavaScript runtime](https://github.com/sstephenson/execjs) if you don't have one installed already; I recommend [Node.js](http://nodejs.org/).) Opening a browser and navigating to <http://localhost:4000> then gives us a view of the HTML version of the first chapter of the book (Figure~\ref{fig:localhost_4000}).

![Running the Softcover server in a separate tab.\label{fig:softcover_server}](images/figures/softcover_server.png)

![Viewing the book on http://localhost:4000.\label{fig:localhost_4000}](images/figures/localhost_4000.png)

Writing books works just fine with a text editor and browser placed side-by-side (Figure~\ref{fig:editor_browser}), but my favorite trick is to connect an iPad to the Softcover server's address on the local network, effectively using the iPad as an external monitor. Then, when I save a source file, the iPad's browser magically refreshes with the updated content. This setup is especially nice for people (like me) who often work remotely and prefer for their production setup to be fully portable.[^ipad_address]

For now, I suggest playing around with the server a little to get the hang of it, and then quickly move on to building the various ebook formats (Section~\ref{sec:building_ebooks}). We'll have more to say about writing a book using the Softcover server in Chapter~\ref{cha:markdown_tutorial} and Chapter~\ref{cha:polytex_tutorial}.

![Writing with the editor and browser side-by-side.\label{fig:editor_browser}](images/figures/editor_browser.png)

### Building ebooks
\label{sec:building_ebooks}

As noted in Section~\ref{sec:softcover_system}, the \softcover\ system outputs HTML, EPUB, MO\-BI, and PDF. We saw in Section~\ref{sec:html_softcover_server} that HTML generation comes bundled with the \softcover\ gem; since EPUB is basically zipped HTML, EPUB generation comes for free as well. On the other hand, generating MOBI and PDF books requires installing some external dependencies, as does generating EPUB books if they contain mathematics. The `softcover` command will automatically prompt you to install the relevant software when the time comes; for example, if you try to build a PDF on a system without \LaTeX, you'll get this prompt with a link to the \LaTeX\ installation page:

```console
$ softcover build:pdf
Building PDF...
Document not built due to missing dependency
Install LaTeX (http://latex-project.org/ftp.html)
```

#### EPUB

EPUB books are essentially HTML combined with CSS and various configuration files, all zipped together in one package. (The easiest way to see this is to change an EPUB file's extension from `.epub` to `.zip` and double-click it.) Getting all the details just right is a real pain, though, and luckily Softcover does it for you.

There are no dependencies for building EPUB books unless the source contains mathematics, so if you want you can remove the math from the generated book and build the EPUB immediately. Otherwise, you'll need to install [PhantomJS](http://phantomjs.org/) and [Inkscape](http://inkscape.org/). Building the EPUB is easy once any necessary dependencies are installed:

```console
$ softcover build:epub
```

\noindent By default, the output of `build:epub` is verbose, but you can pass command-line options to make it quiet (`-q`) or silent (`-s`).

The generated EPUB book is located in the `ebooks` directory:

```console
$ ls ebooks/
example_book.epub
```

\noindent If you don't already have an EPUB viewer installed on your computer, I suggest [Adobe Digital Editions](http://www.adobe.com/products/digital-editions.html). On OS X, if Adobe Digital Editions is associated with `.epub` files, you can open the example book EPUB like this:

```console
$ open ebooks/example_book.epub
```

\noindent The result appears in Figure~\ref{fig:example_epub}. (*Note*: As of OS X Mavericks, you can also use iBooks to open EPUB files.)

![The example book EPUB.\label{fig:example_epub}](images/figures/example_epub.png)


#### MOBI

Once you've built an EPUB book, making a MOBI (the native format for Amazon.com's Kindle) is easy. One method is to use [kindlegen](http://www.amazon.com/gp/feature.html?ie=UTF8&docId=1000765211), supplied by Amazon.com itself, but selling the resulting MOBI anywhere other than Amazon.com violates (!)\ the [kindlegen terms of use](http://www.amazon.com/gp/feature.html?docId=1000599251). To my knowledge, Amazon has never enforced this provision, but authors should be aware of the risk. Luckily, there is an open-source alternative called *Calibre*, so I recommend you [install Calibre](http://calibre-ebook.com/) and then follow the instructions to [enable Calibre command line tools](http://manual.calibre-ebook.com/cli/cli-index.html). This gets you the `ebook-convert` command, and this is what \softcover\ uses by default to build MOBI files:

```console
$ softcover build:mobi
```

\noindent As with EPUB, you can use command-line options to make the MOBI builder quiet (`-q`) or silent (`-s`).

To view the MOBI file on your computer, I recommend installing [Kindle reader](http://www.amazon.com/gp/feature.html?docId=1000493771). The result appears in Figure~\ref{fig:example_mobi}.

![The example book MOBI.\label{fig:example_mobi}](images/figures/example_mobi.png)

If you're not planning to sell your book's MOBI file, or if you're willing to risk violating Amazon's terms of use, you can optionally use `softcover` with `kindlegen` instead:

```console
$ softcover build:mobi --kindlegen
```

#### PDF

Although the EPUB and MOBI ebooks formats are increasingly popular, my preferred ebook format (especially for technical books) is PDF. Building PDF books requires [installing LaTeX](http://latex-project.org/ftp.html) (specifically, the `xelatex` executable, which is a Unicode-friendly PDF builder). \LaTeX\ is a large download, but it's easy to install, and in fact you may already have it:

```console
$ which xelatex
```

\noindent If that command returns the path to `xelatex`, you can skip the installation step. In any case, building a PDF is easy once \LaTeX\ is installed:

```console
$ softcover build:pdf
```

\noindent The result appears in Figure~\ref{fig:example_pdf}.

![The example book PDF.\label{fig:example_pdf}](images/figures/example_pdf.png)

The `softcover build:pdf` command dumps a lot of output to the screen, and as with EPUB and MOBI you can use command-line options to make the PDF builder quiet (`-q`) or silent (`-s`), but I strongly recommend using the default verbose option unless you're *sure* the file will build without error. The issue is that `xelatex` will hang on \LaTeX\ syntax errors, and you need to be able to type `x` to exit. (**Remember this**: type `x` to exit when the PDF builder hangs.

By default, the PDF builder runs twice to ensure all cross-references are updated, but if the cross-references haven't changed (or if your book doesn't have any) you can pass an option to make it run only once:

```console
$ softcover build:pdf --once
```

\noindent This not only saves valuable time when building a longer book, but it is also useful when you're debugging a \LaTeX\ syntax error and you don't want to keep pressing `x` twice every time you run the command.


#### All formats

Once you've installed all the dependencies as above, you can build all formats at once:

```console
$ softcover build:all
```

\noindent On my system (an older MacBook Air), \softcover\ builds the template book's HTML, EPUB, MOBI, and PDF in under 15 seconds:

```console
$ time softcover build:all --silent

real    0m14.159s
user    0m12.086s
sys 0m1.185s
```

#### Previews

Finally, Softcover can optionally build book *previews*, which is a useful feature when selling your ebook (either on your own website or at [Softcover](http://softcover.io)). Because of the different ways the PDF and EPUB/MOBI formats work, there are two separate ways to specify the preview range. (You have to keep them roughly in sync by hand, but it's rarely important for the preview ranges to be exact, so this isn't a big problem in practice.) The configuration for PDF is a *page* range, while for EPUB & MOBI it's a *chapter* range (with Chapter 0 being frontmatter like the table of contents, preface, etc.). Both ranges are specified in `book.yml` (Listing~\ref{code:preview_ranges}).

\begin{codelisting}
\label{code:preview_ranges}
\codecaption{Specifying the preview ranges in \texttt{book.yml}.}
```yaml
---
.
.
.
pdf_preview_page_range: 1..30
epub_mobi_preview_chapter_range: 0..1
```
\end{codelisting}

The previews themselves are built as follows:

```console
$ softcover build:preview
```

The full Softcover publishing platform automatically uploads all the ebook files, including previews, and makes it simple to distribute them to your readers. We'll learn how to do this in the next section (Section~\ref{sec:softcover_website}).

### All dependencies
\label{sec:all_dependencies}

For reference, here are all the current dependencies for the Softcover system:

* PDF
    - [LaTeX](http://latex-project.org/ftp.html)
* EPUB/MOBI with math
    - [PhantomJS](http://phantomjs.org/)
    - [Inkscape](http://inkscape.org/)
* MOBI
    - [Calibre](http://calibre-ebook.com/) with [command-line tools](http://manual.calibre-ebook.com/cli/cli-index.html), or [kindlegen](http://www.amazon.com/gp/feature.html?ie=UTF8&docId=1000765211) (but beware the \linebreak [terms of use](http://www.amazon.com/gp/feature.html?docId=1000599251))
* EPUB validation (`softcover epub:validate`)
    - [Java](http://www.java.com/en/download/help/index_installing.xml)
    - [EpubCheck 3.0](https://github.com/IDPF/epubcheck/releases/download/v3.0/epubcheck-3.0.zip)


## Publishing to the Softcover website
\label{sec:softcover_website}

The \softcover\ command-line client includes commands for interacting with the [Softcover.io](http://softcover.io) website, making it easy to publish the book (in all its formats) to the live web.

### Publishing ebooks

To get started with [Softcover.io](http://softcover.io), first [create an account](http://softcover.io). Once you have an account, log in using the CLI as follows:

```console
$ softcover login
```

\noindent At this point, you're ready to publish to the live site:

```console
$ softcover build:all
$ softcover build:preview
$ softcover publish
```

\noindent You can now navigate to your book using your web browser, and in OS X and Linux you can open the book at the command line as well:

```console
$ softcover open
```

\noindent The result appears in Figure~\ref{fig:softcover_live_book}.

![The HTML book on the live website.\label{fig:softcover_live_book}](images/figures/softcover_live_book.png)

#### One command to rule them all

For convenience, \softcover\ comes with a `deploy` command to build everything and publish the result:

```console
$ softcover deploy
```

\noindent By default, this is equivalent to the following three steps:

```console
$ softcover build:all
$ softcover build:preview
$ softcover publish
```

\noindent You can customize the behavior of `softcover deploy` by editing the
file `.softcover-deploy` in the project's root directory (Listing~\ref{code:deploy_config}). For example,  Listing~\ref{code:deploy_no_preview} removes the `softcover build:preview` command while adding `git push origin`.

\begin{codelisting}
\label{code:deploy_config}
\codecaption{The default deployment configuration file. \\ \filepath{.softcover-deploy}}
```text
# Edit this file to customize your deployment steps with custom command options
# or additional commands.
#
# softcover build:all
# softcover build:preview
# softcover publish
```
\end{codelisting}

\begin{codelisting}
\label{code:deploy_no_preview}
\codecaption{Removing previews and adding a \texttt{git push origin}. \\ \filepath{.softcover-deploy}}
```text
# Edit this file to customize your deployment steps with custom command options
# or additional commands.
#
softcover build:all
softcover publish
git push origin
```
\end{codelisting}

### Selling ebooks and other digital goods

*This section is in preparation in concert with development of the Softcover website.*

<!-- footnotes  -->

[^undefined_xref]: This is what an undefined cross-reference looks like. It will be filled in as soon as the corresponding section is properly defined and labeled.

[^poly_pronunciation]: \PolyTeXnic\ is pronounced exactly like the English word [*polytechnic*](http://www.thefreedictionary.com/polytechnic).

[^maxwell]: If already know [Maxwell's equations](http://en.wikipedia.org/wiki/Maxwell's_equations), the fundamental equations of electrodynamics, Eq.~\eqref{eq:maxwell} may look a little unfamiliar. This is because it writes the equations in a system called *rationalized [MKS units](http://en.wikipedia.org/wiki/MKS_system_of_units)*, which set \( \mu_0=\epsilon_0=1 \). In my view, these are the units in which Maxwell's equations are the most beautiful. Moreover, since the speed of light is \( 1/\sqrt{\mu_0\epsilon_0} \), this choice of units gives us \( c = 1 \) for free.

[^multiple_browsers]: Take care to attach only one browser at a time; otherwise, the Softcover server won't know which browser to refresh.

[^sc_s]: For brevity, you can use `s` in place of `server`, as in `softcover s`. Since `sc` is an alias for `softcover`, you can even write `sc s` to start the local server. This is a little cryptic, so in the text I write `softcover server`, but in real life I nearly always just type `sc s`.

[^ipad_address]: You can find the iPad's local network address by examining the results of `ifconfig`; in my experience the relevant address usually begins with `192` (when on the local wireless network) or `172` (when attached directly to a computer), so you can probably extract the right local address using the command `ifconfig | egrep '(172|192)'`.