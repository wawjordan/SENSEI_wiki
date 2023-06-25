## Contents

- [Prerequisites](#prerequisites)
- [Setting up your bashrc](#setting-up-your-bashrc)
- [Setting up on VT ARC](#setting-up-on-vt-arc) 
- [Setting up your text editor](#setting-up-your-text-editor)

## Prerequisites
- roycfd (/home/roycfd) has a lot of prerequisites installed, so you may not be worried about installing a lot of packages, but you may need to source them.
- Git installed on your system
- [[GCC|GCC]] >= 6.2.0 (maybe can be relaxed)
- A text editor, Vim, gVim, Emacs, etc...
- LAPACK (LAPACK has been installed at roycfd, which is a directory: /home/roycfd/ . LAPACK can be sourced and included using: export LD_LIBRARY_PATH=/home/roycfd/Software/lapack/lapack-3.7.0:$LD_LIBRARY_PATH)
- [[pFunit|pFunit]] (if doing testing. pFunit can be found here: /home/roycfd/Software/pFUnit)

For more information on using Git, please search online.

## Setting up your bashrc

Since the AOE CFD lab computers default to a bash environment,
we'll set up some environment variables and aliases in the .bashrc file:

- Open up a terminal and open .bashrc with a text editor.
- Prepend our custom built GCC (6.4.0) compiler collection and its libraries to our path:

```js
export PATH=/home/roycfd/Software/gcc/gcc-6.4.0/bin:$PATH
export LD_LIBRARY_PATH=/home/roycfd/Software/gcc/gcc-6.4.0/lib64/:$LD_LIBRARY_PATH
```
- Prepend our custom built Doxygen to our path [This no longer works]:

```js
export PATH=/home/roycfd/Software/doxygen-1.8.3.1/bin:$PATH
```
- Add LAPACK to your library path

```js
export LD_LIBRARY_PATH=/home/roycfd/Software/lapack/lapack-3.7.0:$LD_LIBRARY_PATH
```
- In addition, the location of the pFUnit directory must be known:

```js
export PFUNIT=/home/roycfd/Software/pFUnit;
```
- Prepend our OpenMPI compiler collection and its libraries to our path:

```js
export OMPI_GCC_BASE=/home/roycfd/Software/openmpi/openmpi-1.10.3;
export PATH=$OMPI_GCC_BASE/bin:$PATH
export LD_LIBRARY_PATH=$OMPI_GCC_BASE/lib:$LD_LIBRARY_PATH
```
- Run 'source .bashrc' to activate them
- Run 'gcc --version' and make sure it says 6.4.0, otherwise you may have a typo.

## Or, if you want to use the following .bashrc directly (You may optimize it based on your own need)

```js
case $- in
  *i*) ;;
    *) return;;
esac

shopt -s checkwinsize
PS1="[\u@\h:\W]$ "

export VIMRC=~/.vimrc

export GCC_BASE=/home/roycfd/Software/gcc/gcc-6.4.0
export PATH=$GCC_BASE/bin:$PATH
export LD_LIBRARY_PATH=$GCC_BASE/lib64:$LD_LIBRARY_PATH

export OMPI_GCC_BASE=/home/roycfd/Software/openmpi/openmpi-1.10.3
export OMPI_PGI_BASE=/home/roycfd/Software/openmpi/openmpi-1.10.3_pgi
export PATH=$OMPI_GCC_BASE/bin:$PATH

export PATH=/usr/local/cuda-8.0/bin${PATH:+:${PATH}}
export LD_LIBRARY_PATH=/usr/local/cuda-8.0/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}

export LD_LIBRARY_PATH=$OMPI_GCC_BASE/lib:$LD_LIBRARY_PATH
export LIBRARY_PATH=$OMPI_GCC_BASE/lib:$LIBRARY_PATH
export C_INCLUDE_PATH=$OMPI_GCC_BASE/include:$C_INCLUDE_PATH
export CPLUS_INCLUDE_PATH=$OMPI_GCC_BASE/include:$CPLUS_INCLUDE_PATH
export LD_LIBRARY_PATH=/home/roycfd/Software/lapack/lapack-3.7.0_pgi:$LD_LIBRARY_PATH
export LD_LIBRARY_PATH=/home/roycfd/Software/lapack/lapack-3.7.0:$LD_LIBRARY_PATH
export CPLUS_INCLUDE_PATH=$OMPI_GCC_BASE/include:$CPLUS_INCLUDE_PATH

export PATH=/home/roycfd/Software/doxygen-1.8.3.1/bin:$PATH
export PATH=/home/grad3/weich97/SENSEI/build/bin:$PATH
export PATH=/l/weich97/SENSEI/debug/bin:$PATH

#CMake stuff
export PATH=/home/roycfd/Software/cmake/cmake-3.10.1/bin:$PATH

#Pfunit
export PFUNIT="/home/roycfd/Software/pFUnit/pFUnit_3.2.8_serial"
export PFUNIT=/home/roycfd/Software/pfunit;

#Python
export PATH=$PATH:/aoe/bin
export LD_LIBRARY_PATH=/home/grad3/weich97/aoe/lib:$LD_LIBRARY_PATH
export PYTHONPATH=/home/grad3/weich97/aoe/lib:$PYTHONPATH
alias python="/home/grad3/weich97/aoe/bin/python2.7"

#Tecplot (The following three are for the tecplot use in python. Commenting the second for ssh)
export LD_LIBRARY_PATH=/aoe/tecplot/tecplot2017r2/360ex_2017r2/bin/:$LD_LIBRARY_PATH
#export LD_LIBRARY_PATH=/aoe/tecplot/tecplot2017r2/360ex_2017r2/bin/sys/:$LD_LIBRARY_PATH
export LD_LIBRARY_PATH=/home/grad3/weich97/aoe/bin/:$LD_LIBRARY_PATH

alias ls='ls --color=auto'
alias ll="ls -al"
alias ompi_gcc="export PATH=$OMPI_GCC_BASE/bin:$PATH && 
	export LD_LIBRARY_PATH=$OMPI_GCC_BASE/lib64:$LD_LIBRARY_PATH"

alias tecplot="/aoe/bin/tecplot2017r2"


#source /aoe/bin/compilervars.sh intel64
```
 - As said, every time when you launch a terminal, run 'source .bashrc' to activate them
 - Then run 'ompi_gcc', which is an alias to export the PATH.

## Setting up on VT ARC

- Login to a cluster:

```js
ssh -X your_PID@newriver1.arc.vt.edu
ssh -X your_PID@cascades1.arc.vt.edu
ssh -X your_PID@tinkercliffs1.arc.vt.edu
```

### NewRiver

- Apply nodes (interactive mode):

```js
qsub -I -lnodes=1:ppn=12 -q normal_q -l walltime=12:00:00 -W group_list=newriver -A your_allocation
```

You can change node number, ppn (processor per node) and walltime. your_allocation is the allocation you have on ARC.


- Load modules:

```js
module load gcc/5.2.0 openmpi/2.0.0 cmake/3.8.2
module load openblas/0.2.2
```

After loading the modules listed above, you can start to run your case. 
Run a case with SENSEI: first cd to the directory in which you store your cases; then, mpirun -np 4 ~/SENSEI (You can change how many cores you want to use. Also, you need to include the correct path where you place the SENSEI executable)

### Cascades

- Apply nodes (interactive mode):

```js
salloc --time=2:00:00 --partition=normal_q -N 1 -n 8 --account=your_allocation
```

- Load modules:

```js
module load gcc/7.3.0 cmake/3.8.2
module load openmpi/3.1.2 openblas/0.3.6
```

After loading the modules listed above, you can start to run your case. 
Run a case with SENSEI: first cd to the directory in which you store your cases; then, mpirun -np 4 ~/SENSEI (Similar as that on Newriver, you can change how many cores you want to use. Also, you need to use the executable for Cascades, instead of Newriver)

### TinkerCliffs

- Apply nodes (interactive mode):

```js
salloc --time=20:00:00 --partition=normal_q -N 1 -n 8 --account=your_allocation
```

- Load modules:

```js
module load foss
module load CMake/3.16.4-GCCcore-9.3.0
```

Use the executable on TinkerCliffs to run your case there.

### Check your allocation info.

https://www.arc.vt.edu/wp-admin/edit.php?post_type=project2&page=approved_allocations

## Setting up your text editor
By default, the AOE CFD lab computers should have Vim/gVim 7.2 and Emacs 23.1.
See the following for helpful configuration files.

### Setting up gVim
If you want to use Vim or gVim, create a .vimrc file in your home directory. 

Sample .vimrc file for Fortran editing:

```js
" --- Shortcuts ------------------------------------------------------------- "

" Use to edit this file while in Vim "
nnoremap <esc>ev :vsplit $VIMRC<cr><esc>
" Use to apply changes to this file "
nnoremap <esc>sv :source $VIMRC<cr><esc>
" Use to remove Windows EOL characters "
nnoremap <esc>rm :%s/\r$<cr><esc>

" Navigate Vim split windows more easily "
" (Use <Ctrl>+i,j,k,l) "
nmap <C-j> <C-W>h<C-W>=<esc>sv
nmap <C-l> <C-W>l<C-W>=<esc>sv
nmap <C-i> <C-W>k<C-W>=<esc>sv
nmap <C-k> <C-W>j<C-W>=<esc>sv

" --- Formatting/Behavior of Vim -------------------------------------------- "

let &winwidth = 80
let &winheight = &lines * 7/10

" Set file formatting "
filetype plugin indent on
syntax on
set autoread
set autowriteall

" Tab Settings "
"   Set globally here by uncommenting or "
"   copy to ~/.vim/after/ftplugin/fortran.vim "
"     and change 'set' to 'set local' "
"set shiftwidth=2 "
"set tabstop=2 "
"set softtabstop=2 "
"set expand tab "

" Default to free format Fortran "
let fortran_free_source=1
autocmd BufReadPre,BufNewFile *.f90 setfiletype fortran
autocmd BufReadPre,BufNewFile *.F90 setfiletype fortran
autocmd BufReadPre,BufNewFile *.f95 setfiletype fortran
autocmd BufReadPre,BufNewFile *.F95 setfiletype fortran
autocmd BufReadPre,BufNewFile *.f03 setfiletype fortran
autocmd BufReadPre,BufNewFile *.F03 setfiletype fortran
autocmd BufReadPre,BufNewFile *.f08 setfiletype fortran
autocmd BufReadPre,BufNewFile *.F08 setfiletype fortran

"Highlight characters beyond 80 char limit"
autocmd FileType fortran,c,cpp,java,php autocmd BufRead,BufNewFile <buffer> match ErrorMsg '\%>80v.\+'

"Highlight/Remove trailing space"
autocmd FileType fortran,c,cpp,java,php autocmd BufRead,BufNewFile <buffer> 2match ErrorMsg '\s\+$'
autocmd FileType fortran,c,cpp,java,php autocmd BufWritePre <buffer> :%s/\s\+$//e
```

### Setting up Emacs

If you want to use Emacs, open a terminal in your home directory and create a .emacs file.

Sample .emacs file for Fortran editing:

```js
;Code formatting stuff
;These indent settings cause Emacs to insert spaces instead of tabs

;Tabs are bad!
(setq f90-do-indent 2)
(setq f90-continuation-indent 2)
(setq f90-if-indent 2)
(setq f90-type-indent 2)
(setq f90-program-indent 2)
(setq standard-indent 2)
(setq fortran-do-indent 2)
(setq fortran-if-indent 2)
(setq next-line-add-newlines nil)

;Emacs window behavior
(setq inhibit-startup-screen t)
(setq frame-title-format "%b")
(setq make-backup-files nil)
(setq line-number-mode t)
(setq column-number-mode t)

;Adjust the frame size to fit your screen
;80 Column line limit and XX Row limit depends on screen height/font
(set-frame-size (selected-frame) 80 61)

;Trailing whitespace is bad, it clutters up commit diffs!
(add-hook 'before-save-hook 'delete-trailing-whitespace)

;Make Emacs understand that all the .fXX/.FXX extensions are f90
(add-to-list 'auto-mode-alist '("\\\\\\.f90\\\\\\'" . f90-mode))
(add-to-list 'auto-mode-alist '("\\\\\\.f95\\\\\\'" . f90-mode))
(add-to-list 'auto-mode-alist '("\\\\\\.f03\\\\\\'" . f90-mode))
(add-to-list 'auto-mode-alist '("\\\\\\.f08\\\\\\'" . f90-mode))
(add-to-list 'auto-mode-alist '("\\\\\\.F90\\\\\\'" . f90-mode))
(add-to-list 'auto-mode-alist '("\\\\\\.F95\\\\\\'" . f90-mode))
(add-to-list 'auto-mode-alist '("\\\\\\.F03\\\\\\'" . f90-mode))
(add-to-list 'auto-mode-alist '("\\\\\\.F08\\\\\\'" . f90-mode))
```