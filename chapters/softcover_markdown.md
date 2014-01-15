# Softcover-flavored Markdown
\label{cha:softcover_markdown}

Softcover therefore supports a *superset* of vanilla Markdown, including the [*kramdown*](http://kramdown.gettalong.org/) extensions (Section~\ref{sec:kramdown}), advanced enhancements such as GitHub-style fenced code blocks (Section~\ref{sec:advanced_enhancements}), and embedded \LaTeX\ (Section~\ref{sec:embedded_latex}).

The Softcover dialect of Markdown is, to our knowledge, the most powerful one available, with support for figures, tables, code listings, and mathematical equations (all with numbered, linked cross-references).

Softcover-flavored Markdown derives much of its power by converting Markdown first to \PolyTeX, a strict subset of the \LaTeX\ typesetting language (Section~\ref{sec:softcover_system}), and then from \PolyTeX\ to HTML, EPUB, MOBI, and PDF\@. The result is an [abstraction layer](https://en.wikipedia.org/wiki/Abstraction_layer) over the underlying \LaTeX;[^latex_polytex] by allowing *embedded* \LaTeX\ as well, Softcover lets users pierce this abstraction layer and typeset things impossible for vanilla Markdown---for example, "\texttt{typewriter text} \textsc{is different from} `code`" (see Section~\ref{sec:embedded_latex} to learn how to do it).

The resulting hybrid input language, though powerful, can get a bit messy, and
adding features to Markdown as described above is a prime example of how weak systems tend to evolve toward strong ones in an [*ad hoc*](http://en.wikipedia.org/wiki/Ad_hoc) way (Box~\ref{aside:polytex_markdown}). Users who want a consistent input syntax with maximum control can dispense with the abstraction layer and write in raw \PolyTeX\ instead (Chapter~\ref{cha:polytex_tutorial}). Indeed, because the Markdown conversion runs through the \PolyTeX\ pipeline, it's possible to start with Markdown and change over to \PolyTeX\ at any time (Section~\ref{sec:markdown_to_polytex}).

\begin{aside}
\label{aside:polytex_markdown}
\heading{Markdown, \PolyTeX, and Hartl's Tenth Rule of Typesetting}

\noindent I've been a fan of Markdown since it first appeared in 2004, and in my view Markdown deserves the enormous popularity it has achieved. Indeed, in a sense it has succeeded a little *too* well, to the point where people use it even when it may not be the best tool for the job. In particular, because it is essentially a thin layer on top of HTML, the original "vanilla" Markdown is ill-suited to producing longer or more structured documents. As a result, virtually every system using "Markdown" in reality uses some augmented version of the original markup language---an implicit acknowledgment that vanilla Markdown is insufficient for industrial-strength typesetting.

On the other end of the spectrum from Markdown is \LaTeX, an industrial-strength typesetting system if ever there was one. \LaTeX, like the Lisp programming language in its domain, can essentially "do anything"; thus, in the spirit of [Greenspun's Tenth Rule of Programming](https://en.wikipedia.org/wiki/Greenspun's_tenth_rule) on Lisp, I hereby offer the following maxim on \LaTeX:

> **Hartl's Tenth Rule of Typesetting**\\
> *Any sufficiently complicated typesetting system contains an ad hoc,
> informally specified, bug-ridden, slow implementation of half of \LaTeX.*

\noindent Looking at the ever-expanding definition of "Markdown"---from [GFM](http://github.github.com/github-flavored-markdown/) to [kramdown](http://kramdown.gettalong.org/) to Softcover-flavored Markdown itself---we see the pattern of Hartl's Tenth Rule emerge. (As with Greenspun's Tenth Rule of Programming, there are no rules 1--9 preceding Hartl's Tenth Rule of Typesetting; calling it the "tenth rule" (without any predecessors) is part of the joke.)

Of course, there's no law saying that we *have* to use Markdown, and Hartl's Tenth Rule suggests a second possibility: *actually using \LaTeX*. Since \LaTeX\ is designed to make print-friendly formats like PostScript and PDF, we do have to make some concessions when outputting multi-format ebooks, mainly because the popular EPUB and MOBI formats ultimately are based on HTML, and there's simply no general mapping from \LaTeX\ to HTML. But what we *can* do is support a *subset* of \LaTeX\ that maps nicely to HTML (and thence to EPUB and MOBI). The result is a version of \LaTeX\ that supports polymorphic output---i.e., *\PolyTeX*. (Unfortunately, even \PolyTeX\ can't fully escape Hartl's Tenth Rule, since producing HTML output from \LaTeX\ requires writing just such an implementation of half of \LaTeX. (Well, we do our best to make it fast and avoid bugs\ldots) But by using \PolyTeX\ we at least avoid creating an ad hoc, informally specified *syntax* as well.)

\PolyTeX, at the cost of some complexity, gives authors considerably more power, flexibility, and extensibility than any variant of Markdown. If you already know HTML or Markdown, \PolyTeX\ is not hard to learn, with the only really scary syntax being for math input---but if you want to typeset mathematics, you need to learn \LaTeX\ anyway:
\[ \left(-\frac{\hbar^2}{2m}\nabla^2 + V\right)\psi = E\psi \]
\[
\left(\frac{p}{q}\right) \left(\frac{q}{p}\right) = (-1)^{[(p-1)/2][(q-1)/2]} \quad\text{($p$, $q$ distinct odd primes)}
\]
(These are the [time-independent Schr\"{o}dinger equation](https://en.wikipedia.org/wiki/Schr%C3%B6dinger_equation#Time-independent_equation) and the [law of quadratic reciprocity](https://en.wikipedia.org/wiki/Quadratic_reciprocity), respectively.)

Despite a fondness for Markdown, \PolyTeX's superior power makes it my preferred Softcover input format. And as a bonus, the resulting \texttt{.tex} files have the highest level of trustworthiness of any file extension (Figure~\ref{fig:xkcd_extensions}). If your curiosity about \PolyTeX\ has been piqued, Chapter~\ref{cha:polytex_tutorial} will get you started.

\end{aside}

![\href{http://www.xkcd.com/1301/}{XKCD 1301}.\label{fig:xkcd_extensions}](images/figures/file_extensions.png)


## The kramdown extensions
\label{sec:kramdown}

The [kramdown](http://kramdown.gettalong.org/) [[sic](https://en.wikipedia.org/wiki/Sic)] project is a pure-Ruby library that supports a superset of Markdown inspired by [Markuku](http://maruku.rubyforge.org/) and [PHP Markdown Extra](http://michelf.ca/projects/php-markdown/extra/). From the perspective of the Softcover platform, the most important additions are support for numbered footnotes and for embedded tables.

The \softcover\ system piggybacks on kramdown's internals, which include a Markdown-to-\LaTeX\ converter to support PDF output. As a result, \softcover\ doesn't support any kramdown syntax (such as embedded `div` tags) that have no natural conversion to \LaTeX. This means that \softcover\ is intentionally less flexible in this regard in order to avoid the supporting HTML output that doesn't also work in PDFs.


### Tables
\label{sec:kramdown_tables}

The kramdown converter includes a lightweight syntax for making tables using pipes (`|`), dashes (`-`), and equals signs (`=`). Pipes define cells as follows:

| A simple | table |
| with multiple | lines|

\noindent This is produced by the following code:

```
| A simple | table |
| with multiple | lines|
```

Slightly more complicated tables can be defined using dashes to define a header and equals signs to define a footer:

| Header1 | Header2 | Header3 |
|---------|---------|---------|
| cell1   | cell2   | cell3   |
| cell4   | cell5   | cell6   |
| cell1   | cell2   | cell3   |
| cell4   | cell5   | cell6   |
|=======
| Foot1   | Foot2   | Foot3

This is produced by the following code

```
| Header1 | Header2 | Header3 |
|---------|---------|---------|
| cell1   | cell2   | cell3   |
| cell4   | cell5   | cell6   |
| cell1   | cell2   | cell3   |
| cell4   | cell5   | cell6   |
|=======
| Foot1   | Foot2   | Foot3
```

One important caveat is that dashes *can't* be used to define horizontal rules, so

```
| Header1 | Header2 | Header3 |
|---------|---------|---------|
| cell1   | cell2   | cell3   |
|---------|---------|---------|
| cell4   | cell5   | cell6   |
```

\noindent produces

| Header1 | Header2 | Header3 |
|---------|---------|---------|
| cell1   | cell2   | cell3   |
|---------|---------|---------|
| cell4   | cell5   | cell6   |

\noindent instead of the expected

\begin{center}
\begin{tabular}{|c|c|c|}
\hline
Header1 & Header2 & Header3 \\ \hline
cell1   & cell2   & cell3 \\ \hline
cell4   & cell5   & cell6 \\
\hline
\end{tabular}
\end{center}

\noindent Authors who want to produce this or other more complicated effects should use embedded \LaTeX\ (Section~\ref{sec:embedded_latex}) or \PolyTeX\ (Chapter~\ref{cha:polytex_tutorial}).


### Numbered footnotes

Vanilla Markdown doesn't support numbered footnotes, so kramdown adds them using the following syntax:[^example_footnote]

```text
kramdown adds them using the following syntax:[^example_footnote]
.
.
.
[^example_footnote]: This is an example footnote.
```

\noindent where `[^example_footnote]: This is an example footnote.` appears at the bottom of the file. *Note*: the order of the footnotes is determined by the order of their appearance in the main text; the order at the bottom of the file is irrelevant.

### Miscellaneous features

In addition to tables and footnotes, kramdown includes other miscellaneous features, including a verbatim override to prevent Markdown processing. This means, for example, that you can demonstrate Markdown's double-asterisk boldface syntax {::nomarkdown}**like this**{:/}:
```
you can demonstrate Markdown's double-asterisk boldface syntax
{::nomarkdown}**like this**{:/}
```

If you just want to escape individual characters, such as \*, you can do so with a backslash:
```
such as \*, you can do so with a backslash
```

\noindent Here are some other characters you might want to escape:
```
*         asterisk
`         back tick
<<        left guillemet
>>        right guillemet
$         dollar sign
{}        braces
```

Finally, in kramdown snake_case_words appear with underscores (perhaps familiar from GitHub-flavored Markdown, which has the same feature). This is convenient because vanilla Markdown would intepret the underscores as emphasis, yielding "snake*case*words", which probably isn't what you wanted.

## Other advanced enhancements
\label{sec:advanced_enhancements}

Softcover-flavored Markdown includes some additional advanced enhancements over vanilla Markdown and kramdown.

### GitHub-flavored fenced code blocks
\label{sec:code_fencing}

Softcover borrows one key feature from [Github-flavored Markdown](https://help.github.com/articles/github-flavored-markdown), namely, [*fenced code blocks*](https://help.github.com/articles/github-flavored-markdown#fenced-code-blocks), or "code fencing" for short. This syntax involves placing code samples inside "fences" composed of three backticks (\verb+```+):

```
# "Hello, world!" in Ruby
def hello
  puts "hello, world!"
end
```

\noindent In Markdown, this is produced by the following code:

    ```
    # "Hello, world!" in Ruby
    def hello
      puts "hello, world!"
    end
    ```

\noindent (Here we indicate code using indentation rather than a shaded code blocksbecause plain code fences can't talk about themselves: there's no way for the parser to know if the first inner \verb+```+ is the end of a fenced block or the beginning of example code.)

Following GitHub's example, Softcover supports an optional string after the opening of the fence indicating the language of the sample, yielding language-specific syntax highlighting:

```ruby
# Prints a greeting.
def hello
  puts "hello, world!"
end
```

\noindent This is produced by the following Markdown:

```
```ruby
# Prints a greeting.
def hello
  puts "hello, world!"
end
```
```

\noindent The language designation (e.g., `ruby`) can be any language supported by the [available Pygments lexers](http://pygments.org/docs/lexers/).

As a final enhancement, Softcover adds a hook directly into the [Pygments formatter options](http://pygments.org/docs/formatters/), allowing, for example, turning on line numbering and highlighting specific lines:

```ruby, options: "linenos": true, "hl_lines": [1, 3]
# Prints a greeting.
def hello
  puts "hello, world!"
end
```

\noindent This is produced by the following Markdown:

```text
```ruby, options: "linenos": true, "hl_lines": [1, 3]
# Prints a greeting.
def hello
  puts "hello, world!"
end
```
```

\noindent Here the hash

```
options: "linenos": true, "hl_lines": [1, 3]
```

\noindent gets passed directly to Pygments, so any option listed on the [Pygments formatter options page](http://pygments.org/docs/formatters/) is automatically supported by Softcover.

### Leanpub-style (???)

***Maybe it's just kramdown?***

### Code inclusion

Softcover supports code inclusion directly from local files, such as this valedictory program:

<<(source/goodnight.rb)

\noindent The corresponding file is in `source/goodnight.rb`, so the markup

```text
<<(source/goodnight.rb)
```

\noindent includes the source of the file in the current document. Because Pygments associates the `.rb` filename extension with Ruby, syntax highlighting comes for free. For extensions that Pygments doesn't understand, you can add additional information as in Section~\ref{sec:code_fencing}. For example, this is the `book.yml` file for a newly generated example book (last seen in Listing~\ref{code:book_yml}):

<<(example_book/config/book.yml, lang: yaml)

\noindent This is produced by passing the `lang: yaml` option to tell \softcover\ to highlight the code as YAML:

```text
<<(example_book/config/book.yml, lang: yaml)
```

### Embedded math

Softcover supports embedded math, such as {$$}\phi^2 - \phi - 1 = 0{/$$}, and centered math, such as

{$$}
\phi = \frac{1+\sqrt{5}}{2}.
{/$$}

\noindent This uses the syntax `{\$\$\}...\{/\$\$\}` for both inline and centered math, with the only difference being the absence or presence of newlines:

```
Softcover supports embedded math, such as {$$}\phi^2 - \phi - 1 = 0{/$$}, and
centered math, such as

{$$}
\phi = \frac{1+\sqrt{5}}{2}.
{/$$}
```

\noindent This syntax is odd and is included only for compatibility with other systems (particularly Leanpub's proprietary Markdown variant); \softcover\ also supports the proper \LaTeX\ syntax (Section~\ref{sec:latex_math}), which is strongly preferred.


## Embedded \LaTeX
\label{sec:embedded_latex}

Most advanced option: select \LaTeX\ embedding. Leads to gotchas

As noted in the introduction, this allows us to typeset things like "\texttt{typewriter text} \textsc{is different from} `code`", which in embedded \LaTeX\ appears as follows:

```latex
"\texttt{typewriter text} is different from `code`"
```

\noindent This uses \verb+\texttt+ to set \texttt{typewriter text} and \verb+\textsc+ to set \textsc{small caps}.


### \LaTeX\ commands

\LaTeX\ commands start with a backslash `\` and typically take 1--3 arguments.


Can use verbatim, typewriter text, etc. kode

Caveat: nesting doesn't work

Also supports \LaTeX\ dashes, as in 1999--2003 and --- and ties~

noindent backslash space


### Labels and cross-references

One of the biggest advantages of using embedded \LaTeX\ is being able to use *cross-references* to tie together the structure of the document. Cross-references consist of pairing a *label* with a *reference*.

For example, at the beginning of this chapter is a label appearing immediately after the chapter indicator:

```latex
# Softcover-flavored Markdown
\label{cha:softcover_markdown}
```

\noindent This allows the definition of a cross-reference using the \verb+\ref+ command, whose argument is the label name. Thus,

```latex
Chapter~\ref{cha:introduction_to_markdown}
```

\noindent produces "Chapter~\ref{cha:introduction_to_markdown}". Similarly, the beginning of this section has

```latex
## Embedded \LaTeX
\label{sec:embedded_latex}
```

\noindent so that

```latex
Section~\ref{sec:embedded_latex}
```

\noindent produces "Section~\ref{sec:embedded_latex}". These cross-references are clickable links across all output formats (HTML, EPUB, MOBI, and PDF).

In addition to working with chapters and sections, Softcover cross-references also work with code listings, aside boxes, figures, tables, and centered equations. The label names can be virtually anything, but I follow the common convention of [namespacing](https://en.wikipedia.org/wiki/Namespace) them by type, so that chapter labels are prefixed with `cha:`, sections with `sec:`, codelistings with `code:`, etc.

The advantage of using named labels instead of hard-coded numbers can hardly be over-stated: it means that if you add a new chapter to the beginning of your book, all the subsequent cross-references will automatically be renumbered. There is simply no way an author could keep track of more than a few cross-references by hand, but with Softcover you can use as many as you want.

I am a strong advocate of extensive cross-referencing, and not only because of their obvious benefits to readers. Cross-references are extraordinarily useful for *authors* as well: they let you immediately orient yourself when picking up after leaving off or going back later to edit. They are also useful when deferring material to the future, as undefined cross-references are helpful reminders to fill in the material later. I like to say that *cross-references are the connective tissue in the body of a book*.

### Tabular and tables

We saw in Section~\ref{sec:kramdown_tables} that Softcover supports tables via a literal-minded kramdown syntax, as in

```
| A simple | table |
| with multiple | lines|
```

\noindent Softcover also supports more powerful \LaTeX\ tables via the `tabular` environment:

\begin{tabular}{|r|lc|}
  \hline
  2A & hexadecimal & (base 16) \\
  52 & octal & (base 8) \\
  101010 & binary & (base 2) \\
  \hline
  42 & decimal & (base 10) \\
  \hline
  \multicolumn{3}{|c|}{\textsc{All your base are belong to us.}} \\
  \hline
\end{tabular}

\noindent This is produced by the code in Listing~\ref{code:tabular}.

\begin{codelisting}
\label{code:tabular}
\codecaption{Code to produce a \texttt{tabular} environment.}
```latex, options: "linenos": true
\begin{tabular}{|r|lc|}
  \hline
  2A & hexadecimal & (base 16) \\
  52 & octal & (base 8) \\
  101010 & binary & (base 2) \\
  \hline
  42 & decimal & (base 10) \\
  \hline
  \multicolumn{3}{|c|}{\textsc{All your base are belong to us.}} \\
  \hline
\end{tabular}
```
\end{codelisting}

Let's examine the anatomy of the table in Listing~\ref{code:tabular}:

- **Line 1** shows that `begin` in the `tabular` environment takes *two* arguments. The first argument simply identifies it as a `tabular` environment, while the second defines the total number of columns (three), their alignment (`r` for "right", `l` for "left", and `c` for "center"), and the presence or absence of vertical borders between the columns (borders everywhere except between the second and third columns).

- **Lines 2, 6, 8, and 10** use the \verb+\hline+ command a horizontal line between rows using the.

- **Lines 3--5 and line 7** include three entries each, one for each column, separated by the ampersand character `&` and ended with a double-backslash \verb+\\+.

- **Line 9** shows a \verb+\multicolumn+ command to create a row that spans three columns. It takes three arguments: the number of columns (3), the alignment and vertical borders (centered with borders on either side) and the contents ([AYBABTU](https://en.wikipedia.org/wiki/All_your_base_are_belong_to_us)).

In addition to the `tabular` environment, Softcover also supports the somewhat confusingly named `table` environment, which turns a `tabular` environment into a "float", which in a print or PDF document will be placed automatically by \TeX's float-placement algorithms. A `table` environment is typically used with a caption and a label (which should be placed *inside* the caption), which yields numbered, cross-referenced tables, as seen Table~\ref{table:answer}. The code to produce Table~\ref{table:answer} appears in Listing~\ref{code:answer_table}. Note that because of how tables are processed, it is important that the `caption` contents appear in a single line of text; it's fine if the line wraps in your text editor, but it should contain no newlines.

\begin{table}
\caption{An important answer in several bases.\label{table:answer}}
\begin{tabular}{|r|lc|}
  \hline
  2A & hexadecimal & (base 16) \\
  52 & octal & (base 8) \\
  101010 & binary & (base 2) \\
  \hline
  42 & decimal & (base 10) \\
  \hline
  \multicolumn{3}{|c|}{\textsc{All your base are belong to us.}} \\
  \hline
\end{tabular}
\end{table}

\begin{codelisting}
\label{code:answer_table}
\codecaption{The code to produce Table~\ref{table:answer}}
```latex, options: "hl_lines": [1, 2, 14]
\begin{table}
\caption{An important answer in several bases.\label{table:answer}}
\begin{tabular}{|r|lc|}
  \hline
  2A & hexadecimal & (base 16) \\
  52 & octal & (base 8) \\
  101010 & binary & (base 2) \\
  \hline
  42 & decimal & (base 10) \\
  \hline
  \multicolumn{3}{|c|}{\textsc{All your base are belong to us.}} \\
  \hline
\end{tabular}
\end{table}
```
\end{codelisting}

### Figures

We've seen figures. Several different possibilities. Plain figure, as seen before in Section~\ref{sec:links_and_images}:

![Some dude.](images/2011_michael_hartl.png)

Can also make numbered figures with captions by including a label, as shown in Figure~\ref{fig:michael_hartl}. Using a bare label yields a figure with just a number (Figure~\ref{fig:road_to_hana}). The code to generated the two captioned figures is shown in Listing~\ref{code:captioned_figures}. As usual, the labels are namespaced, in this case with `fig:`.

![Some dude.\label{fig:michael_hartl}](images/2011_michael_hartl.png)

![\label{fig:road_to_hana}](images/2011_michael_hartl.png)

\begin{codelisting}
\label{code:captioned_figures}
\codecaption{Code for Figure~\ref{fig:michael_hartl} and Figure~\ref{fig:road_to_hana}.}
```latex
![Some dude.\label{fig:michael_hartl}](images/2011_michael_hartl.png)

![\label{fig:road_to_hana}](images/2011_michael_hartl.png)
```
\end{codelisting}


### Code listings

In addition to syntax-highlighted source code, \softcover\ also supports code *listings*, which are numbered code blocks with optional captions. For example,

```latex
\begin{codelisting}
\label{code:palindrome}
\codecaption{Adding a \texttt{palindrome?} method to strings.}
```ruby
class String
  def palindrome?
    self == self.reverse
  end
end
```
\end{codelisting}
```

\noindent produces Listing~\ref{code:palindrome}. The cross-reference itself is set using the code in Listing~\ref{code:palindrome_reference}.

\begin{codelisting}
\label{code:palindrome}
\codecaption{Adding a \texttt{palindrome?} method to strings.}
```ruby
class String
  def palindrome?
    self == self.reverse
  end
end
```
\end{codelisting}


\begin{codelisting}
\label{code:palindrome_reference}
\codecaption{A reference to the palindrome code listing.}
```latex
Listing~\ref{code:palindrome}
```
\end{codelisting}


To get a code listing with only a number, simply leave the \verb+\codecaption+ empty. For example, the code in Listing~\ref{code:no_caption_code} produces Listing~\ref{code:palindrome_no_caption}.

\begin{codelisting}
\label{code:no_caption_code}
\codecaption{}
```latex
\begin{codelisting}
\label{code:palindrome_no_caption}
\codecaption{}
```ruby
class String
  def palindrome?
    self == self.reverse
  end
end
```
\end{codelisting}
```
\end{codelisting}

\begin{codelisting}
\label{code:palindrome_no_caption}
\codecaption{}
```ruby
class String
  def palindrome?
    self == self.reverse
  end
end
```
\end{codelisting}

__obviously lots of typing, so use macros. I plan to release mine for sublime text__

### Aside boxes

In the course of writing a narrative document like a book, you may find that you want to make an *aside*, i.e., a digression that covers some ancillary topic in more depth. In order to prevent breaking up the narrative, Softcover supports an *aside box environment* suitable for cross-referencing. We've seen several examples so far in this manual, most recently in Box~\ref{aside:polytex_markdown}. The code to produce that aside appears in Listing~\ref{code:polytex_markdown_aside_code}, and the code to produce the reference appears in Listing~\ref{code:polytex_markdown_ref_code}.

\begin{codelisting}
\label{code:polytex_markdown_aside_code}
\codecaption{The code to produce Box~\ref{aside:polytex_markdown}.}
```latex, options: "hl_lines": [1, 2, 3, 12]
\begin{aside}
\label{aside:polytex_markdown}
\heading{Markdown, \PolyTeX, and Hartl's Tenth Rule of Typesetting}

\noindent I've been a fan of Markdown since it first appeared in 2004,
.
.
.
If your curiosity about \PolyTeX\ has been piqued,
Chapter~\ref{cha:polytex_tutorial} will get you started.

\end{aside}
```
\end{codelisting}

\begin{codelisting}
\label{code:polytex_markdown_ref_code}
\codecaption{The code to make a reference to Box~\ref{aside:polytex_markdown}.}
```latex, options: "hl_lines": [2]
We've seen several examples so far in this manual, most recently in
Box~\ref{aside:polytex_markdown}.
```
\end{codelisting}

As with previous environments, the aside code in Listing~\ref{code:polytex_markdown_aside_code} uses a prefix for the label---in this case, `aside:`. We also see that aside box captions are created using the \verb+\heading+ command, whose argument is optional; the code

```latex
\heading{}
```

\noindent produces a box with a number but no text.


### Math and numbered equations

We've seen math using the ugly `{\$\$\}...\{/\$\$\}` syntax, but also can use proper \LaTeX\ syntax, as in \( \phi^2 - \phi - 1 = 0 \), which is set using \LaTeX's native "backslash parenthesis" notation:

```latex
as in \( \phi^2 - \phi - 1 = 0 \), which is set
```

\noindent The pithier \TeX-style dollar-sign notation is not supported by Markdown input in order to avoid confusion with (very common) ordinary dollar signs, but it is supported by \PolyTeX. Power users who want to be able to write

```latex
$x$
```

\noindent instead of

```latex
\( x \)
```

\noindent should thus use raw \PolyTeX.


\noindent Softcover also supports centered math, as follows:
\[ \phi^2 - \phi - 1 = 0. \]
This equation is set using \LaTeX's native "backslash square bracket" notation:


```latex
\[ \phi^2 - \phi - 1 = 0. \]
```

\noindent The \TeX-style double-dollar-sign notation is not supported by Markdown input in order to avoid confusion with (very common) ordinary dollar signs, but it is supported by \PolyTeX. Power users who want to be able to write

```latex
$$ \phi^2 - \phi - 1 = 0. $$
```

\noindent instead of

```latex
\[ \phi^2 - \phi - 1 = 0. \]
```

\noindent should thus use raw \PolyTeX. (I actually prefer \TeX-style dollar signs for inline math but \LaTeX-style backslash square brackets for centered math---one of the many reasons I prefer \PolyTeX\ to Markdown for serious typesetting.)

Finally, Softcover supports numbered, cross-referenced equations using the `equation` environment, as shown in Eq.~\eqref{eq:golden_ratio}. The code to produce this equation is shown in Listing~\ref{code:golden_ratio_code}. To my knowledge, Softcover is the only typesetting system capable of producing numbered, linked, cross-referenced equations in all output formats (HTML, EPUB, MOBI, and PDF).[^eq_epub_mobi]

\begin{equation}
\label{eq:golden_ratio}
\phi = \frac{1+\sqrt{5}}{2}\approx 1.618 \qquad{\text{The Golden Ratio}}
\end{equation}

\begin{codelisting}
\label{code:golden_ratio_code}
\codecaption{The \LaTeX\ code to produce Eq.~\eqref{eq:golden_ratio}.}
```latex, options: "hl_lines": [1, 2, 4]
\begin{equation}
\label{eq:golden_ratio}
\phi = \frac{1+\sqrt{5}}{2}\approx 1.618 \qquad{\text{The Golden Ratio}}
\end{equation}
```
\end{codelisting}

Softcover supports both common \LaTeX\ methods for referencing equations: normal references using \verb+\ref+, as in Eq.~\ref{eq:golden_ratio}, and the preferred \verb+\eqref+, which automatically adds parentheses around the equation number, as in Eq.~\eqref{eq:golden_ratio}. The latter is especially useful when omitting the "Eq." part, allowing compact equation references like \eqref{eq:golden_ratio}. Listing~\ref{code:eq_refs} compares the methods.

\begin{codelisting}
\label{code:eq_refs}
\codecaption{Comparing the equation reference methods.}
```latex
Eq.~\ref{eq:golden_ratio}      % produces "Eq. 3.1"
Eq.~\eqref{eq:golden_ratio}    % produces "Eq. (3.1)"
\eqref{eq:golden_ratio}        % produces "(3.1)"
```
\end{codelisting}

## Switching from Markdown to \PolyTeX

It's possible to switch over at any time, can be done on a chapter-by-chapter basis.

Just move the generated \PolyTeX\ file from the `generated_polytex` directory into the `chapters/` directory and edit the `Book.txt` file. Once you're confident you really want to make the switch, remove the original Markdown file. (If you're using Git for version control, which I *strongly* recommend, you can use `git rm` to remove the file. It will then be available in your history should you ever get cold feet and want to go back to Markdown.)

<!-- footnotes  -->

[^latex_polytex]: This manual uses "\PolyTeX" when the distinction with \LaTeX\ is important and "\LaTeX" otherwise.

[^example_footnote]: This is an example footnote.

[^eq_epub_mobi]: The real challenge is producing EPUB and MOBI output. The trick is to (1) create a self-contained HTML page with embedded math, (2) include the amazing [MathJax](http://www.mathjax.org/) JavaScript library, configured to render math as [SVG](https://en.wikipedia.org/wiki/Scalable_Vector_Graphics) images, (3) hit the page with the headless [PhantomJS](http://phantomjs.org/) browser to force MathJax to render the math (including any equation numbers), (4) extract self-contained SVGs from the rendered pages, and (5) use [Inkscape](http://www.inkscape.org/) to convert the SVGs to PNGs for inclusion in EPUB and MOBI books. Easy, right? No, in fact, it was excruciating and possibly required excessive amounts of profanity to achieve. But it's done, so ha.