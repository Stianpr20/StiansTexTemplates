# Stian's LaTeX templates

## Description

This repository contains projects serving as templates for TeX documents.
They are based on projects and courses from my time at university.
Currently, only the Thesis template has been uploaded.

## Installation

### TeX

Install BasicTex, either by downloading the package manually from [TUG.org](https://www.tug.org/mactex/morepackages.html) or via [Homebrew](https://brew.sh) (if using macOS):

```shell
brew install --cask basictex
```

### Packages

Install additional packages with the `tlmgr install` command, i.e.:

```shell
sudo tlmgr install <list of packages>
```

The list should be an array of package names separated by space.

#### Packages required for the `Thesis` template

```text
anyfontsize arydshln biber biblatex cancel csquotes csvsimple enumitem esint extarrows gobble ifmtarg imakeidx lipsum markdown mhchem mwe nextpage nomencl paralist rsfs siunitx stmaryrd tablefootnote texcount tikzducks titlesec tocbibind tocloft wallpaper wrapfig xifthen zref
```

(`lipsum`, `mwe` and `tikzducks` are just for placeholder content.)

## Setup

You will need an editor to write your LaTeX code in and make the compilation process more convenient.
I recommend using [Visual Studio Code](https://code.visualstudio.com/) with the [LaTeX Workshop](https://github.com/James-Yu/LaTeX-Workshop) extension.
A couple of alternatives include [Texmaker](https://www.xm1math.net/texmaker/) and [TexItEasy](https://github.com/Bramas/texiteasy).

### Compilation notes

A complete compilation sequence should contain the following steps:

1. XeLaTeX
2. MakeIndex (regular index)
3. Biber
4. XeLaTeX
5. XeLaTeX
6. TeXcount summary
7. TeXcount total count
8. XeLaTeX

In the subsections below are suggested configuration commands.

#### Visual Studio Code

The various tools can be defined in VScode's `settings.json`:

```json
"latex-workshop.latex.tools": [
        {
            "name": "XeLaTeX",
            "command": "xelatex",
            "args": [
                "-shell-escape",
                "-synctex=1",
                "-interaction=nonstopmode",
                "-file-line-error",
                "%DOCFILE%"
            ],
            "env": {}
        },
        {
            "name": "MakeIndex (regular index)",
            "command": "makeindex",
            "args": [
                "-s",
                "Misc/IndexStyle.ist",
                "%DOCFILE%.idx"
            ]
        },
        {
            "name": "Biber",
            "command": "biber",
            "args": [
                "%DOCFILE%"
            ]
        },
        {
            "name": "TeXcount summary",
            "command": "texcount",
            "args": [
                "-inc",
                "-sum",
                "%DOCFILE%.tex",
                "-out=./Misc/%DOCFILE%_wordcount_summary.txt"
            ],
            "env": {}
        },
        {
            "name": "TeXcount total count",
            "command": "texcount",
            "args": [
                "-inc",
                "-sum",
                "-1",
                "%DOCFILE%.tex",
                "-out=./Misc/%DOCFILE%_wordcount.txt"
            ],
            "env": {}
        }
    ]
```

A complete sequence recipe may be defined in the same `settings.json` file as well:

```json
"latex-workshop.latex.recipes": [
    {
        "name": "Run complete sequence (XeLaTeX)",
        "tools": [
            "XeLaTeX",
            "MakeIndex (regular index)",
            "Biber",
            "XeLaTeX",
            "XeLaTeX",
            "TeXcount summary",
            "TeXcount total count",
            "XeLaTeX"
        ]
    }
]
```

#### Texmaker

```shell
"/Library/TeX/texbin/xelatex" -synctex=1 -interaction=nonstopmode %.tex|"/Library/TeX/texbin/makeindex" -s Setup/IndexStyle.ist %.idx|"/Library/TeX/texbin/makeindex" %.nlo -s nomencl.ist -o %.nls|"/Library/TeX/texbin/bibtex" %.aux|"/Library/TeX/texbin/xelatex" -synctex=1 -interaction=nonstopmode %.tex|"/Library/TeX/texbin/xelatex" -synctex=1 -interaction=nonstopmode %.tex|open %.pdf
```

#### TexitEasy

```shell
xelatex -synctex=1 -interaction=nonstopmode %1 ;
makeindex -s Setup/IndexStyle.ist %1.idx ;
makeindex %1.nlo -s nomencl.ist -o %1.nls ;
bibtex "%1" ;
xelatex -synctex=1 -interaction=nonstopmode %1 ;
xelatex -synctex=1 -interaction=nonstopmode %1

texcount -inc -sum %1.tex -out=misc/%1_wordcount_summary.txt;
texcount -inc -sum -1 %1.tex -out=misc/%1_wordcount.txt
```
