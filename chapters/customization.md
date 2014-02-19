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

As indicated in the comment at the top of Listing~\ref{code:default_custom_sty}, commands that only make sense for PDF output---such as hyphenation definitions or including specific \LaTeX\ packages---should be included in `custom_pdf.sty` instead of `custom.sty` (Section~\ref{sec:pdf_style}).

*Note*: Custom math commands are supported locally but are not currently supported on the Softcover site, but we're planning to add it as soon as someone complains. Send complaints to \texttt{michael@softcover.io} and we'll get right on it.

### HTML style

custom.css

*Note*: Custom CSS is supported locally but is not currently not supported on the Softcover site, but we're planning to add it as soon as someone complains. Send complaints to \texttt{michael@softcover.io} and we'll get right on it.


### EPUB/MOBI style

custom_epub.css

git add it

### PDF style
\label{sec:pdf_style}

custom_pdf.sty, including hyphenation

Note way to get rid of need for noindent

preamble.tex


## Foreign-language support (experimental)

Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod
tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam,
quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo
consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse
cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non
proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
