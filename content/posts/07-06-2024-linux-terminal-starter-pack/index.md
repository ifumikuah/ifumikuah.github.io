---
title: "Minimalist Linux Term"
date: 2024-06-07
description: "Konfigurasi minimal terminal linux"
tags: [teknologi, linux]
---

Konfigurasi minimal untuk shell dan vim sebagai text editor, untuk shell menggunakan `fish`. `fish` memiliki syntax autosuggestion dan syntax highlighting mempermudah navigasi dan perintah.

## Konfigurasi vim

```plain
call plug#begin()
Plug 'itmammoth/doorboy.vim'
Plug 'preservim/NERDTree'
Plug 'sheerun/vim-polyglot'
Plug 'dracula/vim', { 'as': 'dracula' }
call plug#end()

set autoindent smartindent expandtab tabstop=2 shiftwidth=2
set splitbelow
set encoding=utf-8
set number
set relativenumber
set cursorline
set nowrap
set showcmd
"set visualbell
set termwinsize=6x0
set t_Co=256
"colorscheme dracula
```
