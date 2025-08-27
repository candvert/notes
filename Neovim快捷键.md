```
:help nvim-quickstart

可以使用init.lua或init.vim作为配置文件，但同时只能存在一个


Enter                           打开高亮文本的链接
type K on any word to find its documentation
:echo stdpath('config') 输出配置文件所在目录

ruler选项  右下角可以看到光标所在行和列
wrapscan选项 使用/搜索时到文件末尾就结束，不循环搜索

:exe 'edit' stdpath('config') .. '/init.lua' 首次创建init.lua
:e $MYVIMRC 之后通过该命令便可修改init.lua


IDE的功能由LSP提供

:lua print("Hello!")  每条命令有自己的作用域和变量声明
:lua= 和:lua vim.print(...)相同
:source ~/myluafile.lua


vim.g:   global variables (g:)
vim.b:   variables for the current buffer (b:)
vim.w:   variables for the current window (w:)
vim.t:   variables for the current tabpage (t:)
vim.v:   predefined Vim variables (v:)
vim.env: environment variables defined in the editor session

vim.opt:        behaves like :set
vim.opt_global: behaves like :setglobal
vim.opt_local:  behaves like :setlocal

vim.o:  behaves like :set
vim.go: behaves like :setglobal
vim.bo: for buffer-scoped options
vim.wo: for window-scoped options (can be double indexed)


Mappings can be created using vim.keymap.set(). This function takes three
mandatory arguments:
• {mode} is a string or a table of strings containing the mode
  prefix for which the mapping will take effect. The prefixes are the ones
  listed in :map-modes, or "!" for :map!, or empty string for :map.
• {lhs} is a string with the key sequences that should trigger the mapping.
• {rhs} is either a string with a Vim command or a Lua function that should
  be executed when the {lhs} is entered.
  An empty string is equivalent to <Nop>, which disables a key.

Examples:
    -- Normal mode mapping for Vim command
    vim.keymap.set('n', '<Leader>ex1', '<cmd>echo "Example 1"<cr>')
    -- Normal and Command-line mode mapping for Vim command
    vim.keymap.set({'n', 'c'}, '<Leader>ex2', '<cmd>echo "Example 2"<cr>')
    -- Normal mode mapping for Lua function
    vim.keymap.set('n', '<Leader>ex3', vim.treesitter.start)
    -- Normal mode mapping for Lua function with arguments
    vim.keymap.set('n', '<Leader>ex4', function() print('Example 4') end)

vim.keymap.del('n', '<Leader>ex1')
vim.keymap.del({'n', 'c'}, '<Leader>ex2', {buffer = true})
```