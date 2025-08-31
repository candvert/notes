```
完全使用vimscript编写，没有任何额外依赖
加载只需不到1微秒
帮助文档:h airline
状态栏可以分为多个section，每个section可以分为多个part

:h airline-default-sections

启用字体，需要安装 Powerline 字体或 Nerd Fonts
let g:airline_powerline_fonts = 1

更改主题
:AirlineTheme simple
let g:airline_theme = '<theme>'


标签栏
-- 启用airline的标签栏
vim.g['airline#extensions#tabline#enabled'] = 1
-- 是否显示右侧的"buffers"
vim.g['airline#extensions#tabline#show_buffers'] = 0
-- 是否显示左侧的"tab"
vim.g['airline#extensions#tabline#show_tabs'] = 0
-- 是否显示左边的"tab"和右边的"tab"数量
vim.g['airline#extensions#tabline#show_tab_type'] = 0
vim.g['airline#extensions#tabline#show_tab_count'] = 0



设置不同模式显示的文本：
let g:airline_mode_map = {
  \ '__'     : '-',
  \ 'c'      : 'C',
  \ 'i'      : 'I',
  \ 'ic'     : 'I',
  \ 'ix'     : 'I',
  \ 'n'      : 'N',
  \ 'multi'  : 'M',
  \ 'ni'     : 'N',
  \ 'no'     : 'N',
  \ 'R'      : 'R',
  \ 'Rv'     : 'R',
  \ 's'      : 'S',
  \ 'S'      : 'S',
  \ ''     : 'S',
  \ 't'      : 'T',
  \ 'v'      : 'V',
  \ 'V'      : 'V',
  \ ''     : 'V',
  \ }


:AirlineToggle 切换标准状态栏和airline


Airline 的状态栏从左到右分为 a, b, c (左半部分) 和 x, y, z (右半部分)
airline的不同部分及其显示的内容
let g:airline_section_a       (mode, crypt, paste, spell, iminsert, executable)
let g:airline_section_b       (hunks, branch)[*]
let g:airline_section_c       (bufferline or filename, readonly)
let g:airline_section_gutter  (csv)
let g:airline_section_x       (tagbar, filetype, virtualenv)
let g:airline_section_y       (fileencoding, fileformat, 'bom', 'eol')
let g:airline_section_z       (percentage, line number, column number)
let g:airline_section_error   (ycm_error_count, syntastic-err, eclim,
							 languageclient_error_count)
let g:airline_section_warning (ycm_warning_count, syntastic-warn,
							 languageclient_warning_count, whitespace)
更改显示内容
let g:airline_section_c = '%t'

在airline_section_y添加part foo
let g:airline_section_y = airline#section#create_right(['ffenc','foo'])

let g:airline_section_b = airline#section#create(['branch', ' ', 'hunks'])

预定义的parts
* `mode`         displays the current mode
* `iminsert`     displays the current insert method
* `paste`        displays the paste indicator
* `crypt`        displays the crypted indicator
* `exectuable`   displays the executable indicator
* `spell`        displays the spell indicator
* `filetype`     displays the file type
* `readonly`     displays the read only indicator
* `file`         displays the filename and modified indicator
* `path`         displays the filename (absolute path) and modifier indicator
* `linenr`       displays the current line number
* `maxlinenr`    displays the number of lines in the buffer
* `ffenc`        displays the file format and encoding

将part foo定义为红色
call airline#parts#define_accent('foo', 'red')

bold, italic, red, green, blue, yellow, orange, purple, none

定义section和整个stausline
function! AirlineInit()
	let g:airline_section_a = airline#section#create(['mode', ' ', 'foo'])
	let g:airline_section_b = airline#section#create_left(['ffenc','file'])
	let g:airline_section_c = airline#section#create(['%{getcwd()}'])
endfunction
autocmd User AirlineAfterInit call AirlineInit()
```