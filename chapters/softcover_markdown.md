# Softcover-flavored Markdown
\label{cha:softcover_flavored_markdown}

As noted in Chapter~\ref{cha:introduction_to_markdown}, the classic implementation of Markdown is beautifully simple but is not adequate for serious typesetting. Softcover therefore supports a *superset* of vanilla Markdown called *Softcovered-flavored Markdown* (SFM), which includes select [*kramdown*](http://kramdown.gettalong.org/) extensions (Section~\ref{sec:kramdown}), advanced enhancements such as GitHub-style fenced code blocks (Section~\ref{sec:advanced_enhancements}), and *super*-advanced additions via embedded \LaTeX\ (Section~\ref{sec:embedded_latex}). As noted in Chapter~\ref{cha:introduction_to_markdown}, the Softcover dialect of Markdown is, to our knowledge, the most powerful one available.

Softcover-flavored Markdown derives much of its power by converting \linebreak Markdown first to \PolyTeX, a strict subset of the \LaTeX\ typesetting language (Section~\ref{sec:softcover_system}), and then from \PolyTeX\ to HTML, EPUB, MOBI, and PDF\@. The result is an [abstraction layer](https://en.wikipedia.org/wiki/Abstraction_layer) over the underlying \LaTeX;[^latex_polytex] by allowing *embedded* \LaTeX\ as well, Softcover lets users pierce this abstraction layer to typeset things impossible for vanilla Markdown---for example, "\texttt{typewriter text} \textsc{is different from} `code`". (See Section~\ref{sec:embedded_latex} to learn how to typeset this.)

The resulting hybrid input language, though powerful, can get a bit messy, and
adding features to Markdown as described above is a prime example of how weak systems tend to evolve toward strong ones in an [*ad hoc*](http://en.wikipedia.org/wiki/Ad_hoc) way (Box~\ref{aside:polytex_markdown}). Users who want a consistent input syntax with maximum control can dispense with the abstraction layer and write in raw \PolyTeX\ instead (Chapter~\ref{cha:polytex_tutorial}). Indeed, because the Markdown conversion runs through the \PolyTeX\ pipeline, it's possible to start with Markdown and change over to \PolyTeX\ at any time (Section~\ref{sec:markdown_to_polytex}).

\begin{aside}
\label{aside:polytex_markdown}
\heading{Markdown, \PolyTeX, and Hartl's Tenth Rule of Typesetting}

\noindent I've been a fan of Markdown since it first appeared in 2004. Markdown is my first choice for things like [README files](https://github.com/softcover/softcover/blob/master/README.md) and [short news announcements](http://news.railstutorial.org/), and in my view Markdown deserves the enormous popularity it has achieved. Indeed, in a sense it has succeeded a little *too* well, to the point where people use it even when it may not be the best tool for the job. In particular, because it is essentially a thin layer on top of HTML, the original "vanilla" Markdown is ill-suited to producing longer or more structured documents. As a result, virtually every system using "Markdown" for ebook publishing in reality uses some augmented version of the original markup language---an implicit acknowledgment that vanilla Markdown is insufficient for industrial-strength typesetting.

On the other end of the spectrum from Markdown is \LaTeX, an industrial-strength typesetting system if ever there was one. \LaTeX, like the Lisp programming language in its domain, can essentially "do anything"; thus, in the spirit of [Greenspun's Tenth Rule of Programming](https://en.wikipedia.org/wiki/Greenspun's_tenth_rule) on Lisp, I hereby offer the following maxim on \LaTeX:

> **Hartl's Tenth Rule of Typesetting**\\
> *Any sufficiently complicated typesetting system contains an ad hoc,
> informally specified, bug-ridden, slow implementation of half of \LaTeX.*

\noindent Looking at the ever-expanding definition of "Markdown"---from [GitHub-flavored Markdown](http://github.github.com/github-flavored-markdown/) to [kramdown](http://kramdown.gettalong.org/) to Softcover-flavored Markdown itself---we see the pattern of Hartl's Tenth Rule emerge. (As with Greenspun's Tenth Rule of Programming, there are no rules 1--9 preceding Hartl's Tenth Rule of Typesetting; calling it the "tenth rule" is part of the joke.)

Of course, there's no law saying that we *have* to use Markdown, augmented or otherwise, and Hartl's Tenth Rule suggests a second possibility: *actually using \LaTeX*. Since \LaTeX\ is designed to make print-quality formats like PostScript and PDF, we do have to make some concessions when outputting multi-format ebooks, mainly because the popular EPUB and MOBI formats ultimately are based on HTML, and there's simply no general mapping from \LaTeX\ to HTML\@. But what we *can* do is support a *subset* of \LaTeX\ that maps nicely to HTML (and thence to EPUB and MOBI). The result is a version of \LaTeX\ that supports *polymorphic output*---i.e., *\PolyTeX*. (Unfortunately, even \PolyTeX\ can't fully escape Hartl's Tenth Rule, since producing HTML output from \LaTeX\ requires writing just such an implementation of half of \LaTeX\@. But by using \PolyTeX\ we at least avoid creating an *ad hoc*, informally specified *syntax* as well.)

\PolyTeX, at the cost of some complexity, gives authors considerably more power, flexibility, and extensibility than any variant of Markdown, Softcover-flavored Markdown included. If you already know HTML or Markdown, \PolyTeX\ is not hard to learn, with the only really scary syntax being for math input---but if you want to typeset mathematics, you need to learn \LaTeX\ anyway:
\[ \left(-\frac{\hbar^2}{2m}\nabla^2 + V\right)\psi = E\psi \]
\[
\left(\frac{p}{q}\right) \left(\frac{q}{p}\right) = (-1)^{[(p-1)/2][(q-1)/2]} \quad\text{($p$, $q$ distinct odd primes)}
\]
(These are the [time-independent Schr\"{o}dinger equation](https://en.wikipedia.org/wiki/Schr%C3%B6dinger_equation#Time-independent_equation) and the [law of quadratic reciprocity](https://en.wikipedia.org/wiki/Quadratic_reciprocity), respectively.) As a bonus, the resulting \texttt{.tex} files have the highest level of trustworthiness of any file extension (Figure~\ref{fig:xkcd_extensions}).

Despite a fondness for Markdown, \PolyTeX's superior power makes it my preferred Softcover input format. If your curiosity about \PolyTeX\ has been piqued, Chapter~\ref{cha:polytex_tutorial} will help get you started.

\end{aside}

![\href{http://www.xkcd.com/1301/}{XKCD 1301}.\label{fig:xkcd_extensions}](images/figures/file_extensions.png)


## The kramdown extensions
\label{sec:kramdown}

The [kramdown](http://kramdown.gettalong.org/) [[sic](https://en.wikipedia.org/wiki/Sic)] project is a pure-Ruby library that supports a superset of Markdown inspired by [Maruku](http://maruku.rubyforge.org/) and [PHP Markdown Extra](http://michelf.ca/projects/php-markdown/extra/). From the perspective of the Softcover platform, the most important additions are support for simple embedded tables and numbered footnotes.

The Softcover system piggybacks on kramdown's internals, which include a Markdown-to-\LaTeX\ converter to support PDF output. As a result, Softcover doesn't support kramdown syntax (such as embedded `div` tags) that can't be converted naturally to \LaTeX\@. This means that Softcover is intentionally less flexible in this regard in order to avoid the supporting HTML output that doesn't also work in PDFs.


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

\noindent Authors who want to produce tables with horizontal rules (or other more complicated effects) should use embedded \LaTeX\ (Section~\ref{sec:embedded_latex}) or \PolyTeX\ (Chapter~\ref{cha:polytex_tutorial}).


### Numbered footnotes

Vanilla Markdown doesn't support numbered footnotes, so kramdown adds them using the following syntax:[^example_footnote]

```text
kramdown adds them using the following syntax:[^example_footnote]
.
.
.
[^example_footnote]: This is an example footnote.
```

\noindent The vertical ellipsis indicates that intervening text has been omitted, and the code
```text
[^example_footnote]: This is an example footnote.
```
\noindent conventionally appears at the bottom of the file. Note that the order of the footnotes is determined by the order of their appearance in the main text; the order at the bottom of the file is irrelevant.

### Miscellaneous features
\label{sec:miscellaneous_features}

In addition to tables and footnotes, kramdown includes other miscellaneous features, including a verbatim override to prevent Markdown processing. This means, for example, that you can typeset a literal example of Markdown's double-asterisk boldface syntax {::nomarkdown}**like this**{:/}:
```
you can demonstrate Markdown's double-asterisk boldface syntax
{::nomarkdown}**like this**{:/}
```
(I personally find this syntax ugly and hard to remember, and prefer to typeset inline verbatim text using \LaTeX's \verb+\verb+ syntax; see Section~\ref{sec:embedded_latex_commands} for details.)

If you just want to escape individual characters, such as \*, you can do so with a backslash:
```
such as \*, you can do so with a backslash
```

\noindent Here are some other common characters you might want to escape:
```
*         asterisk
`         back tick
<<        left guillemet
>>        right guillemet
{}        braces
|         Unix pipe
```

\noindent Note that some of \LaTeX's special characters, such as $, are automatically escaped.

Finally, in kramdown snake_case_words appear with underscores (a feature it shares with GitHub-flavored Markdown). This is convenient because vanilla Markdown would intepret the underscores as emphasis, yielding "snake*case*\-words", which probably isn't what you intended.

## Other advanced enhancements
\label{sec:advanced_enhancements}

Softcover-flavored Markdown includes several additional advanced en\-hance\-ments over vanilla Markdown and kramdown. Principal among these are *fenced code blocks* (Section~\ref{sec:code_fencing}), which are the preferred way to include code blocks in the Softcover system, and *code inclusion* (Section~\ref{sec:code_inclusion}), which allows you to include code snippets from the local filesystem into the current document.

### GitHub-flavored fenced code blocks
\label{sec:code_fencing}

Softcover borrows one key feature from [Github-flavored Markdown](https://help.github.com/articles/github-flavored-markdown), namely, [fenced code blocks](https://help.github.com/articles/github-flavored-markdown#fenced-code-blocks), or "code fencing" for short. This syntax involves placing code samples inside "fences" composed of three backticks (\verb+```+):

```
# "Hello, world!" in Ruby
def hello
  puts "hello, world!"
end
```

\noindent In Markdown, this is produced by the following code:

{lang="text"}
    ```
    # "Hello, world!" in Ruby
    def hello
      puts "hello, world!"
    end
    ```

\noindent

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

\noindent The language designation (e.g., `ruby`) can be any language supported by the [available Pygments lexers](http://pygments.org/docs/lexers/) which is most of them, including Ruby and \LaTeX\ (though not, annoyingly, Markdown). For example, here is the highlighting for a combination of HTML and PHP:

```html+php
Name: <input type="text" name="name" value="<?php echo $name;?>">
```

\noindent This is produced by the code

```
```html+php
Name: <input type="text" name="name" value="<?php echo $name;?>">
```
```


As a final enhancement, Softcover adds a hook directly into the [Pygments formatter options](http://pygments.org/docs/formatters/) via an options hash. This allows, for example, turning on line numbering and highlighting specific lines:

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

Because of how Softcover processes code blocks, any text immediately after code will be treated as a new paragraph. This isn't a problem in HTML, EPUB, or MOBI, but in PDFs new paragraphs are indented by default. If this isn't what you want---i.e., if the code block should be considered part of the middle of a paragraph---it is necessary to prepend the \LaTeX\ command \verb+\noindent+ before the first line after the block, as follows:

```text
```ruby
# Prints a greeting.
def hello
  puts "hello, world!"
end
```

\noindent This is produced by the following Markdown
```

\noindent See Section~\ref{sec:embedded_latex_commands} for more details.


### Leanpub-style language blocks

SFM supports indented code blocks with an explicit language designation, which is based on [Leanpub](http://leanpub.com/)'s proprietary Markdown variant. This is mainly useful when using SFM to talk about SFM\@. In particular, plain code fences can't talk about themselves, because there's no way for the parser to know if the first inner \verb+```+ is the end of a fenced block or the beginning of example code. Thus, a code block like
{lang="text"}
    ```
    # "Hello, world!" in Ruby
    def hello
      puts "hello, world!"
    end
    ```
\noindent can't be set by nesting one fenced block inside another. Instead, we use
```
{lang="text"}
    ```
    # "Hello, world!" in Ruby
    def hello
      puts "hello, world!"
    end
    ```
```
\noindent where the line
```
{lang="text"}
```
\noindent specifies the language explicitly (in this case, `text`, because, as noted in Section~\ref{sec:code_fencing}, Pygments lacks a Markdown lexer).

Although this syntax can be used to typeset highlighted code like
{lang="ruby"}
    # Prints a greeting.
    def hello
      puts "hello, world!"
    end
\noindent using
```
{lang="ruby"}
    # Prints a greeting.
    def hello
      puts "hello, world!"
    end
```
\noindent I virtually always prefer to use code fencing instead, i.e.,
{lang="text"}
    ```ruby
    # Prints a greeting.
    def hello
      puts "hello, world!"
    end
    ````

### Code inclusion
\label{sec:code_inclusion}

Softcover supports code inclusion directly from local files, such as this program to wish "goodnight" to the Moon:

<<(source/goodnight.rb)

\noindent The corresponding file is in `source/goodnight.rb`, so the markup

```text
<<(source/goodnight.rb)
```

\noindent includes the source of the file into the current document. Because Softcover automatically associates the `.rb` filename extension with the code block, syntax highlighting comes for free via Pygments. For extensions that Pygments doesn't understand, you can add additional information as in Section~\ref{sec:code_fencing}. For example, this is the `book.yml` file for a newly generated example book (last seen in Listing~\ref{code:book_yml}):

<<(example_book/config/book.yml, lang: yaml)

\noindent Because Pygments (for some odd reason) doesn't understand `.yml` but *does* understand `.yaml`, we can arrange for the proper highlighting by passing the `lang: yaml` option, which tells Pygments to highlight the code as [YAML](https://en.wikipedia.org/wiki/YAML):

```text
<<(example_book/config/book.yml, lang: yaml)
```

### Embedded math
\label{sec:terrible_math}

Softcover supports embedded math via the terrible syntax `{\$\$\}...\{/\$\$\}`, as in {$$}\phi^2 - \phi - 1 = 0{/$$}, and centered math, as in

{$$}
\phi = \frac{1+\sqrt{5}}{2}.
{/$$}

\noindent This works for both inline and centered math, with the only difference being the absence or presence of newlines:

```
Softcover supports embedded math via the terrible syntax `{\$\$\}...\{/\$\$\}`,
as in {$$}\phi^2 - \phi - 1 = 0{/$$}, and centered math, as in

{$$}
\phi = \frac{1+\sqrt{5}}{2}.
{/$$}
```

\noindent This syntax is included only for compatibility with other systems (particularly Leanpub Markdown); Softcover also supports the proper \LaTeX\ syntax (Section~\ref{sec:embedded_math}), which is strongly preferred.


## Embedded \LaTeX
\label{sec:embedded_latex}

Short of using raw \PolyTeX\ (Chapter~\ref{cha:polytex_tutorial}), the most advanced typesetting options supported by Softcover involve embedding \LaTeX\ code directly in Markdown. As noted in the introduction to this chapter, this allows us to typeset things like "\texttt{typewriter text} \textsc{is different from} `code`", which in embedded \LaTeX\ appears as follows:

```latex
"\texttt{typewriter text} \textsc{is different from} `code`"
```

\noindent This uses \verb+\texttt+ (read "text-tee-tee") to set \texttt{typewriter text} and \linebreak \verb+\textsc+ to set \textsc{small caps}.

Not all of \LaTeX\ is supported, of course. The embeddable subset consists of single commands such as \verb+\texttt+ and \verb+\label+ (Section~\ref{sec:embedded_latex_commands} and Section~\ref{sec:embedded_labels_and_cross_references}), tables (Section~\ref{sec:embedded_tabular_and_tables}), figures (Section~\ref{sec:embedded_figures}), code listings (Section~\ref{sec:embedded_code_listings}), aside boxes (Section~\ref{sec:embedded_asides}), and mathematics (Section~\ref{sec:embedded_math}). That's still a lot, though, and experienced Markdown users new to \LaTeX\ will be amazed at all the things it can do.

Incidentally, in addition to the commands mentioned above, Softcover also supports \LaTeX's syntax for en-dashes using two dashes (\verb+--+), as in "1740--1780", and em-dashes---like this---using three dashes (\verb+---+):

```latex
as in "1740--1780", and em-dashes---like this---using
```

### \LaTeX\ commands
\label{sec:embedded_latex_commands}

In order to use embedded \LaTeX, we need a crash course on \LaTeX\ syntax. Luckily, the basics are not complicated. All \LaTeX\ commands start with a backslash `\`, and typically take 0 or 1 arguments inside curly braces. For example, we saw above how to typeset \texttt{typewriter text}:
```latex
we saw above how to typeset \texttt{typewriter text}
```
\noindent Here \verb+\texttt+ is the command and `typewriter text` is the argument.[^nesting_caveat]

The \LaTeX\ command itself is an example of a command taking no arguments:
```latex
The \LaTeX\ command itself
```
\noindent Because of how \LaTeX\ processes text, any space *after* a command gets "eaten", so here we've used the special "backslash space" command `\ ` to insert a space after the \verb+\LaTeX+ command.

We mentioned \verb+\noindent+, another command with zero arguments, back at the end of Section~\ref{sec:code_fencing}; when producing PDFs, it prevents indenting lines after things like code blocks:
```text
The \LaTeX\ command itself is an example of a command taking no arguments:
```latex
The \LaTeX\ command itself
```
\noindent Because of how \LaTeX\ processes text
```

\LaTeX\ also supports various *environments*, which are defined by the special `begin` and `end` commands. For example, we'll see in Section~\ref{sec:embedded_tabular_and_tables} that tables are set using the `tabular` environment as follows:
```latex
\begin{tabular}
.
.
.
\end{tabular}
```
\noindent Similarly, in Section~\ref{sec:embedded_math} we'll see how to typeset numbered equations using the `equation` environment:
```latex
\begin{equation}
<math>
\end{equation}
```

SFM also natively supports any command that doesn't require special formatting between \verb+\begin+ and \verb+\end+, such as the `quote` environment:
\begin{quote}
Il semble que la perfection soit atteinte non
quand il n'y a plus rien \`{a} ajouter,
mais quand il n'y a plus rien \`{a} retrancher.

---Antoine de Saint-Exup\'{e}ry, \emph{Terre des hommes}
\end{quote}
This is typeset as in Listing~\ref{code:latex_quote}.


\begin{codelisting}
\label{code:latex_quote}
\codecaption{Typesetting an embedded \texttt{quote} environment. Compare with Listing~\ref{code:blockquote}.}
```latex
\begin{quote}
Il semble que la perfection soit atteinte non
quand il n'y a plus rien \`{a} ajouter,
mais quand il n'y a plus rien \`{a} retrancher.

---Antoine de Saint-Exup\'{e}ry, \emph{Terre des hommes}
\end{quote}
```
\end{codelisting}


Listing~\ref{code:latex_quote} uses the native \LaTeX\ commands for typesetting em-dashes \linebreak (\verb+---+) and foreign accents (as in \verb+\`{a}+ for the grave accent \`{a} and \verb+\'{e}+ for the acute accent \'{e}), but as we saw in Listing~\ref{code:blockquote} SFM also supports the raw Unicode characters (i.e., —, à, and é).


Finally, here's [one weird trick](http://www.slate.com/articles/business/moneybox/2013/07/how_one_weird_trick_conquered_the_internet_what_happens_when_you_click_on.html) for including literal commands inside a line using the \verb+\verb+ command, as in "\verb+\LaTeX+":
```latex
as in "\verb+\LaTeX+"
```
The \verb+\verb+ command is unusual in that it doesn't formally take any arguments, but rather is followed by literal text surround by any two identical characters. The usual convention is to use plus signs, as in \verb+\texttt+, but other characters like exclamation points also work, as in \verb!\textsc!:

```latex
The usual convention is to use plus signs, as in \verb+\texttt+, but other
characters like exclamation points also work, as in \verb!\textsc!
```

### Labels and cross-references
\label{sec:embedded_labels_and_cross_references}

One of the biggest advantages of using embedded \LaTeX\ is being able to use *cross-references* to tie together the structure of the document. Cross-references consist of pairing a *label* (\verb+\label+) with a *reference* (\verb+\ref+).

For example, at the beginning of this chapter is a label appearing immediately after the chapter indicator:

```latex, options: "hl_lines": [2]
# Softcover-flavored Markdown
\label{cha:softcover_flavored_markdown}
```

\noindent This allows the definition of a cross-reference using the \verb+\ref+ command, \linebreak whose argument is the label name. Thus,

```latex, options: "hl_lines": [1]
Chapter~\ref{cha:introduction_to_markdown}
```

\noindent produces "Chapter~\ref{cha:introduction_to_markdown}". Similarly, the beginning of this section has

```latex, options: "hl_lines": [2]
## Embedded \LaTeX
\label{sec:embedded_latex}
```

\noindent so that

```latex, options: "hl_lines": [1]
Section~\ref{sec:embedded_latex}
```

\noindent produces "Section~\ref{sec:embedded_latex}". These cross-references are clickable links across all output formats (HTML, EPUB, MOBI, and PDF).

Incidentally, the tilde
```latex
~
```
\noindent is \LaTeX's no-break space, which in
```latex
Section~\ref{sec:embedded_latex}
```
\noindent connects the number to the word preceding it. This common typesetting convention prevents the number breaking across a line. Such cross-references are the only special use of tildes in SFM, and ordinarily a tilde appears as follows: ~. To get a literal tilde, use the \LaTeX\ command \verb+\textasciitilde+:
```
\textasciitilde
```
This yields \textasciitilde, and is especially useful if you want to put a tilde in an inline code environment, as in `cd \textasciitilde`.

In addition to working with chapters and sections, Softcover cross-ref\-er\-en\-ces also work with code listings, aside boxes, figures, tables, and centered equations. The label names can be virtually anything, but I follow the common convention of [namespacing](https://en.wikipedia.org/wiki/Namespace) them by type, so that chapter labels are prefixed with `cha:`, sections with `sec:`, codelistings with `code:`, etc.

In the context of book cross-references, the advantage of using named labels instead of hard-coded numbers can hardly be over-stated: it means that if you add a new chapter to the beginning of your book, all the subsequent cross-references will automatically be renumbered. There is simply no way an author could keep track of more than a few cross-references by hand, but with Softcover the computer does the heavy lifting so that you can use as many as you want.

I am a strong advocate of extensive cross-referencing, and not only because of their obvious benefits to readers. Cross-references are extraordinarily useful for *authors* as well: they let you immediately orient yourself when picking up after leaving off or going back later to edit. They are also useful when deferring material to the future, as undefined cross-references are helpful reminders to fill in the material later. I like to say that *cross-references are the connective tissue in the body of a book*.

### Tabular and tables
\label{sec:embedded_tabular_and_tables}

We saw in Section~\ref{sec:kramdown_tables} that Softcover supports tables via a literal-minded kramdown syntax, as in

```
| A simple | table |
| with multiple | lines|
```

\noindent Softcover also supports more powerful \LaTeX\ tables via the `tabular` environment:[^all_your_base]

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

- **Line 1** shows that `begin` in the `tabular` environment takes *two* arguments. The first argument simply identifies it as a `tabular` environment, while the second, `|r|lc|` defines the alignment of each column (`r` for "right", `l` for "left", and `c` for "center") and the presence or absence of vertical borders between them (borders everywhere except between the second and third columns).

- **Lines 2, 6, 8, and 10** use the \verb+\hline+ command a horizontal line between rows using the.

- **Lines 3--5 and line 7** include three entries each, one for each column. Each cell is separated by an ampersand character `&`, and each line is ended with a double-backslash \verb+\\+.

- **Line 9** shows a \verb+\multicolumn+ command to create a row that spans three columns. It takes three arguments: the number of columns (3), the alignment and vertical borders (`|c|`, or centered with borders on either side) and the contents ([AYBABTU](https://en.wikipedia.org/wiki/All_your_base_are_belong_to_us)).

In addition to the `tabular` environment, Softcover also supports the similarly named `table` environment, which produces a "float" that in a print or PDF document will be placed automatically by \TeX's float-placement algorithms. A `table` environment is typically used with a caption and a label (which should be placed *inside* the caption), which yields numbered, cross-referenced tables, as seen in Table~\ref{table:answer}. The code to produce Table~\ref{table:answer} appears in Listing~\ref{code:answer_table}. Note that because of how tables are processed, it is important that the `caption` contents appear in a single line of text; it's fine if the line wraps in your text editor, but it should contain no newlines.

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
\codecaption{The code to produce Table~\ref{table:answer}.}
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

#### Wrapping long lines
\label{sec:wrapping_long_table_lines}

Sometimes text in a table cell will be too long for the line. In HTML, this text gets wrapped automatically, but to get the line to wrap in the PDF as well you have to use the \verb+\pbox+ ("paragraph box") command. The \verb+\pbox+ command takes two arguments, the width of the box (typically in centimeters) and the text itself:

```latex
\pbox{9cm}{Lorem ipsum...}
```

\noindent To find a good width, make a guess and the build the PDF to see how it looks, iterating as necessary.

An example of a table with a \verb+\pbox+ command appears in Table~\ref{table:long_line}, with the corresponding code as in Listing~\ref{code:long_line}.

\begin{table}
\caption{A table with a long line.\label{table:long_line}}
\begin{tabular}{c|l}
  \textbf{Source} & \textbf{Text} \\ \hline
  Seneca & \emph{Docendo discimus.} \\
  Cicero (fragment) & \pbox{9cm}{\emph{Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod
  tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam,
  quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo
  consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse
  cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non
  proident, sunt in culpa qui officia deserunt mollit anim id est laborum.}}
\end{tabular}
\end{table}

\begin{codelisting}
\label{code:long_line}
\codecaption{Source for Table~\ref{table:long_line} (long line truncated).}
```latex, options: "hl_lines": [6]
\begin{table}
\caption{A table with a long line.\label{table:long_line}}
\begin{tabular}{c|l}
  \textbf{Source} & \textbf{Text} \\ \hline
  Seneca & \emph{Docendo discimus.} \\
  Cicero (fragment) & \pbox{9cm}{\emph{Lorem ipsum...}}
\end{tabular}
\end{table}
```
\end{codelisting}

### Figures
\label{sec:embedded_figures}

We saw in Section~\ref{sec:links_and_images} how to include raw images into Softcover books:

![Some dude.](images/figures/01_michael_hartl_headshot.jpg)

As a reminder, this is produced using the following code:
```text
![Some dude.](images/figures/01_michael_hartl_headshot.jpg)
```

Softcover also allows authors to make *numbered figures*, including captions, by including a label in the image's bracketed text. Figure~\ref{fig:michael_hartl} shows the result, which is produced by Listing~\ref{code:figure_with_caption}.

![Some dude.\label{fig:michael_hartl}](images/figures/01_michael_hartl_headshot.jpg)

\begin{codelisting}
\label{code:figure_with_caption}
\codecaption{The code to produce Figure~\ref{fig:michael_hartl}.}
```latex
![Some dude.\label{fig:michael_hartl}](images/figures/01_michael_hartl_headshot.jpg)
```
\end{codelisting}


Using a bare label with no caption text yields a figure with just a number (Figure~\ref{fig:road_to_hana} and Listing~\ref{code:figure_only_number}). Note that in both cases the labels are namespaced with `fig:`.


![\label{fig:road_to_hana}](images/figures/01_michael_hartl_headshot.jpg)


\begin{codelisting}
\label{code:figure_only_number}
\codecaption{The code to produce Figure~\ref{fig:road_to_hana}.}
```latex
![\label{fig:road_to_hana}](images/figures/01_michael_hartl_headshot.jpg)
```
\end{codelisting}

#### Placement
\label{sec:placement}

In HTML, and thus in EPUB and MOBI, images and figures are placed exactly where they appear in the book source, but in PDFs this is true only of raw images. This is because PDFs are bound by the constraints of print documents, so figure placement in general must be allowed to "float" in order to achieve a sensible layout for the surrounding text. (As with tables (Section~\ref{sec:embedded_tabular_and_tables}), figures are thus often described as "floats".) By default, figures in PDFs are placed using \LaTeX's float-placement algorithms, which sometimes leads to figures not being located where you want them to be. Unfortunately, there is no Markdown syntax for overriding \LaTeX's defaults, but authors desiring finer-grained control can use embedded \LaTeX\ to use more advanced float-placement options, as described in Section~\ref{sec:advanced_figure_placement}.


### Code listings
\label{sec:embedded_code_listings}

In addition to syntax-highlighted source code, Softcover also supports code *listings*, which are numbered code blocks with optional captions. For example,

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
```latex, options: "hl_lines": [3]
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

Obviously, making code listings involves lots of typing, so I recommend you use macros. If there is sufficient demand, I plan to release my macros for Sublime Text, and Mark Bates has released [Softcover Vim snippets](https://gist.github.com/markbates/2c2e6d37dd98c43e6d7e) as well.

### Aside boxes
\label{sec:embedded_asides}

In the course of writing a long document like a book, you may find that you want to make an *aside*, i.e., a digression that covers some ancillary topic in more depth. In order to prevent breaking up the narrative, Softcover supports an *aside box environment* suitable for cross-referencing. We've seen several examples so far in this manual, most recently in Box~\ref{aside:polytex_markdown}. The code to produce that aside appears in Listing~\ref{code:polytex_markdown_aside_code}, and the code to produce the reference appears in Listing~\ref{code:polytex_markdown_ref_code}.

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
\label{sec:embedded_math}

We saw in Section~\ref{sec:terrible_math} that Softcover supports embedded mathematics using the ugly `{\$\$\}...\{/\$\$\}` syntax, but I strongly recommend you use the proper \LaTeX\ syntax instead. For inline math, this means using \LaTeX's native "backslash parenthesis" notation, as in \( \phi^2 - \phi - 1 = 0 \), which is set as follows:

```latex
as in \( \phi^2 - \phi - 1 = 0 \), which is set as follows
```

\noindent Some authors may be aware of the pithier \TeX-style dollar-sign notation, which sets inline math using notation like `$x$`. In order to avoid confusion with ordinary dollar signs, this is not supported by Markdown input, but it is supported by \PolyTeX, so power users who want to be able to write

```latex
$x$
```

\noindent instead of

```latex
\( x \)
```

\noindent should use raw \PolyTeX\ instead (Chapter~\ref{cha:polytex_tutorial}).


\noindent Softcover also supports centered math, as follows:
\[ \phi^2 - \phi - 1 = 0. \]
This equation is set using \LaTeX's native "backslash square bracket" notation:


```latex
\[ \phi^2 - \phi - 1 = 0. \]
```

\noindent As with inline math, \TeX\ provides an alternate dollar-sign syntax, in this case using `$$...$$` for centered math. As with single dollar signs, this notation is not supported Markdown input but it supported by \PolyTeX, so power users who want to be able to write

```latex
$$ \phi^2 - \phi - 1 = 0. $$
```

\noindent instead of

```latex
\[ \phi^2 - \phi - 1 = 0. \]
```

\noindent should use raw \PolyTeX\ instead. (I actually prefer \TeX-style dollar signs for inline math but \LaTeX-style backslash square brackets for centered math.)

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


### Colored text
\label{sec:markdown_colored_text}

Via the \verb+\coloredtext+ and \verb+\coloredtexthtml+ commands, Softcover supports including \coloredtext{red}{colored} \coloredtexthtml{E8AB3A}{text} across all output formats:

```latex
Softcover supports \coloredtext{red}{colored} \coloredtexthtml{E8AB3A}{text}
```

\noindent See Section~\ref{sec:colored_text} for more details.

<!-- ## Switching from Markdown to \PolyTeX
\label{sec:markdown_to_polytex}

It's possible to switch over at any time, can be done on a chapter-by-chapter basis.

Just move the generated \PolyTeX\ file from the `generated_polytex` directory into the `chapters/` directory and edit the `Book.txt` file. Once you're confident you really want to make the switch, remove the original Markdown file. (If you're using Git for version control, which I *strongly* recommend, you can use `git rm` to remove the file. It will then be available in your history should you ever get cold feet and want to go back to Markdown.) -->


### Inputting contents of other files
\label{sec:input}

\LaTeX's \verb+\input+ command inputs the contents of a external file into the current file:

```latex
This includes the contents of example.tex:

\input{example}
```

\noindent In this example, \verb+\input{example}+ automatically includes the contents of `example.tex` into the current file; i.e., \LaTeX\ infers the `.tex` filename extension. This is fine when writing \PolyTeX\ documents (chapter~\ref{cha:polytex_tutorial}), but it doesn't work for including Markdown documents. To fix this, Softcover overrides the default behavior of \verb+\input+ so that, in Markdown documents, the code

```latex
\input{chapters/example}
```

\noindent causes `chapters/example.md` to be included into the current file. This makes it possible to break long chapters into smaller pieces and then assemble them using repeated invocations of \verb+\input+:

```latex
# Example chapter

Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod
tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam,
quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo
consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse
cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non
proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

\input{chapters/a_section}
\input{chapters/another_section}
\input{chapters/yet_another_section}
```

 \noindent Note also that \verb+\input+ is recursive, so the included documents can themselves use \verb+\input+, and so on *ad infinitum*.

<!-- footnotes  -->

[^latex_polytex]: This manual uses "\PolyTeX" when the distinction with \LaTeX\ is important and "\LaTeX" otherwise.

[^example_footnote]: This is an example footnote.

[^eq_epub_mobi]: The real challenge is producing EPUB and MOBI output. The trick is to (1) create a self-contained HTML page with embedded math, (2) include the amazing [MathJax](http://www.mathjax.org/) JavaScript library, configured to render math as [SVG](https://en.wikipedia.org/wiki/Scalable_Vector_Graphics) images, (3) hit the page with the headless [PhantomJS](http://phantomjs.org/) browser to force MathJax to render the math (including any equation numbers) as SVGs, (4) extract self-contained SVGs from the rendered pages, and (5) use [Inkscape](http://www.inkscape.org/) to convert the SVGs to PNGs for inclusion in EPUB and MOBI books. Easy, right? In fact, no---it was excruciating and required excessive amounts of profanity to achieve. But it's done, so ha.

[^nesting_caveat]: Because of the way Softcover processes text, *nested* commands won't work in Markdown, but they *will* work in raw \PolyTeX.

[^all_your_base]: The phrase "All your base are belong to us [sic]" is a broken English phrase from the video game "Zero Wing" that has become an [Internet meme](http://en.wikipedia.org/wiki/All_your_base_are_belong_to_us).