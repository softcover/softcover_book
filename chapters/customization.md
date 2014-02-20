# Customization and advanced options
\label{cha:customization}

Softcover includes a large number of advanced options such as CLI customization, user-defined styling and typesetting commands, and foreign-language support. Many of these options are in active development, so I recommend requesting an invitation to the [Softcover Google Group](https://groups.google.com/forum/#!forum/softcover-publishing) to get the inside track on their status.

## Command-line interface

Two of the most important commands in the Softcover CLI are `build:all` and `deploy`. Their default behavior is sufficient for most purposes, but some authors will want to customize them for their specific needs. Softcover allows such customization via [dot-files](https://en.wikipedia.org/wiki/Dot-file) in the book's root directory, as described below.

### Customizing builds
\label{sec:customizing_builds}

By default, `softcover build:all` generates HTML, EPUB, MOBI, and PDF, but it's easy to customize. All you need to do is edit the file `.softcover-build` in the book's root directory (Listing~\ref{code:build_config}).

\begin{codelisting}
\label{code:build_config}
\codecaption{The default build configuration file \texttt{.softcover-build}. \\ \filepath{\$ROOT\_DIRECTORY/.softcover-build}}
```text
# Edit this file to customize your build steps with custom command options
# or additional commands.
#
# softcover build:pdf
# softcover build:mobi
# softcover build:preview
```
\end{codelisting}

For example, if you want to use Calibre in place of KindleGen (Section~\ref{sec:build_mobi}) and build previews (Section~\ref{sec:build_previews}) by default, you can use the `.softcover-build` file shown in Listing~\ref{code:build_preview_calibre}. This uncomments the lines in Listing~\ref{code:build_config} and adds the \verb+--calibre+ flag to the `build:mobi` command. (Note that Listing~\ref{code:build_preview_calibre} omits `softcover build:epub` because EPUB files are generated automatically as a side-effect of building MOBI.)

\begin{codelisting}
\label{code:build_preview_calibre}
\codecaption{Using Calibre and building previews in \texttt{softcover build:all}. \\ \filepath{\$ROOT\_DIRECTORY/.softcover-build}}
```text, options: "hl_lines": [5, 6]
# Edit this file to customize your build steps with custom command options
# or additional commands.
#
softcover build:pdf
softcover build:mobi --calibre
softcover build:preview
```
\end{codelisting}


### Customizing deploys
\label{sec:customizing_deploys}

You can customize the behavior of `softcover deploy` by editing the
file `.softcover-deploy` in the project's root directory (Listing~\ref{code:deploy_config}).

\begin{codelisting}
\label{code:deploy_config}
\codecaption{The default deployment configuration file \texttt{.softcover-deploy}. \\ \filepath{\$ROOT\_DIRECTORY/.softcover-deploy}}
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
\codecaption{Removing previews and adding a \texttt{git push origin}. \\ \filepath{\$ROOT\_DIRECTORY/.softcover-deploy}}
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

Softcover books are based on \LaTeX\ (for PDF) and HTML (for EPUB and MOBI), both of which allow for extensive customization via style files and CSS, respectively. There even one point of overlap: commands defined in `latex_styles/custom.sty` are available across all formats (Section~\ref{sec:custom_commands}).


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



*Note*: Custom *math* commands are supported locally but are not currently not supported on the Softcover site. We're planning to add it as soon as someone wants it, though, so send a request to \texttt{michael@softcover.io} and we'll get right on it.

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

Any changes you make in `custom.css` (Section~\ref{sec:html_style}) will automatically be incorporated into the EPUB and MOBI styles as well, but sometimes you'll want additional customization that applies *just* to EPUB and MOBI books. The file `custom_epub.css` allows for this extra layer of detail (Listing~\ref{code:custom_epub_css}). Its location is specific to the EPUB standard; it's located in `epub/OEBPS/styles/custom_epub.css`.

\begin{codelisting}
\label{code:custom_epub_css}
\codecaption{The generated custom CSS file for EPUB/MOBI. \\ \filepath{epub/OEBPS/styles/custom\_epub.css}}
<<(epub/OEBPS/styles/custom_epub.css)
\end{codelisting}

The entire `epub/` directory is `.gitignore`d by default, so if you're using Git and you've made custom changes you should add the `custom_epub.css` file by hand:

```console
$ git add --force epub/OEBPS/styles/custom_epub.css
```

### PDF style
\label{sec:pdf_style}

You can customize the PDF styles in two different ways. The first and simpler is to edit the file `latex_styles/custom_pdf.sty`, whose default content is shown in Listing~\ref{code:custom_pdf_style}. This file gets included *last*, so any rules in `custom_pdf.sty` will override the defaults. Common uses for `custom_pdf.sty` include defining hyphenation rules for words \LaTeX\ can't hyphenate natively and adding support for any Unicode characters not supported by the PDF typesetting engine (xelatex). For example, uncommenting the code in Listing~\ref{code:custom_pdf_style} adds the rule for hyphenating "JavaScript" and adds PDF support for the Unicode characters ★ and ž (Listing~\ref{code:custom_pdf_style_uncommented}).

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

One use of `custom_pdf.sty` is important enough to deserve special mention, namely, changing the default paragraph indentation and spacing to match the style in HTML/EPUB/MOBI. In line with standard practices for professionally typeset books, the default behavior in PDF is for all paragraphs after the first one in a section to be indented. This indentation serves as a visual marker for paragraph boundaries, making them easier to parse visually. On the other hand, this convention requires suppressing indentation by hand (using a \verb+\noindent+) after elements like code blocks, as discussed at the end of Section~\ref{sec:code_fencing}. Without a \verb+\noindent+, any text after the code block gets interpreted as a new paragraph and is indented, which is often not what you want.

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

The second method for customizing PDF output is to edit the file `preamble.tex` in the `config` directory; the default contents appear in Listing~\ref{code:preamble_tex}. By editing this file, you can do things like change the PDF font size or include packages that don't work when included in `custom_pdf.sty`. For example, the default font size (`14pt`) is designed to look good on tablet devices such as iPad, but some authors may prefer the smaller fonts typically used for print publications (`10pt` or `12pt`). The `preamble.tex` file is especially important for foreign language support, which requires that the appropriate `polyglossia` package be included before the default `softcover.sty` file. See Section~\ref{sec:foreign_language} for details.


\begin{codelisting}
\label{code:preamble_tex}
\codecaption{The preamble file for changing fonts, etc. \\ \filepath{config/preamble.tex}}
<<(example_book/config/preamble.tex)
\end{codelisting}


## Foreign-language support
\label{sec:foreign_language}

Softcover has experimental support for foreign languages. Please request an invitation to the [Softcover Google Group](https://groups.google.com/forum/#!forum/softcover-publishing) and send us a note if you're interested in writing a Softcover book in a language other than English.

### Polyglossia and \texttt{lang.yml}

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

Second, to enable foreign language support in HTML/EPUB/MOBI, we need to edit the file `lang.yml` in the `config` directory to tell Softcover the names of the various elements (chapter, section, listing, etc.). The default (English) values are shown in Listing~\ref{code:default_lang_yml}, while the edited values for French are shown in Listing~\ref{code:french_lang_yml}. (I speak a little French, but I'm not fluent, so any corrections to Listing~\ref{code:french_lang_yml} are appreciated.)

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
aside: Boîte
listing: Inscription
equation: Equation
eq: Eq
frontmatter: Préliminaires
contents: Table de Matières
```
\end{codelisting}

With the settings as in Listing~\ref{code:french_lang_yml}, labels such as "Chapitre" for "Chapter" will be unified across output formats. In addition, cross-references will link to the full word in addition to the number, so that, e.g., the link "Boîte 1.1" would include the word "Boîte" as well as "1.1" (as in Box~\ref{aside:softcover_uses}).

### Terrifyingly advanced comments on Hungarian

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

<!-- ## Detailed refinements
\label{sec:detailed_refinements}
 -->

