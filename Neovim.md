## windows上的配置文件
所在目录为C:\Users\leiyu\AppData\Local\nvim\init.lua
## 配置文件目录
neovim可以将功能划分为不同的文件
目录结构
```
nvim
	- init.lua
	lua
		core
		   - options.lua
		   - keymaps.lua
		plugins
			- plugins-setup.lua
```
## 配置文件的选项
options.lua
```lua
local opt = vim.opt

-- 显示行号
vim.opt.number = true

-- 缩进
opt.expandtab = true
opt.autoindent = true

-- 防止包裹
opt.wrap = false

-- 光标行
opt.cursorline = true

-- 复制粘贴
opt.clipboard:append("unnamedplus")

-- 默认新窗口右和下
opt.splitright = true
opt.spllitbelow = true

-- 搜索
opt.ignorecase = true
opt.smartcase = true

-- 外观
opt.termguicolors = true
opt.signcolumn = "yes"
```
keymaps.lua
```lua
vim.g.mapleader = " "

local keymap = vim.keymap

-- ----------------插入模式------------------ ---
keymap.set("i", "jk", "<ESC>")

-- ----------------视觉模式------------------ ---
-- 单行或多行移动
keymap.set("v", "J", ":m '>+1<CR>gv=gv")
keymap.set("v", "K", ":m '>-2<CR>gv=gv")

-- ----------------正常模式------------------ ---
-- 窗口
keymap.set("n", "<leader>sv", "<C-w>v")  -- 水平新增窗口
keymap.set("n", "<leader>sh", "<C-w>s")  -- 垂直新增窗口

-- 取消高亮
keymap.set("n", "<leader>nh", ":nohl<CR>")
```
plugins-setup.lua
```lua
local ensure_packer = function()
  local fn = vim.fn
  local install_path = fn.stdpath('data')..'/site/pack/packer/start/packer.nvim'
  if fn.empty(fn.glob(install_path)) > 0 then
    fn.system({'git', 'clone', '--depth', '1', 'https://github.com/wbthomason/packer.nvim', install_path})
    vim.cmd [[packadd packer.nvim]]
    return true
  end
  return false
end

local packer_bootstrap = ensure_packer()

vim.cmd([[
	augroup packer_user-config
		autocmd!
		autocmd BufWritePost plugins-setup.lua source <afile> | PackerSync
	augroup end
]])

return require('packer').startup(function(use)
  use 'wbthomason/packer.nvim'
  -- My plugins here
  -- use 'foo1/bar1.nvim'
  -- use 'foo2/bar2.nvim'

  -- Automatically set up your configuration after cloning packer.nvim
  -- Put this at the end after all plugins
  if packer_bootstrap then
    require('packer').sync()
  end
end)
```
init.lua
```lua
require("plugins-setup.lua")
require("core.options")
require("core.keymaps")
```