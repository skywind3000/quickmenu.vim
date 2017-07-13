## Preface

There are many keymaps defined in my `.vimrc`. Tired to check my `.vimrc` again and again when I forget some, so I made this `quickmenu` plugin which can be fully customized:

- Well formatted and carefully colored to ensure neat and handy.
- Press `<F12>` to popup `quickmenu` on the right, use `j` and `k` to move up and down.
- Press `<Enter>` or `1` to `9` to select an item.
- `Help` details will display in the cmdline when you are moving around the cursor.
- Items can be filtered by `filetype`, different items for different filetypes.
- Macros in `%{...}` form from `text` and `help` will be evaluated and expanded.
- No longer have to be afraid for forgetting keymaps.

Just see this GIF demonstration below:

![](http://skywind3000.github.io/word/images/menu/menu-2.gif)

Trying to share my configuration to my friends, I found that they did't have patience to remember all the keymaps in my vimrc, but a quickmenu is quite accaptable for them. 


## Install

Copy `autoload/quickmenu.vim` and `syntax/quickmenu.vim` to your `~/.vim/autoload` and `~/.vim/syntax`. or use Vundle to install it from `skywind3000/quickmenu.vim`. Add these lines to your `.vimrc`:

```VimL
" choose a favorite key to show/hide quickmenu
noremap <silent><F12> :call quickmenu#toggle(0)<cr>

" enable cursorline (L) and cmdline help (H)
let g:quickmenu_options = "HL"
```

## Tutorial

Simply use the function below:

```VimL
function quickmenu#append(text, action [, help = ''])
```

- `text` will be show in the quickmenu, vimscript in `%{...}` will be evaluated and expanded.
- `action` is a piece of vimscript to be executed when a item is selected.
- `help` will display in the cmdline if g:quickmenu_options contains `H`.

A item will be treated as "static text" (unselectable) If `action` is empty. `text` starting with "#" represents a new section.

Example for `.vimrc`:

```
" enable cursorline (L) and cmdline help (H)
let g:quickmenu_options = "LH"

" clear all the items
call g:quickmenu#reset()

" bind to F12
noremap <silent><F12> :call quickmenu#toggle(0)<cr>


" section 1, text starting with "#" represents a section (see the screen capture below)
call g:quickmenu#append('# Develop', '')

call g:quickmenu#append('item 1.1', 'echo "1.1 selected"', 'select item 1.1')
call g:quickmenu#append('item 1.2', 'echo "1.2 selected"', 'select item 1.2')
call g:quickmenu#append('item 1.3', 'echo "1.3 selected"', 'select item 1.3')

" section 2
call g:quickmenu#append('# Misc', '')

call g:quickmenu#append('item 2.1', 'echo "2.1 selected"', 'select item 2.1')
call g:quickmenu#append('item 2.2', 'echo "2.2 selected"', 'select item 2.2')
call g:quickmenu#append('item 2.3', 'echo "2.3 selected"', 'select item 2.3')
call g:quickmenu#append('item 2.4', 'echo "2.4 selected"', 'select item 2.4')
```

And then quickmenu is ready:

![](http://skywind3000.github.io/word/images/menu/menu-1.gif)

Vim is lack of ui components, that's ok for experienced user, but hard for the others. A quickmenu is quite easier for them. Now we have this cute menu and show/hide it with F12. and no longer have to worry about  forgetting keymaps.


## Documentation


#### Add new items into menu

```VimL
function quickmenu#append(text, action [, help = ''])
```

- `text` will be show in the quickmenu, vimscript in `%{...}` will be evaluated and expanded.
- `action` is a piece of vimscript to be executed when a item is selected.
- `help` will display in the cmdline if g:quickmenu_options contains `H`.

A item will be treated as "static text" (unselectable) If `action` is empty. `text` starting with "#" represents a new section.

Note that, script evaluation of `%{...}` is happened **before** quickmenu open, you can get  information from current document and display them in the menu, like current filename or word under cursor.

And `action` will be executed **after** quickmenu closed. All the `action` will affect current document (not the quickmenu window).

#### Clear items


```VimL
function quickmenu#reset()
```

This will reset the quickmenu

#### Set header

```VimL
function quickmenu#header(text)
``` 

Replace the origin title of `QuickMenu X.X.X`

#### Show / hide quickmenu

```VimL
function quickmenu#toggle(menuid)
```

You have unlimited menus to use, not only one. The `menuid` is used to indicate which menu to be displayed. 0 can be used by default. 

#### Set current menuid

```VimL
function quickmenu#current(menuid)
```

If current menu changed (which is 0 by default), `quickmenu#append()` will insert new items into the new menu and `quickmenu#reset()` will clear items in it either.


## Example

The configuration for the first GIF screencapture: 

```VimL

" clear all the items
call quickmenu#reset()

" enable cursorline (L) and cmdline help (H)
let g:quickmenu_options = "HL"

" use your favorite key to show / hide quickmenu
noremap <silent><F12> :call quickmenu#toggle(0)


" new section: empty action with text starts with "#" represent a new section
call quickmenu#append("# Debug", '')

" script between %{ and } will be evaluated before menu open
call quickmenu#append("Run %{expand('%:t')}", '!./%', "Run current file")


" new section
call quickmenu#append("# Git", '')

" use fugitive to show diff
call quickmenu#append("git diff", 'Gvdiff', "use fugitive's Gvdiff on current document")

call quickmenu#append("git status", 'Gstatus', "use fugitive's Gstatus on current document")


" new section
call quickmenu#append("# Misc", '')

call quickmenu#append("Turn paste %{&paste? 'off':'on'}", "set paste!", "enable/disable paste mode (:set paste!)")

call quickmenu#append("Turn spell %{&spell? 'off':'on'}", "set spell!", "enable/disable spell check (:set spell!)")

call quickmenu#append("Function List", "TagbarToggle", "Switch Tagbar on/off")

```

## Credits

Thanks to [vim-startify](https://github.com/mhinz/vim-startify), quickmenu uses its syntax and some idea. 

It is a great honor if you like it and star this repository or vote it in: http://www.vim.org/scripts/script.php?script_id=5589 


