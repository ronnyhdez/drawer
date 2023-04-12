## How to set up vim

There are two config files: one for vim, otherone for nvim

 - vim: ~/.vimrc
 - nvim: ~/.config/nvim/init.vim

I cannot find at this point a config file for vim **O.o** 
So, I will just create a `.vimrc` under my home:

Now, given that I have installed in my computer vim and nvim, the way that
the configuration files interact, seems that nvim takes prevalence.

So, all the changes I made in the ~/.vimrc are not going to be stablished
unless I source the vimrc in the init.vim

As an example, this is the way my init.vim looks like:

```
" =============================
" Source vimrc
" ============================
source ~/.vimrc

" =============================
" Plugins
" =============================
call plug#begin()
 Plug 'JuliaEditorSupport/julia-vim'
 Plug 'kdheepak/JuliaFormatter.vim'
 Plug 'jpalardy/vim-slime'
 Plug 'neoclide/coc.nvim', {'branch': 'release'}
 Plug 'morhetz/gruvbox'
call plug#end()

" =============================
" VIM Slime
" =============================
let g:slime_target = "neovim"
```

# Creating and modifying the vimrc

```
vim ~/.vimrc
```
## Settings for vim

First I'm going to edit the vimrc file. One trick to try the configurations 
that I want, is that if I'm working on vim I can go to normal mode and try 
for example `:set relative number` and that will include the modification for 
that vim session.

## Customizing vim (plugins)

Plugins are for this. Different from the settings that are things that comes 
already with vim, the plugins are made for this.

To use plugins, we need a plugin manager. One of the most used ones is the
`vim-plug`

In order to use plugins (after installing the vim-plug) our .vimrc file should
contain a section that starts and finish with this:

```
call plug#begin('~/.vim/plugged')

call plug#end()
```

Now, if we want to install a plugin, we should write in the middle of those 
two calls the plugin. Save and then run

```
:PlugInstall
```

And that's it. It will download and install the files needed. As an example, to
change a color scheme we can do:

```
call plug#begin('~/.vim/plugged')
Plug 'gruvbox-community/gruvbox'
call plug#end()
```

After the installation, we can set:

```
:colorscheme gruvbox
```
If I want to know the pluggins installed, and given that we are using the
vim-plug we can check the pluggins in use with:

```
:PlugStatus
```

## Working with vim and git

Is kind of bothering to exit vim in order to commit changes in the git 
repository, so a way to do it is to use the bang `:!` to execute shell
commands.

```
:!git status
:!git commit -m "Ref #2 my changes done"
:!git push
```

Here we can use the `%` to indicate current file:

```
:!git add %
:!git checkout %
```

To be honest, its easier to open a new window with tmux and execute all
terminal commands from there than from the vim session. It's less tipying.

## Workflow

When changing the `.vimrc` file

## Errors

Section with some of the errors that I have faced and followed steps to
solve them.

### .coc nodejs not executable

When opening a vim session, always this message will pop-up:

`"node" is not executable, checkout https://nodejs.org/en/download/`

To install nodejs and avoid this problem, I did:

```
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt-get install -y nodejs
```

### <localleader>rf not starting R session 

Given that the <localleader>rf is not working when opening a R file, one of
the solutions is to indicate the R path in the `.vimrc` file, so I included it

```
let R_path = '~/R/x86_64-pc-linux-gnu-library/4.2'
```

Nonetheless, it's still not working.


## **New start**

So, given that my configuration wasn't working who knows why, I just commented
all the `.vimrc` file and start over again checking what worked and what no.


After commenting my `.vimrc`, I reloaded it. I opened a `vim_test.R` file, hit
`\rf` and it worked!

Now that it worked, I went through the `.vimrc` and start uncommenting some
of the things that I thought would not affect again nvim-r. The final file looks
like this one:

```
" No compatibility with vi to avoid problems
"set nocompatible


" Source vimrc file if specific project have one
"set exrc

" Cursor as a block
set guicursor=

" Use relative numbers in files
set relativenumber

" Line where I'm positioned is the real line number
set nu

" Avoid leaving highligth after a search is done
"set nohlsearch

" No saving or keeping buffer in the background
set hidden

" Indentetation configuration
set tabstop=4 softtabstop=4
set shiftwidth=4
set expandtab
set smartindent

" Start scrolling until 8 spaces away
set scrolloff=8

" Create extra column
set signcolumn=yes

" Mark 80 characters line
set colorcolumn=80

" Enable type file detection
"filetype on

" Enable plugins and load plugins for the detected filetype
"filetype plugin on

" Load an indent file for the detected file type
"filetype indent on

" Set the leader key
"let mapleader=" "

" Set encoding
"set encoding=utf-8

" Plugins
" gruvbox to change color schema
" powerline to obtain bar in bottom with git/project status
call plug#begin()
"Plug 'gruvbox-community/gruvbox'
Plug 'jalvesaq/Nvim-R'     " To work with vim and R
Plug 'preservim/nerdtree'  " View the files
Plug 'ncm2/ncm2'           " Auto-complete R commands
Plug 'gaalcaras/ncm-R'     " Auto complete R commands
call plug#end()

" NERDTree configuration test
nnoremap <C-n> :NERDTree<CR>

" Set the R path for plugin nvim-r
"let R_path = '~/R/x86_64-pc-linux-gnu-library/4.2'
```

### **vim workflow for R projects**

Ok, I don't want to spend more time on this configuration thing. So I started
using vim to work on my R project just to feel the main pains when switching
from RStudio. Along the way, if there is too much pain because of lack of some
sort of functionality that I was too attached to it, I will change the `.vimrc`
file to try to relieve some pain.

`\rf` Connect to R Console

`\rq` Quit R Console

`\d`  Run current line and move to the next line

`\l` Run current line, but cursor will stay on the same line.

`\pp` Run paragraph. But cursor will stay on the same block. 

`\pd` Runs block and move to the next one

`\ss` Execute a block of **selected code**. This has to be done with visual mode

`\aa` Run entire script

`\ro` Open the "Global Environment"

`ctrl + w + l` Jump to left panel

`ctrl + w + h` Jump to right panel

If I made a change on the `~/.vimrc` file, I can source it with `:source ~/.vimcr`

If I have installed the pluggin `NERDTree`, I need to remap some keys. At the 
moment I have in my `~/.vimrc` file the line: `nnoremap <C-n> :NERDTree<CR>`,
so I don't have to do `:NERDTree` to open the file browser. Now this can be done
with:

`ctrl + n` Open file browser on left side by default.

Inside the file tree browser, I can move with the arrows. To expand one folder
and show contents, I can hit `ENTER`. Doing the same in an open folder, will
close it.

To move out from the NERDTree browser I can `ctrl + w + w` so I will be 
jumping panels from left to right.




# References

Plugin repos

 - https://github.com/gaalcaras/ncm-R
 - https://github.com/preservim/nerdtree
 - https://github.com/jamespeapen/Nvim-R/wiki/Use


Sites that can be useful:

https://github.com/nodesource/distributions/blob/master/README.md#debinstall
http://manuals.bioinformatics.ucr.edu/home/programming-in-r/vim-r
https://hpcc.ucr.edu/manuals_linux-cluster_terminalIDE.html
https://gist.github.com/tgirke/7a7c197b443243937f68c422e5471899


# Using NerdTree

When I have a nvim session open, I can open a NerdTree panel with:

```
:NERDTree
```

Once I have to or panels open, I can move between them with `Ctrl + w`

If I'm browsing the files, and I want to open them, this can be useful:

```
enter = open the file in the current window
s = open file in vertical split window
i = open file in horizontal split window
```


