## vim

### Install [Syntastic](https://github.com/scrooloose/syntastic)
### Settings for ~/.vimrc

    execute pathogen#infect()
    
    " use indents of 4 spaces, and have them copied down lines:
    set shiftwidth=4        " Num. of spaces used for each (auto)indent step
    set shiftround
    set expandtab           " Insert spaces instead of tabs
    set autoindent          " Always set auto-indenting on
    set tabstop=4
    set matchtime=2         " show matching bracket for 0.2 seconds
    set smartindent         " smart auto indenting
    set smarttab            " smart tab handling for indenting
    set fileformat=unix     " file mode is unix
    set hidden              " remember undo after quitting
    
    autocmd FileType javascript set omnifunc=javascriptcomplete#CompleteJS
    
    " When saving a file, if there are errors, have Syntastic plugin mark them
    let g:syntastic_enable_signs=1
    let g:syntastic_auto_loc_list=1
    let g:syntastic_javascript_checkers = ['eslint']

### [eslint](http://eslint.org)

There is an .eslintrc file in the root directory of the repo. Some code editors are smart enough to use it and highlight coding errors for you, which we encourage.