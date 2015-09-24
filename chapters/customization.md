# Customization and advanced options
\label{cha:customization}

Softcover includes a large number of advanced options such as CLI customization, user-defined styling and typesetting commands, and foreign-language \linebreak support. Many of these options are in active development, so I recommend requesting an invitation to the [Softcover Google Group](https://groups.google.com/forum/#!forum/softcover-publishing) to get the inside track on their status.

## Command-line interface
\label{sec:CLI}

Two of the most important commands in the Softcover CLI are `build:all` and `deploy`. Their default behavior is sufficient for most purposes, but some authors will want to customize them for their specific needs. Softcover allows such customization via [dot-files](https://en.wikipedia.org/wiki/Dot-file) in the book's root directory, as described below.

### Customizing builds
\label{sec:customizing_builds}

By default, `softcover build:all` generates HTML, EPUB, MOBI, and PDF files, but it's easy to customize. All you need to do is edit the file \linebreak `.softcover-build` in the book's root directory (Listing~\ref{code:build_config}).

\begin{codelisting}
\label{code:build_config}
\codecaption{The default build configuration file. \\ \filepath{\$ROOT\_DIRECTORY/.softcover-build}}
```text
# Edit this file to customize your build steps with custom command options
# or additional commands.
#
# softcover build:pdf
# softcover build:mobi
# softcover build:preview
```
\end{codelisting}

For example, if you want to use KindleGen in place of Calibre \linebreak (Section~\ref{sec:build_mobi}) and build previews (Section~\ref{sec:build_previews}) by default, you can use the `.softcover-build` file shown in Listing~\ref{code:build_preview_kindlegen}. This uncomments the lines in Listing~\ref{code:build_config} and adds the \verb+--kindlegen+ flag to the `build:mobi` command. (Note that Listing~\ref{code:build_preview_kindlegen} omits `softcover build:epub` because EPUB files are generated automatically as a side-effect of building MOBI.)

\begin{codelisting}
\label{code:build_preview_kindlegen}
\codecaption{Using KindleGen and building previews. \\ \filepath{\$ROOT\_DIRECTORY/.softcover-build}}
```text, options: "hl_lines": [5, 6]
# Edit this file to customize your build steps with custom command options
# or additional commands.
#
softcover build:pdf
softcover build:mobi --kindlegen
softcover build:preview
```
\end{codelisting}


### Customizing deploys
\label{sec:customizing_deploys}

You can customize the behavior of `softcover deploy` by editing the
file `.softcover-deploy` in the project's root directory (Listing~\ref{code:deploy_config}).

\begin{codelisting}
\label{code:deploy_config}
\codecaption{The default deployment configuration file. \\ \filepath{\$ROOT\_DIRECTORY/.softcover-deploy}}
```text
# Edit this file to customize your deployment steps with custom command options
# or additional commands.
#
# softcover build:all
# softcover build:preview
# softcover publish
```
\end{codelisting}

For example, Listing~\ref{code:deploy_no_preview} removes the `softcover build:preview` command while adding `git push origin` to sync the local copy with a remote Git repository.

\begin{codelisting}
\label{code:deploy_no_preview}
\codecaption{Removing previews and adding a Git push. \\ \filepath{\$ROOT\_DIRECTORY/.softcover-deploy}}
```text, options: "hl_lines": [6]
# Edit this file to customize your deployment steps with custom command options
# or additional commands.
#
softcover build:all
softcover publish
git push origin
```
\end{codelisting}


## Commands and styles
\label{sec:commands_and_styles}

Softcover books are based on \LaTeX\ (for PDF) and HTML (for EPUB and MOBI), both of which allow for extensive customization via style files and CSS, respectively. There is even one point of overlap: commands defined in `latex_styles/custom.sty` are available across all output formats (Section~\ref{sec:custom_commands}).


### Custom commands
\label{sec:custom_commands}

\LaTeX\ natively supports custom commands using `newcommand`, and Softcover adds support for `newcommand` to HTML (and thus EPUB/MOBI) as well. The default customization file has examples to get you started (Listing~\ref{code:default_custom_sty}). As indicated in the comment at the top of Listing~\ref{code:default_custom_sty}, commands that only make sense for PDF output---such as hyphenation definitions or specific \LaTeX\ package includes---should be included in `custom_pdf.sty` instead of `custom.sty` (Section~\ref{sec:pdf_style}).

\begin{codelisting}
\label{code:default_custom_sty}
\codecaption{The default customization file. \\ \filepath{latex\_styles/custom.sty}}
<<(example_book/latex_styles/custom.sty, lang: latex)
\end{codelisting}

For example, looking at the sample in Listing~\ref{code:default_custom_sty}, we see how to define the command \verb+\bfi+ to take one argument and set it in \bfi{boldface italic}, as shown in Listing~\ref{code:bfi}.

\begin{codelisting}
\label{code:bfi}
\codecaption{Defining a \bfi{boldface italic} command. \\ \filepath{latex\_styles/custom.sty}}
```latex
\newcommand{\bfi}[1]{\textbf{\textit{{#1}}}}
```
\end{codelisting}

Because the `custom.sty` file is raw \LaTeX, definitions must be in \PolyTeX\ (Chapter~\ref{cha:polytex_tutorial}) and not Markdown. This isn't as hard as you think, though, and \LaTeX\ commands are highly Googleable. Try, for example, the search "[latex boldface italic](http://lmgtfy.com/?q=latex+boldface+italic)" to see how you might have guessed the definition of \verb+\bfi+ without my help.

*Note*: Custom *math* commands are supported locally but are not currently supported on the Softcover site. We're planning to add it as soon as someone wants it, though, so send a request to \texttt{michael@softcover.io} and we'll get right on it.

### HTML style
\label{sec:html_style}

You can customize Softcover's HTML styles using the `custom.css` file in the `html/stylesheets` directory. As noted in the comment shown in Listing~\ref{code:custom_css}, the only caveat is that the CSS rules have to be scoped by the `#book` id.

\begin{codelisting}
\label{code:custom_css}
\codecaption{The generated custom CSS file. \\ \filepath{html/stylesheets/custom.css}}
<<(example_book/html/stylesheets/custom.css)
\end{codelisting}

*Note*: Custom CSS is supported locally but is not currently not supported on the Softcover site. We're planning to add it as soon as someone wants it, though, so send a request to \texttt{michael@softcover.io} and we'll get right on it.


### EPUB/MOBI style
\label{sec:epub_mobi_style}

Any changes you make in `custom.css` (Section~\ref{sec:html_style}) will automatically be incorporated into the EPUB and MOBI styles as well, but sometimes you'll want additional customization that applies *just* to EPUB and MOBI books. The file `custom_epub.css` allows for this extra layer of detail (Listing~\ref{code:custom_epub_css}). (Its somewhat strange-looking directory location is set by the EPUB standard.)

\begin{codelisting}
\label{code:custom_epub_css}
\codecaption{The generated custom CSS file for EPUB/MOBI. \\ \filepath{epub/OEBPS/styles/custom\_epub.css}}
<<(epub/OEBPS/styles/custom_epub.css)
\end{codelisting}

In earlier versions of Softcover, the entire `epub/` directory was ignored by default, so Git users may have to add the custom CSS file by hand:

```console
$ git add --force epub/OEBPS/styles/custom_epub.css
```

### PDF style
\label{sec:pdf_style}

You can customize the PDF styles in two different ways. The first and simpler is to edit the file `latex_styles/custom_pdf.sty`, whose default content is shown in Listing~\ref{code:custom_pdf_style}. This file gets included *last*, so any rules in `custom_\-pdf.sty` will override the defaults. Common uses for the custom PDF style file include defining hyphenation rules for words \LaTeX\ can't hyphenate natively and adding support for any Unicode characters not supported by the PDF typesetting engine (xelatex). For example, uncommenting the code in Listing~\ref{code:custom_pdf_style} adds the rule for hyphenating "JavaScript" and adds PDF support for the Unicode characters ★ and ž (Listing~\ref{code:custom_pdf_style_uncommented}).

\begin{codelisting}
\label{code:custom_pdf_style}
\codecaption{The default custom PDF style file. \\ \filepath{latex\_styles/custom\_pdf.sty}}
<<(example_book/latex_styles/custom_pdf.sty, lang: latex)
\end{codelisting}

\begin{codelisting}
\label{code:custom_pdf_style_uncommented}
\codecaption{Uncommenting the default PDF style file. \\ \filepath{latex\_styles/custom\_pdf.sty}}
```latex
\hyphenation{Ja-va-Script}
\usepackage{newunicodechar}
\newunicodechar{★}{\ensuremath{\star}}
\newunicodechar{ž}{\v{z}}
```
\end{codelisting}

#### Changing paragraph styles

One use of `custom_pdf.sty` is important enough to deserve special mention, namely, changing the default paragraph indentation and spacing to match the style in HTML/-EPUB/-MOBI. In line with standard practices for professionally typeset books, the default behavior in PDF is for all paragraphs after the first one in a section to be indented. This indentation serves as a visual marker for paragraph boundaries, making them easier to parse visually. On the other hand, this convention requires suppressing indentation by hand (using a \verb+\noindent+) after elements like code blocks, as discussed at the end of Section~\ref{sec:code_fencing}. Without a \verb+\noindent+, any text after the code block gets interpreted as a new paragraph and is indented, which is often not what you want.

My preference is to make PDFs as close to traditional print-quality as possible, but some authors would rather unify the appearance of their ebooks and not have to worry about adding \verb+\noindent+ by hand. To arrange for this behavior, you need to tell \LaTeX\ not to indent paragraphs, while also increasing the vertical space *between* paragraphs so that they still stand out. The code to accomplish this appears in Listing~\ref{code:noindent_style}.

\begin{codelisting}
\label{code:noindent_style}
\codecaption{Removing paragraph indentation and adding vertical space. \\ \filepath{latex\_styles/custom\_pdf.sty}}
```latex
% You can also use this file to define commands that *only* pertain to the PDF.

% Distinguish paragraphs by vertical spacing instead of by indentation.
\setlength{\parindent}{0.0in}
\setlength{\parskip}{0.1in}
```
\end{codelisting}

The second method for customizing PDF output is to edit the file \linebreak `preamble.tex` in the `config` directory; the default contents appear in Listing~\ref{code:preamble_tex}. By editing this file, you can do things like change the PDF font size or include packages that don't work when included in `custom_pdf.sty`. For example, the default font size (`14pt`) is designed to look good on tablet devices such as iPad, but some authors may prefer the smaller fonts typically used for print publications (`10pt` or `12pt`). These smaller fonts use the default `book` class in place of the `extbook` class needed for 14pt fonts, as shown in Listing~\ref{code:preamble_tex_12_pt}.

\begin{codelisting}
\label{code:preamble_tex}
\codecaption{The preamble file for changing fonts, etc. \\ \filepath{config/preamble.tex}}
<<(example_book/config/preamble.tex)
\end{codelisting}

\begin{codelisting}
\label{code:preamble_tex_12_pt}
\codecaption{Setting the font size to 12pt. \\ \filepath{config/preamble.tex}}
```
\documentclass[12pt]{book}
```
\end{codelisting}


The `preamble.tex` file is especially important for foreign language support, which requires that the appropriate `polyglossia` package be included before the default `softcover.sty` file. See Section~\ref{sec:foreign_language} for details.

### Advanced figure placement
\label{sec:advanced_figure_placement}

As mentioned in Section~\ref{sec:placement}, figure placement in PDF documents is determined automatically by \LaTeX's float-placement algorithms. These often work well, but sometimes they result in placement different from that desired by the author. Using embedded \LaTeX, authors can override the default algorithms by passing options to the \kode{figure} command, as shown in Listing~\ref{code:figure_placement}.

\begin{codelisting}
\label{code:figure_placement}
\codecaption{A template for passing a figure placement specifier.}
```latex
\begin{figure}[placement specifier]
\image{images/figures/image.png}
\end{figure}
```
\end{codelisting}

\noindent Some common options for the placement specifier appear in Table~\ref{table:figure_placement}.

\begin{table}
\caption{Options for the placement specifier in Listing~\ref{code:figure_placement}.\label{table:figure_placement}}
\begin{tabular}{l|l}
\textbf{Specifier} & \textbf{Placement} \\ \hline
\kode{h} & Place the float \emph{approximately} here \\
\kode{h!} & Place the float \emph{(almost) exactly} here \\
\kode{H} & Place the float \emph{exactly} here (requires Listing~\ref{code:float_package}) \\
\kode{t} & Place at the top of the page \\
\kode{b} & Place at the bottom of the page \\
\kode{p} & Put on a special page for floats only
\end{tabular}
\end{table}

Listing~\ref{code:figure_placement_example} shows a concrete example of using the `H` option, whose result appears in Figure~\ref{fig:figure_placement_example}. As indicated in Table~\ref{table:figure_placement}, the \kode{H} option, which places the figure *exactly* "here", requires including the \kode{float} package to work. As described in Section~\ref{sec:pdf_style}, this can be accomplished by editing the configuration file \kode{custom\_pdf.sty}, as shown in Listing~\ref{code:float_package}.

\begin{codelisting}
\label{code:figure_placement_example}
\codecaption{A working example of figure placement.}
```
\begin{figure}[H]
\image{images/figures/michael_hartl.png}
\caption{An image placed ``here''.\label{fig:figure_placement_example}}
\end{figure}
```
\end{codelisting}


\begin{figure}[H]
\image{images/figures/2011_michael_hartl.png}
\caption{An image placed exactly ``here''.\label{fig:figure_placement_example}}
\end{figure}

\begin{codelisting}
\label{code:float_package}
\codecaption{Including the \kode{float} package to enable the \kode{H} option. \\ \filepath{config/custom\_pdf.sty}}
```
\include{float}
```
\end{codelisting}

## Foreign-language support
\label{sec:foreign_language}

Softcover has experimental support for foreign languages. Please request an invitation to the [Softcover Google Group](https://groups.google.com/forum/#!forum/softcover-publishing) and send us a note if you're interested in writing a Softcover book in a language other than English.

### Polyglossia and \texttt{lang.yml}
\label{sec:polyglossia}

Only two steps are required to support foreign languages across different output formats. For concreteness, we'll use French as an example. First, to enable French support in PDF, we need to edit `preamble.tex` to include the `polyglossia` package and set the default language to `french`, as shown in Listing~\ref{code:polyglossia_french}. (The final line in Listing~\ref{code:polyglossia_french} is needed to work around an error when building the PDF; although I've been using \LaTeX\ for years, I solved it the same way you would have: by Googling the error message.)

\begin{codelisting}
\label{code:polyglossia_french}
\codecaption{Setting the default language to French using \texttt{polyglossia}. \\ \filepath{config/preamble.tex}}
```latex
\documentclass[14pt]{extbook}
\usepackage{polyglossia}
\setdefaultlanguage{french}
\DeclareTextCommandDefault{\nobreakspace}{\leavevmode\nobreak\ }
```
\end{codelisting}

Second, to enable foreign language support in HTML/-EPUB/-MOBI, we need to edit the file `lang.yml` in the `config` directory to tell Softcover the names of the various elements (chapter, section, listing, etc.). The default (English) values are shown in Listing~\ref{code:default_lang_yml}, while the edited values for French are shown in Listing~\ref{code:french_lang_yml}.

\begin{codelisting}
\label{code:default_lang_yml}
\codecaption{The default language settings. \\ \filepath{config/lang.yml}}
<<(example_book/config/lang.yml, lang: yaml)
\end{codelisting}

\begin{codelisting}
\label{code:french_lang_yml}
\codecaption{French language settings. \\ \filepath{config/lang.yml}}
```yaml
---
chapter:
  word: Chapitre
  order: standard     # Use 'reverse' to change 'Chapter 1' to '1 Chapter'
section: Section
table: Table
figure: Figure
fig: Fig
aside: Notes
listing: Listing
equation: Équation
eq: Eq
frontmatter: Préliminaires
contents: Table des Matières
```
\end{codelisting}

With the settings as in Listing~\ref{code:french_lang_yml}, labels such as "Chapitre" for "Chapter" will be unified across output formats. In addition, cross-references will link to the full word in addition to the number, so that, e.g., the link "Notes 1.1" would include the word "Notes" as well as "1.1" (as in Box~\ref{aside:softcover_uses}).

### Terrifyingly advanced comments on Hungarian
\label{sec:hungarian}

The `order` option in Listing~\ref{code:default_lang_yml} is included to support languages such as Hungarian, in which the translation of "Chapter 1" appears as "1 fejezet". As indicated by the comment in Listing~\ref{code:default_lang_yml}, this can be arranged by setting `order: reverse` in `lang.yml` (Listing~\ref{code:magyar_lang_yml}). Ironically, the Polyglossia support for Hungarian gets the order wrong; a fix appears in Listing~\ref{code:magyar_preamble}.

\begin{codelisting}
\label{code:magyar_lang_yml}
\codecaption{Reversing chapter/number order to get `1 fejezet'. \\ \filepath{config/lang.yml}}
```latex
---
chapter:
  word: fejezet
  order: reverse     # Use 'reverse' to change 'Chapter 1' to '1 Chapter'
.
.
.

```
\end{codelisting}

\begin{codelisting}
\label{code:magyar_preamble}
\codecaption{Fixing chapter ordering for Hungarian (\texttt{magyar}). \\ \filepath{config/preamble.tex}}
```latex
\documentclass[14pt]{extbook}
\usepackage{polyglossia}
\setdefaultlanguage{magyar}
\usepackage{etoolbox}
\makeatletter
\patchcmd{\@makechapterhead}
  {\@chapapp\space \thechapter}
  {\thechapter\space \@chapapp}
  {}{}
\makeatother
\DeclareTextCommandDefault{\nobreakspace}{\leavevmode\nobreak\ }
```
\end{codelisting}

## Detailed refinements
\label{sec:detailed_refinements}

Once your book is nearing completion, Softcover helps you put on the final bits of polish. These include eliminating "overfull hboxes" (Section~\ref{sec:overfull_hboxes}), handling problems with labels and cross-references (Section~\ref{sec:labels_and_reference_problems}), and validating EPUB books (Section~\ref{sec:epub_validation}).

### Overfull hboxes
\label{sec:overfull_hboxes}

When building a PDF, you may notice that some of the error messages indicate an "overfull hbox". This happens when the line is too long for the page, such as when you include some `LongRubyClassName` that Softcover doesn't know how to hyphenate. Because they can mar the appearance of PDF books, Softcover comes with a utility to help you find them:[^overfull_code]

```console
$ softcover build:pdf --find-overfull
$ softcover build:pdf --find-overfull
```

\noindent (You need to run it twice initially to make sure the cross-reference are up-to-date, as this can affect the presence of overfull hboxes.) If the second command returns no results, it means that your book is 100% overfull hbox--free.

Unfortunately, because of the way \LaTeX\ processes files, the line numbers output by the error message aren't useful for tracking down the source of the overfull hbox. As a compromise, the \verb+--find-overfull+ flag gives some context around the problematic line, which should allow you to find the culprit using the search function in your text editor.

Once you find the source of each overfull hbox, you need to add markup to help the Softcover system break the line properly. This will typically involve telling the system how to hyphenate some word that's spilling into the right margin. For ordinary words such as "JavaScript", you can add custom hyphenation rules as in Section~\ref{sec:pdf_style}, but for inline code (and any words \LaTeX\ refuses to hyphenate, such as those already containing a hyphen) you'll need to add in hyphens by hand using the special \LaTeX\ syntax \verb+\-+. For example, to get Softcover to hyphenate `LongRubyClassName`, you could write the following:

```text
`Long\-Ruby\-Class\-Name`
```

\noindent These hyphens will be ignored unless needed to break the text across a line.

Sometimes the source of the problem isn't a long word, or it's a word you don't want to hyphenate, so you'll have to force a line break by hand. This is easy with the \verb+\linebreak+ command:

```latex
...for customizing PDF output is to edit the file \linebreak `preamble.tex`
```

When tracking down overfull hboxes, I recommend rebuilding with

```console
$ softcover build:pdf --find-overfull
```

\noindent after each change and checking the PDF to make sure it's fixed before moving on. This can be tedious, but it usually only needs to be done once at the end of the writing process.

*Note*: Sometimes it's virtually impossible to get all the linebreaks just right. In particularly recalcitrant cases, I've even been known to recast the sentence slightly to fix them. Often, such edits, though born of necessity, nevertheless manage to improve the prose.


### Problems with labels and cross-references
\label{sec:labels_and_reference_problems}

When building PDFs, the \LaTeX\ output will often warn you about undefined references and multiply defined labels. These are easy to fix with a little practice.

#### Undefined references

An undefined reference happens when you include a reference (as in Section~\ref{sec:embedded_labels_and_cross_references}) that doesn't correspond to any label:

```latex
Section~\ref{sec:foo_bar}
```

\noindent This is easy to fix by finding the relevant section and adding the corresponding label:

```latex
## Foo bar
\label{sec:foo_bar}
```

#### Multiply defined labels

A multiply defined label simply means that two elements have the same label. Here is an example:

```latex, options: "hl_lines": [2, 5]
## Section foo
\label{sec:foo}

## Section bar
\label{sec:foo}
```

\noindent These are easy to fix just by changing one of the labels:

```latex, options: "hl_lines": [2, 5]
## Section foo
\label{sec:foo}

## Section bar
\label{sec:bar}
```

### EPUB validation
\label{sec:epub_validation}

As an optional step, you can validate your book according to the EPUB3 standard using Softcover's built-in validator:

```console
$ softcover epub:validate
```

\noindent This isn't particularly useful when you're in the middle of writing your book, as many things (such as undefined cross-references) will render your book invalid, but it's helpful near the end when you want a stringent check on your book's validity.

<!-- footnotes -->

[^overfull_code]: Because of the way Softcover implements code blocks, every code sample generates an overfull warning. Since they are not cause for concern, the \verb+--find-overfull+ flag filters them out.
