# Getting started
\label{cha:getting_started}

This is [*The Softcover Book*](http://manual.softcover.io/book)---the manual for *Softcover*, a digital publishing platform for technical authors.[^online_version] Based on the technology and business model used in the [Ruby on Rails Tutorial](http://ruby.railstutorial.org/) by [Michael Hartl](http://www.michaelhartl.com/), Softcover is designed to help authors make the transition from "writing a book" to "building a product empire".

Softcover consists of two principal parts: an [open-source typesetting system](https://github.com/softcover/softcover) for producing high-quality multi-format ebooks (Section~\ref{sec:softcover_system}), and a [online platform](http://www.softcover.io/) for publishing, marketing, and selling ebooks and other digital goods (Section~\ref{sec:softcover_website}).

**Note:** Softcover is currently in private beta. Visit [Softcover.io](http://www.softcover.io/) to request an invitation.

## The `softcover` typesetting system
\label{sec:softcover_system}

In this section, we'll cover the basics of `softcover`, an open-source ebook typesetting system for technical authors. Its [*raison d'\^{e}tre*](http://www.merriam-webster.com/dictionary/raison%20d'etre) is producing professional-grade multi-format ebooks from a common set of source files. In particular, `softcover` accepts input in *Markdown*, a lightweight markup language, or *\PolyTeX*, an easy-to-learn subset of the powerful \LaTeX\ typesetting language, and outputs ebooks as HTML, EPUB, MOBI, and PDF. The `softcover` system also comes with a local server that automatically rebuilds a book's HTML output when the source files change, so that your favorite text editor and web browser combine to form a real-time development environment for writing ebooks.[^ipad_trick] Finally, authors can use  to upload ebooks and other media files to the [Softcover website](http://www.softcover.io/) with a single command, thereby dramatically lowering the barrier to publishing, updating, and selling digital information products.

Originally developed under the name *\PolyTeXnic*[^poly_pronunciation] to write the [*Ruby on Rails Tutorial* book](http://railstutorial.org/book) and [*The Tau Manifesto*](http://tauday.com/tau-manifesto), `softcover` has been rewritten and expanded as part of developing the [Softcover publishing platform](http://www.softcover.io/). True to its origins, `softcover` supports a wide range of features useful for writing technical books, including mathematical typesetting (Eq.~\eqref{eq:maxwell})[^maxwell] and syntax-highlighted source code listings (Listing~\ref{code:eval}). It is also well-suited to writing non-technical books, with support for chapters, sections, cross-references, footnotes, lists, figures, tables, etc.---the only requirement is that the *author* be technical. (If you know how to use a command line and have a favorite text editor, you are technical enough to use `softcover`.)

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

Naturally, *The Softcover Book* itself is written using `softcover`. Indeed, you can consider this manual the *de facto* [spec](http://en.wikipedia.org/wiki/Specification_(technical_standard)) for the system: essentially everything that `softcover` can do, this document does. Because the [source code](http://github.com/softcover/softcover_book) of *The Softcover Book* is available online, you can learn how to typeset anything you see in this manual simply by referring to the corresponding place in the source.

### Installing `softcover`

The `softcover` system is distributed as a Ruby gem under the permissive [MIT License](http://opensource.org/licenses/MIT). Installing `softcover` is simple once you have installed Ruby and RubyGems:

```console
$ gem install softcover
```

\noindent This installs the `softcover` command-line interface (CLI) for creating new books, building ebooks, and publishing ebooks and other digital assets to the [Softcover website](http://www.softcover.io/). Let's get started by creating a new book project.

### Creating a Softcover book

The `softcover new` command gets you started writing a new book by generating a template with example markup:

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
.
.
.
Creating README.md
Creating softcover.sty
Creating upquote.sty
Done. Please update book.yml
```


Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod
tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam,
quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo
consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse
cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non
proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

[^online_version]: This document is available online at <http://manual.softcover.io/book>.

[^ipad_trick]: My favorite trick is to connect an iPad to the `softcover` server's address on the local network, effectively using the iPad as an external monitor. Then, when I save a source file, the iPad's browser magically refreshes with the updated content.

[^poly_pronunciation]: \PolyTeXnic\ is pronounced exactly like the English word *polytechnic*.

[^maxwell]: If already know [Maxwell's equations](http://en.wikipedia.org/wiki/Maxwell's_equations), the fundamental equations of electrodynamics, Eq.~\eqref{eq:maxwell} may look a little unfamiliar. This is because it writes the equations in a system called *rationalized [MKS units](http://en.wikipedia.org/wiki/MKS_system_of_units)*, which set \( \mu_0=\epsilon_0=1 \). In my view, these are the units in which Maxwell's equations are the most beautiful. Moreover, since the speed of light is \( 1/\sqrt{\mu_0\epsilon_0} \), this choice of units gives us \( c = 1 \) for free.
