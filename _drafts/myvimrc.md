---
layout: post
title: My .vimrc file
category: me
excerpt: My .vimrc file configuration for programming.
comments: false
google_adsense: false
---
```bash
set tabstop=8                           "Length of tab."
set shiftwidth=8                        "Indent length"
filetype plugin indent on               "Indents next line according to Language"
syntax on                               "Enabling Syntax Highlight"
colorscheme slate                       "Selecting the syntax hightlight theme"
set mouse-=a                            "Disable `visual` mode when text"
                                        "selected with mouse"
set backspace=indent,eol,start          "Enable Backspace key before `start of"
                                        "the line`, `line break` and `character`."
set showcmd                             "Display Command at Bottom Right Corner"
set ruler                               "Display Line Num at Bottom Right Corner"
set hlsearch                            "Highlight matched words while searching"
packadd! matchit                        "Extend the `%` command functionality to"
                                        "html tags"
map Q ^i/* <ESC>A */<ESC>               "Convert line into a Comment"
set shell=/bin/bash\ -i                 "Execute alias within shell"
let g:GetLatestVimScripts_allowautoinstall=1
                                        "Give permission to 'getscript' to install"
                                        "Plugins"
```

**Notes**
* `map` is used to create key mappings for certain operation.  
For example: `map Q dd` creates vim shortcut `Q` to do `dd` operation.
