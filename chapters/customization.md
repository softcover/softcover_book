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
```text
# Edit this file to customize your build steps with custom command options
# or additional commands.
#
softcover build:pdf
softcover build:mobi --calibre
softcover build:preview
```
\end{codelisting}


### Customizing deploys


## Commands and styles

### Custom commands

custom.sty,

### HTML style

custom.css

### EPUB/MOBI style

custom_epub.css

git add it

### PDF style

custom_pdf.sty

Note way to get rid of need for noindent

preamble.tex


## Foreign-language support (experimental)

Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod
tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam,
quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo
consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse
cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non
proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
