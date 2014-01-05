# Softcover-flavored Markdown
\label{cha:softcover_markdown}

Softcover therefore supports a *superset* of vanilla Markdown, including the [*kramdown*](http://kramdown.gettalong.org/) extensions, GitHub-style [*fenced code blocks*](https://help.github.com/articles/github-flavored-markdown#fenced-code-blocks), and embedded \LaTeX\@.

The Softcover dialect of Markdown is, to our knowledge, the most powerful one available, with support for figures, tables, code listings, and mathematical equations (all with numbered, linked cross-references).



Softcover-flavored Markdown derives much of its power by converting Markdown first to \PolyTeX, a strict subset of the \LaTeX\ typesetting language (Section~\ref{sec:softcover_system}), and then from \PolyTeX\ to HTML, EPUB, MOBI, and PDF\@. The result is an [abstraction layer](https://en.wikipedia.org/wiki/Abstraction_layer) over the underlying \LaTeX;[^latex_polytex] by allowing *embedded* \LaTeX\ as well, Softcover lets users pierce this abstraction layer and typeset things impossible for vanilla Markdown---for example, "Hackers in Hawai`i write \kode{'Aloha, world!'}".

The resulting hybrid input language, though powerful, can get a bit messy, and
adding features to Markdown as described above is a prime example of how weak systems tend to evolve toward strong ones in an [*ad hoc*](http://en.wikipedia.org/wiki/Ad_hoc) way (Box~\ref{aside:polytex_markdown}). Users who want a consistent input syntax with maximum control can dispense with the abstraction layer and write in raw \PolyTeX\ instead (Chapter~\ref{cha:polytex_tutorial}). Indeed, because the Markdown conversion runs through the \PolyTeX\ pipeline, it's possible to start with Markdown and change over to \PolyTeX\ at any time (Section~\ref{sec:markdown_to_polytex}). As noted in [XKCD 1301](http://www.xkcd.com/1301/), the resulting \texttt{.tex} files have the highest level of trustworthiness of any format (Figure~\ref{fig:xkcd_extensions}).

\begin{aside}
\label{aside:polytex_markdown}
\heading{\PolyTeX, Markdown, and Hartl's Tenth Rule of Typesetting}

Markdown succeeds spectacularly well in achieving its goals, and I have been an enthusiastic user of Markdown since shortly after it debuted. Indeed, in a sense it has succeeded a little *too* well, to the point where people use it even when it's not the best tool for the job.

In the spirit of [Greenspun's Tenth Rule of Programming](https://en.wikipedia.org/wiki/Greenspun's_tenth_rule), I offer the following rule:

> **Hartl's Tenth Rule of Typesetting**\\
> *Any sufficiently complicated typesetting system contains an ad hoc,
> informally specified, bug-ridden, slow implementation of half of \LaTeX.*

\LaTeX\ is really two things: a typesetting language and a system for converting that language into print-quality documents (PostScript, PDF, etc.) Of course, even \PolyTeXnic\ can't fully escape Hartl's Tenth Rule, since using  \LaTeX\ input to produce HTML output requires writing just such an implementation of half of \LaTeX. (Well, we do our best to make it fast and avoid bugs\ldots) But by using \PolyTeX\ we at least avoid creating an ad hoc, informally specified \emph{input} language as well. Indeed, looking at the ever-expanding definition of ``Markdown''---as kramdown, Leanpub, GitHub, etc., add non-Markdown features to a Markdown base---we see the familiar pattern of Hartl's Tenth Rule emerge.

\PolyTeXnic\ authors are, of course, welcome to use Markdown++, but anyone who knows Markdown or HTML can learn enough \PolyTeX\ to be productive in only a few minutes. And given that Markdown+ extends vanilla Markdown in dozens of ways, you're going to have to learn something new no matter which way you go. My view is that you might as well learn a real typesetting language that has, e.g., a \href{http://tex.stackexchange.com/}{real Stack Exchange community} associated with it, but I'm perfectly prepared for the possibility that some variant of Markdown will eventually win. Since it's easy to support both, \PolyTeXnic\ does.

It's true that the \LaTeX\ commands for typesetting mathematics can be complicated and even a little scary---but if you want to typeset mathematics, you need to learn \LaTeX\ anyway:
\[ \left(-\frac{\hbar^2}{2m}\nabla^2 + V\right)\psi = E\psi \]
\[
\left(\frac{p}{q}\right) \left(\frac{q}{p}\right) = (-1)^{[(p-1)/2][(q-1)/2]} \quad\text{($p$, $q$ distinct odd primes)}
\]

\end{aside}

![\label{fig:xkcd_extensions}](images/figures/file_extensions.png)

## Advanced enhancements
\label{sec:code_fencing_kramdown}

### GitHub-flavored fenced code blocks

Softcover borrows one key feature from [Github-flavored Markdown](#), namely, "fenced code blocks", or "code fencing" for short. This syntax involves placing code samples inside "fences" composed of three backticks (\verb+```+):

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

Following GitHub's example, Softcover supports an optional string after the opening of the fence indicating the language of the sample, yielding language-specific syntax highlighting:

```ruby
# "Hello, world!" in Ruby
def hello
  puts "hello, world!"
end
```

\noindent This is produced by the following Markdown:

    ```ruby
    # "Hello, world!" in Ruby
    def hello
      puts "hello, world!"
    end
    ```

\noindent The language designation (e.g., `ruby`) can be any language supported by the [available Pygments lexers](http://pygments.org/docs/lexers/).

As a final enhancement, Softcover adds a hook directly into the [Pygments formatter options](http://pygments.org/docs/formatters/), allowing, for example, turning on line numbering and highlighting specific lines:

```ruby, options: "linenos": true, "hl_lines": [1, 3]
# "Hello, world!" in Ruby
def hello
  puts "hello, world!"
end
```

\noindent This is produced by the following Markdown:

    ```ruby, options: "linenos": true, "hl_lines": [1, 3]
    # "Hello, world!" in Ruby
    def hello
      puts "hello, world!"
    end
    ```

\noindent Here the hash

```
options: "linenos": true, "hl_lines": [1, 3]
```

\noindent gets passed directly to Pygments, so any option listed on the [Pygments formatter options page](http://pygments.org/docs/formatters/) is automatically supported by Softcover.

### Code inclusion

Can include code

### The kramdown extensions

Includes kramdown: footnotes, tables, etc.


## Embedded \LaTeX
\label{sec:embedded_latex}

Most advanced option: select \LaTeX\ embedding. Leads to gotchas

### Labels and cross-references

Can define labels and cross-references

### Code listings

Can make code listings

### Aside boxes

Can make aside boxes

### Miscellaneous formatting

Can use verbatim, typewriter text, etc.

