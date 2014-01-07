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

I've been a fan of Markdown since it first appeared in 2004, and its high adoption rates since then show how well it fits its intended role of being a friendly skin on raw HTML. Indeed, in a sense it has succeeded a little *too* well, to the point where people use it even when it may not be the best tool for the job. The result is that virtually every system out there using "Markdown" in reality uses some augemented version of the original markup language---an implicit acknowledgment that vanilla Markdown is insufficient for industrial-strength typesetting.

On the other end of the spectrum from Markdown is \LaTeX, an industrial-strength typesetting system if ever there was one. \LaTeX, like Lisp in its domain, can essentially "do anything", so in the spirit of [Greenspun's Tenth Rule of Programming](https://en.wikipedia.org/wiki/Greenspun's_tenth_rule) I offer the following maxim:

> **Hartl's Tenth Rule of Typesetting**\\
> *Any sufficiently complicated typesetting system contains an ad hoc,
> informally specified, bug-ridden, slow implementation of half of \LaTeX.*

This suggests at least the possibility of *actually using \LaTeX* instead of using "Markdown on steroids". We do have to make concessions when making multi-format ebooks, especially since the popular EPUB & MOBI formats are based on HTML---there's simply no general mapping from \LaTeX\ to HTML. But what we *can* do is support a *subset* of \LaTeX\ that maps nicely to HTML (and thence to EPUB & MOBI). The result is a version of \LaTeX\ that supports polymorphic output---thus, *\PolyTeX*.

Of course, even \PolyTeX\ can't fully escape Hartl's Tenth Rule, since producing HTML output from \LaTeX\ requires writing just such an implementation of half of \LaTeX. (Well, we do our best to make it fast and avoid bugs\ldots) But by using \PolyTeX\ we at least avoid creating an ad hoc, informally specified *input* language as well. Indeed, looking at the ever-expanding definition of "Markdown"---from GitHub to Leanpub to Softcover itself---we see the familiar pattern of Hartl's Tenth Rule emerge.

Authors who hunger for more power and flexibility in their typesetting system are thus invited to try \PolyTeX. Anyone who knows Markdown or HTML can learn enough \PolyTeX\ to be productive in only a few minutes, and the ceiling on what you can acheive is much higher. If your curiosity is piqued, Chapter~\ref{cha:polytex_tutorial} will help get you started.

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

