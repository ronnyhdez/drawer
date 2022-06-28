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




# References

Sites that can be useful:

https://github.com/nodesource/distributions/blob/master/README.md#debinstall
http://manuals.bioinformatics.ucr.edu/home/programming-in-r/vim-r
https://hpcc.ucr.edu/manuals_linux-cluster_terminalIDE.html
https://gist.github.com/tgirke/7a7c197b443243937f68c422e5471899




