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
    let g:syntastic_javascript_checkers = ['jshint']

### [JSHint](http://www.jshint.com)

JSHint is a better match for the style used by the principal authors than JSLint. JSlint styled code is quite acceptable, but expect resistance if you alter existing code solely to conform to JSLint conventions.

