# Vim Tips

I collected some tips on `vim` that I find helpful.  If you are new to `vim`, please try out the command `vimtutor` on any machine where `vim` is installed, and check out the nice article [Learn vim Progressively](
http://yannesposito.com/Scratch/en/blog/Learn-Vim-Progressively/).  

## 1. Useful Configuration

You can configure your `vim` by putting your configuration options and scripts in the `~/.vimrc` file (a hidden file named `.vimrc` in your home directory).  This file will be loaded whenever you start `vim`.

You can copy a sample `.vimrc` file from `~cs1010/.vimrc` to your home directory.
You can edit this file `~/.vimrc` just like any other file, using `vim`.

### Help

In `vim,` the command `:help <topic>` shows help about a particular topic in `vim`.  Example, `:help backup`.

### Backup Files

You can ask `vim` to automatically backup files that you edit.  This has been a lifesaver for me on multiple occasions.

In your `~/.vimrc` file,

```
set backup
```

will cause a copy of your file to be saved with suffix `~` appended to its name every time you save.

I prefer not to clutter my working directory, so I set

```
set backupdir=~/.backup
```

and create a directory named `~/.backup` to store my backup files.

So if you made changes to a file that you regretted, or if you accidentally deleted a file, you can check under `~/.backup` to see if the backup can save you.

### Syntax Highlighting

If for some reasons, syntax highlighting is not on by default, add this to your `~/.vimrc`:

```
syntax on
```

### Ruler and Numbers

If you prefer to show the line number you are on and the column number you are on, adding the commands to `~/.vimrc`

```
set ruler
```

will display the line number and the column number on the lower right corner.  

You can also add
```
set number
```

to label each line with a line number.

### Auto Indentation

Proper indentation is important to make your code readable (to yourself and others).  You should enable this in `vim` with:

```
set autoindent
set smartindent
```

Autoindent will cause the next line to have the same indentation as the previous line; while smartindent has some understanding of C-like syntax (such as recognizing `{` and `}`) and indent your code accordingly.  The size of the indentation is based on the setting `shiftwidth`.  For CS1010, please set it to either `2` or `4`:

```
set shiftwidth=2
```

## 2. Navigation

### Basic Navigation

Use ++k++ and ++j++ keys to move up and down (just like Gmail and Facebook!).  ++h++ and ++l++ to move left and right.

Other shortcuts (no need to memorize them now, just refer back when you feel like you are typing too many ++h++++j++++k++++l++ to see how you can navigate faster).

- ++w++ jump to the beginning of the next word
- ++b++ ump to the beginning of the previous word (reverse of `w`)
- ++e++ jump to the end of the word (or next word when pressed again)
- ++f++ char: search forward in the line and sit on the next matching char
- ++t++ char:  search forward in the line and sit on one space before the matching char
- ++shift+4++ ($) jump to the end of line
- ++0++ jump to the beginning of the line
- ++shift+6++ (^) jump to the first non-blank character of the line
- ++shift+5++ (%) jump between matching parentheses
- ++control+d++ jump forward (Down) half page
- ++control+f++ jump Forward one page
- ++control+u++ jump backward (Up) half page
- ++control+b++ jump Backward half page

### Jumping to a Line

If the compiler tells you there is an error on Line $x$, you can issue `:<x>` to jump to Line $x$.  For instance, `:40` will go to Line 40.

## 3. Editing Operations

### Undo

Since we are on the topic of correcting mistakes, ++u++ in command mode undo your changes.  Prefix it with a number $n$ to undo $n$ times.  If you want to undo your undo, ++control+r++ will redo.

### Navigation + Editing

`vim` is powerful because you can combine _operations_ with _navigation_.  For instance ++c++ to change, ++d++ to delete, ++y++ to yank (copy).  Since ++w++ is the navigation command to move over the current word, combining them we get:

- ++c++++w++ change the current word (delete the current word and enter insert mode)
- ++d++++w++ delete the current word
- ++y++++w++ yank the current word (copy word into buffer)

Can you guess what each of these do:

- ++d++++f++++shift+0++ 
- ++d++++f++++shift+0++ 
- ++c++++shift+4++
- ++y++++0++

If you repeat the operation ++c++, ++d++, and ++y++, it applies to the whole line, so:

- ++c++++c++ change the whole line
- ++d++++d++ delete the whole line
- ++y++++y++ yank the whole line

You can add a number before an operation to specify how many times you want to repeat an operation.  So ++5++++d++++d++  deletes 5 lines, ++5++++d++++w++ deletes 5 words, etc.

See the article [Operator, the True Power of `Vim`](http://whileimautomaton.net/2008/11/vimm3/operator) for more details.

### Swapping Lines

Sometimes you want to swap the order of two lines of code, in command mode, ++d++++d++++p++ will do the trick.  ++d++++d++ deletes the current line, ++p++ paste it after the current line, in effect swapping the order of the two lines.

### Commenting blocks of code

Sometimes we need to comment out a whole block of code in C for testing purposes. There are several ways to do it in `vim`:

- Place the cursor on the first line of the block of code you want to comment on.
- ++0++ to jump to the beginning of the line
- ++shift+v++ enter visual mode
- Use the arrow key to select the block of code you want to comment on.
- ++shift+i++ to insert at the beginning of the line (here, since we already selected the block, we will insert at the beginning of every selected)
- ++slash++++slash++ to insert the C comment character (you will see it inserted in the current line, but don't worry)
- ++escape++ to escape from the visual code.

To uncomment,

- Place the cursor on the first line of the block of code you want to comment.
- ++0++ to jump to the beginning of the line
- ++control+v++ enter block visual mode
- Use the arrow key to select the columns of text containing `//`
- ++x++ to delete them

## 4. Other Advanced Features

### Search and Replace in `vim`

```
:%s/oldWord/newWord/gc
```

`:` enters the command mode.  `%` means apply to the whole document, `s` means substitute, `g` means global (otherwise, only the first occurrence of each line is replaced). `c` is optional -- adding it cause `vim` to confirm with you before each replacement  

### Shell Command

If you need to issue a shell command quickly, you don't have to exit `vim`, run the command, and launch `vim` again.  You can use `!`,

```
:!<command>
```

will issue the command to shell.  E.g.,

```
:!ls
```

You can use this to compile your current file, without exiting `vim`.

```
:!make
```

`make` is actually a builtin command for `vim` so you can also simply run

```
:make
```

### Abbreviation

You can use the command `ab` to abbreviate frequently typed commands.  E.g., in your `~/.vimrc`,

```
ab pl cs1010_print_long(
```

Now, when you type `pl `, it will be expanded into `cs1010_print_long(`

### Auto-Completion

You can use ++control+p++ or ++control+n++ to auto-complete.  By default, the autocomplete dictionary is based on the text in your current editing buffers.  This is a very useful keystroke saver for long function and variable names.

### Auto-Indent the Whole File

You can ++g++++g++++equal++++shift+g++ in command mode to auto-indent the whole file.  ++g++++g++ is the command to go to the beginning of the file.  ++equal++ is the command to indent.  ++shift+g++ is the command to go to the end of the file.  

### Splitting `vim`'s Viewport

- `:sp file.c` splits the `vim` window horizontally
- `:vsp file.c` splits the `vim` window vertically
- ++control+w++++control+w++ moves between the different `vim` viewports

## 5. Color Schemes

{++NEW++}

We have installed [`vim-colorscheme`](https://github.com/flazz/vim-colorschemes) bundle under `~cs1010/.vim/vim-colorschemes/colors`.

There are a few steps needed.  First, the standard .vimrc is designed for 16 colors, and some of these color schemes are more colorful than that.  You need to comment the following three lines from your `~/.vimrc`

```
" The following should give 16 colors
" set t_AB=^[[%?%p1%{8}%<%t%p1%{40}%+%e%p1%{92}%+%;%dm
" set t_AF=^[[%?%p1%{8}%<%t%p1%{30}%+%e%p1%{82}%+%;%dm
" set t_Co=16
```

Then, run

```
mkdir -p ~/.vim
ln -s ~cs1010/.vim/vim-colorschemes/colors ~/.vim/colors
```

After that, your can change your vim color scheme as usual.  For instance,

```
:color gruvbox
```

You can add the line `color gruvbox` to your `~/.vimrc` so that the color scheme is loaded at the start of every vim session.

The bundle includes some of the popular color schemes among students, such as molokai, solarized, and gruvbox.   Some color schemes display differently depending on whether the background is set to `dark` or `light`

Some examples, with `set background=dark` in `~/.vimrc`:

The default color scheme:

![default](figures/color-scheme-default.png)

The molokai color scheme:

![molokai](figures/color-scheme-molokai.png)

The gruvbox color scheme 

![gruvbox](figures/color-scheme-gruvbox.png)

## 6. Recovery Files

{++NEW++} Vim automatically saves the files you are editing into temporary _swap_ files, with extension `.swp`.  These files are hidden so you don't normally see them when you run `ls`.  (You need to run `ls -a` to view the hidden files)

The swap files are useful if your editing session is disrupted before you save (e.g., the network is disconnected, you accidentally close the terminal, your OS crashes, etc.).

When you launch `vim` to edit a file, say, `foo.c`.  `vim` will check if a swap file `.foo.c.swp` exist.  If it does, `vim` with display the following

```
Found a swap file by the name ".foo.c.swp"
		  owned by: elsa   dated: Sat Aug 21 15:01:04 2021
		 file name: ~elsa/foo.c
          modified: no
		 user name: elsa   host name: pe116
        process ID: 7863 (STILL RUNNING)
While opening file "foo.c"
             dated: Mon Jul 12 18:38:37 2021

(1) Another program may be editing the same file.  If this is the case,
    be careful not to end up with two different instances of the same
    file when making changes.  Quit, or continue with caution.
(2) An edit session for this file crashed.
    If this is the case, use ":recover" or "vim -r a.c"
    to recover the changes (see ":help recovery").
    If you did this already, delete the swap file ".a.c.swp"
    to avoid this message.

Swap file ".a.c.swp" already exists!
[O]pen Read-Only, (E)dit anyway, (R)ecover, (Q)uit, (A)bort:
```

The messages above is self-explanatory.  Read it carefully.  Most of the time, you want to choose "R" to recover your edits, so that you can continue editing.  Remember to remove the file `.foo.c.swp` after you have recovered.  Otherwise `vim` will prompt you the above messages every time you edit `foo.c`.

Warning: if `foo.c` is newer than the state saved in `.foo.c.swp`, and you recover from `.foo.c.swp`, you will revert back to the state of the file as saved in the swap file.  This can happen if (i) you edit the file without recovery, or (ii) you recover the file, continue editing, but did not remove the `.foo.c.swp` file after.

## 7. Approved Plugins

{++UPDATED++}

The following plugins are approved for use during practical exams.  You will be given time to install them (only if you wish to use them) into your exam environment at the beginning of the practical exam.

### DelimitMate

`DelimitMate` is a plugin that automatically inserts a closing `}`, `)`, `>`, etc when you type the opening symbol.

To install this plugin in the exam environment, run
```
ln -s ~cs1010/.vim/pack/plugins/start/delimitMate.vim ~/.vim/pack/plugins/start
```

### Vim-Rainbow

`vim-rainbow` is a plugin that matches opening and closing brackets and colors the matching pairs with matching colors.

To install this plugin in the exam environment, run
```
ln -s ~cs1010/.vim/pack/plugins/start/vim-rainbow ~/.vim/pack/plugins/start
```

The following line will be added on your `~/.vimrc` in the environment:

```
let g:rainbow_active = 1
```

### NERDTree

NERDTree provides a file browsing pane on the left (activated with `:NERDTree`).

To install this plugin in the exam environment, run
```
ln -s ~cs1010/.vim/pack/plugins/start/nerdtree ~/.vim/pack/plugins/start
```

### Lightline

Lightline provides a more useful status line for `vim`.

To install this plugin in the exam environment, run
```
ln -s ~cs1010/.vim/pack/plugins/start/lightline.vim ~/.vim/pack/plugins/start
```

### Unavailable Plugins

The following plugins are not allowed as they provide too much help for writing Java programs.

- `coc`
- `syntastic`
- `youcompleteme`

## 8. Other Useful Plugins (Just Not for Exams)

### Syntax and Style Checker

I use `syntastic` to check for style and syntax whenever I save a file.  [`syntastic`](https://github.com/vim-syntastic/syntastic) is a `vim` plugin.

My `.vimrc` configuration file contains the following:

```
"For syntastic
set laststatus=2
set statusline+=%#warningmsg#
set statusline+=%{SyntasticStatuslineFlag()}
set statusline+=%*

let g:syntastic_error_symbol = '✗'
let g:syntastic_warning_symbol = '⚠'
let g:syntastic_always_populate_loc_list = 1
let g:syntastic_auto_loc_list = 1
let g:syntastic_check_on_open = 1
let g:syntastic_check_on_wq = 0

let g:syntastic_c_checkers = [ 'clang_tidy', 'clang' ]
let g:syntastic_c_compiler = 'clang'
let g:syntastic_c_clang_args = '-Wall -Werror -Wextra -Iinclude'
let g:syntastic_c_clang_tidy_args = '-checks=*'
let g:syntastic_c_compiler_options = '-Wall -Iinclude'
let g:syntastic_c_include_dirs = [ '../include', 'include' ]
let g:syntastic_c_clang_tidy_post_args = ""
```

By default, `clang-tidy` does not know where to find the header files.  So if you include non-standard C headers, it will complain that it cannot find headers.  To resolve this, we need to tell `clang-tidy` the compilation flags that we use when compiling our program.  

We can do this by creating a file named `compile_flags.txt` in your working directory (where your C files are located), containing one compilation flag per line.  For instance, if the header files are located in `/home/course/cs1010/include`, your `compile_flags.txt` should contain the following two lines:

```
-Wall
-I/home/course/cs1010/include
```

Note that `syntastic` is not one of the approved plugins during CS1010 practical exams.

