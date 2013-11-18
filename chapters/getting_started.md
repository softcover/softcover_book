# Getting started
\label{cha:getting_started}

This is [*The Softcover Book*](http://manual.softcover.io/book)---the manual for *Softcover*, a publishing platform for technical authors. Softcover consists of two parts: an [open-source typesetting system](https://github.com/softcover/softcover) for producing high-quality ebooks, and a [website](http://softcover.io/) for publishing (and optionally selling) ebooks and other digital goods.

an open-source
ebook typesetting system for technical authors.[^1] Its *raison d'\^{e}tre* is producing
professional-grade multi-format ebooks from a common set of source
files. In particular,  accepts input in *Markdown*, a lightweight markup
language, or *\PolyTeX*, an easy-to-learn subset of the powerful
\LaTeX typesetting language, and outputs ebooks as HTML, EPUB, MOBI, and
PDF.  also comes with a local server that automatically rebuilds a
book’s HTML output when the source files change, so that your favorite
text editor and web browser combine to form a real-time development
environment for writing ebooks.[^2] Finally, authors can use  to upload
ebooks and other media files to the [Softcover publishing
platform](http://softcover.io/) with a single command, thereby
dramatically lowering the barrier to publishing, updating, and selling
digital information products.

Originally developed by [Michael Hartl](http://www.michaelhartl.com/) to
write the [*Ruby on Rails Tutorial* book](http://railstutorial.org/book) and [*The Tau
Manifesto*](http://tauday.com/tau-manifesto),  has been rewritten and
expanded by Michael Hartl and Nick Merwin as part of developing
[Softcover](http://softcover.io/). True to its origins,  supports a wide
range of features useful for writing technical books, including
mathematical typesetting (Eq. )[^3] and syntax-highlighted source code
listings (Listing~\ref{code:eval}).  is also well-suited to writing
non-technical books, with support for chapters, sections,
cross-references, footnotes, lists, figures, tables, etc.—the only
requirement is that the *author* be technical. (If you know how to use a
command line and have a favorite text editor, you are technical enough
to use .)

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

Naturally,  itself is written using . Indeed, you can consider this
manual the *de facto*
[spec](http://en.wikipedia.org/wiki/Specification_(technical_standard))
for the system: everything that  can do, this document does. Because the
[source code](http://polytexnic.org/source) of  is available online, you
can learn how to typeset anything you see in this manual simply by
referring to the corresponding place in the source.

[^1]: This document is available online at <http://polytexnic.org/book>.

[^2]: My favorite trick is to connect an iPad to the  server’s address
    on the local network, effectively using the iPad as an external
    monitor. Then, when I save a source file, the iPad’s browser
    magically refreshes with the updated content.

[^3]: Eq.  writes [Maxwell’s
    equations](http://en.wikipedia.org/wiki/Maxwell's_equations), the
    fundamental equations of electrodynamics, in a system called
    *rationalized [MKS
    units](http://en.wikipedia.org/wiki/MKS_system_of_units)*, which set
    $\mu_0=\epsilon_0=1$. In my view, these are the units in which
    Maxwell’s equations are the most beautiful. Moreover, since the
    speed of light is $1/\sqrt{\mu_0\epsilon_0}$, this choice of units
    gives us $c=1$ for free.
