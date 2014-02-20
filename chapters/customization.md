# Customization and advanced options
\label{cha:customization}

Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod
tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam,
quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo
consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse
cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non
proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

## Command-line interface

Two of the most important commands in the Softcove CLI are `build:all` and `deploy`. Their default behavior is sufficient for most purposes, but some authors will want to customize them for their specific needs. Softcover allows such customization via dotfiles in the book's root directory, as described below.

### Customizing builds
\label{sec:customizing_builds}

By default, `softcover build:all` generates HTML, EPUB, MOBI, and PDF, but it's easy to customize. All you need to do is edit the file `.softcover-build` in the book's root directory (Listing~\ref{code:build_config}). The generated file has some suggestions that are commented out with the `#` character; if all such commands are commented out, `softcover build:all` uses the defaults as described in Section~\ref{sec:build_all}, but uncommenting any line activates customization.

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

Softcover books are based on \LaTeX\ (for PDF) and HTML (for EPUB and MOBI), both of which allow for extensive customization via style files and CSS, respectively. There's actually one point of overlap: commands defined in `latex_styles/custom.sty` are available across all formats (Section~\ref{sec:custom_commands}).


### Custom commands
\label{sec:custom_commands}

\LaTeX\ natively supports custom commands using `newcommand`, and Softcover adds support for `newcommand` to HTML (and thus EPUB/MOBI) as well. The default customization file has examples to get you started (Listing~\ref{code:default_custom_sty}).

\begin{codelisting}
\label{code:default_custom_sty}
\codecaption{The default customization file. \\ \filepath{latex\_styles/custom.sty}}
<<(example_book/latex_styles/custom.sty, lang: latex)
\end{codelisting}


This means, for example, that we can define the command \verb+\bfi+, which takes one argument and sets it in \bfi{boldface italic}, as shown in Listing~\ref{code:bfi}.

\begin{codelisting}
\label{code:bfi}
\codecaption{Defining a boldface italic command. \\ \filepath{latex\_styles/custom.sty}}
```latex
\newcommand{\bfi}[1]{\textbf{\textit{{#1}}}}
```
\end{codelisting}

Because the `custom.sty` file is raw \LaTeX, definitions must be in \PolyTeX\ (Chapter~\ref{cha:polytex_tutorial}) and not Markdown. This isn't as hard as you think, though, and \LaTeX\ commands are highly Googleable. Try, for example, "[latex boldface italic](http://lmgtfy.com/?q=latex+boldface+italic)") to see how you might have guessed how to make \verb+\bfi+ without my help.

As indicated in the comment at the top of Listing~\ref{code:default_custom_sty}, commands that only make sense for PDF output---such as hyphenation definitions or specific \LaTeX\ package includes---should be included in `custom_pdf.sty` instead of `custom.sty` (Section~\ref{sec:pdf_style}).

*Note*: Custom *math* commands are supported locally but are not currently not supported on the Softcover site. We're planning to add it as soon as someone wants it, though, so send a request to \texttt{michael@softcover.io} and we'll get right on it.

### HTML style
\label{sec:html_style}

You can customize Softcover's HTML styles using the `custom.css` file in the `html/stylesheets` directory. As noted in the comment shown in Listing~\ref{code:custom_css}, the only caveat is that the CSS rules have to be scoped by the `#body` id.

\begin{codelisting}
\label{code:custom_css}
\codecaption{The generated custom CSS file. \\ \filepath{html/stylesheets/custom.css}}
<<(example_book/html/stylesheets/custom.css)
\end{codelisting}

*Note*: Custom CSS is supported locally but is not currently not supported on the Softcover site. We're planning to add it as soon as someone wants it, though, so send a request to \texttt{michael@softcover.io} and we'll get right on it.


### EPUB/MOBI style

Any changes you make in `custom.css` (Section~\ref{sec:html_style}) will automatically be incorporated into the EPUB and MOBI styles as well, but sometimes you'll want additional customization that applies *just* to EPUB and MOBI books. The file `custom_epub.css` allows for this extra later of detail (Listing~\ref{code:custom_epub_css}). Its location is specific to the EPUB standard; it's located in `epub/OEBPS/styles/custom_epub.css`.

\begin{codelisting}
\label{code:custom_css}
\codecaption{The generated custom CSS file for EPUB/MOBI. \\ \filepath{epub/OEBPS/styles/custom\_epub.css}}
<<(epub/OEBPS/styles/custom_epub.css)
\end{codelisting}

The entire `epub/` directory is Git-ignored by default, so if you're using Git and you've made custom changes you should add the `custom_epub.css` file by hand:

```console
$ git add --force epub/OEBPS/styles/custom_epub.css
```

### PDF style
\label{sec:pdf_style}

Finally, you can customize the PDF styles in two different ways

gets included last, so use to override defaults.

\begin{codelisting}
\label{code:custom_pdf_style}
\codecaption{The default custom PDF style file. \\ \filepath{latex\_styles/custom\_pdf.sty}}
<<(example_book/latex_styles/custom_pdf.sty, lang: latex)
\end{codelisting}

custom_pdf.sty, including hyphenation

Note way to get rid of need for noindent

\begin{codelisting}
\label{code:noindent_style}
\codecaption{Removing paragraph indentation.}
```latex
gonna be here
```
\end{codelisting}


\begin{codelisting}
\label{code:preamble_tex}
\codecaption{The preamble file for changing fonts, etc. \\ \filepath{config/preamble.tex}}
<<(example_book/config/preamble.tex)
\end{codelisting}




preamble.tex


## Foreign-language support (experimental)

* Uncomment polyglossia in preamble.tex
* Edit lang.yml


Softcover has experimental advanced support for foreign languages. Request an invitation to the [Softcover Google Group](https://groups.google.com/forum/#!forum/softcover-publishing) and send us a note if you're interested in writing a Softcover book in a foreign language.