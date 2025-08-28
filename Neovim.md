## 配置文件
Linux上Neovim会默认读取~/.config/nvim/init.lua文件
Windows上会默认读取C:\Users\leiyu\AppData\Local\nvim\init.lua

理论上所有配置都可以放入init.lua中，但这样不是一个好的做法，因此我划分不同的文件和目录来分管不同的配置
首先看下按照本篇教程配置Nvim之后，目录结构看起来会是怎么样：
```
nvim
	- init.lua
	lua
		keymaps.lua
		options.lua
		plugins.lua
```
lua目录。当我们在Lua里面调用require加载模块（文件）的时候，它会自动在lua文件夹里面进行搜索
keymaps.lua配置按键映射
options.lua配置选项
plugins.lua配置插件
## 安装Neovim
```
Window上：
winget install Neovim.Neovim
```
在安装完成之后，可以在终端使用nvim命令进入nvim后输入下面命令创建init.lua
```
:exe 'edit' stdpath('config') .. '/init.lua'
```
可以通过如下命令查看配置文件所在目录
```
:echo stdpath('config') 输出配置文件所在目录
```
## 安装插件管理器
使用的是最流行的[lazy.nvim](https://lazy.folke.io/installation)
新建plugins.lua文件并填入如下内容
```lua
-- Bootstrap lazy.nvim
local lazypath = vim.fn.stdpath("data") .. "/lazy/lazy.nvim"
if not (vim.uv or vim.loop).fs_stat(lazypath) then
  local lazyrepo = "https://github.com/folke/lazy.nvim.git"
  local out = vim.fn.system({ "git", "clone", "--filter=blob:none", "--branch=stable", lazyrepo, lazypath })
  if vim.v.shell_error ~= 0 then
    vim.api.nvim_echo({
      { "Failed to clone lazy.nvim:\n", "ErrorMsg" },
      { out, "WarningMsg" },
      { "\nPress any key to exit..." },
    }, true, {})
    vim.fn.getchar()
    os.exit(1)
  end
end
vim.opt.rtp:prepend(lazypath)

-- Make sure to setup `mapleader` and `maplocalleader` before
-- loading lazy.nvim so that mappings are correct.
-- This is also a good place to setup other settings (vim.opt)
vim.g.mapleader = " "
vim.g.maplocalleader = "\\"

-- Setup lazy.nvim
require("lazy").setup({
  spec = {
    -- add your plugins here
  },
  -- Configure any other settings here. See the documentation for more details.
  -- colorscheme that will be used when installing plugins.
  install = { colorscheme = { "habamax" } },
  -- automatically check for plugin updates
  -- checker = { enabled = true },
})
```
在 lazy.nvim 中指定第三方插件很简单，只需要在 require("lazy").setup({ ... }) 里面声明插件
在 init.lua 文件里面导入
```lua
require("plugins")
```
现在打开 nvim 后就可以通过 :Lazy home 命令进入插件管理页面了
## 自动补全插件
使用的是[blink.cmp](https://github.com/saghen/blink.cmp)
在plugins.lua里新增这个插件
```lua
-- ...
-- 省略其他行
require("lazy").setup({
  spec = {
    -- add your plugins here
	-- 只用给出github作者和仓库名，lazy.nvim会自动补全github地址并下载安装
    { "saghen/blink.cmp", opts = {} },
  },
})
```
## LSP配置
配置完blink.cmp之后，已经有基本的自动补全功能，但和 IDE 相比，我们还需要定义跳转、代码补全等功能，因此需要配置 LSP，使用的工具是 [mason.nvim](https://github.com/mason-org/mason.nvim?tab=readme-ov-file#recommended-setup-for-lazynvim) 和 [mason-lspconfig.nvim](https://github.com/mason-org/mason.nvim?tab=readme-ov-file#recommended-setup-for-lazynvim)，他们的功能分别是
- mason.nvim: LSP 管理器，可以实现 LSP 的下载、更新等，管理起来比较方便
- mason-lspconfig.nvim: 主要功能是处理 mason.nvim 和 nvim-lspconfig 之间名字不一致的问题。比如，mason.nvim 里面管 Lua 语言的 LSP 叫做 lua-language-server，但是 nvim-lspconfig 叫做 lua_ls。另外，mason-lspconfig.nvim 还会自动调用 vim.lsp.enable 启动安装好的 LSP服务器
修改 plugins.lua 文件，增加这两个插件
```lua
-- ...
-- 省略其他行
require("lazy").setup({
  spec = {
	{ "mason-org/mason.nvim", opts = {} },
    {
        "mason-org/mason-lspconfig.nvim",
		opts = {},
        dependencies = {
            { "mason-org/mason.nvim", opts = {} },
            "neovim/nvim-lspconfig",
        },
    },
  },
})
```
对于Windows系统，mason.nvm要求必须有powershell、git、GNU tar、[7zip](https://www.7-zip.org/)
现在打开 nvim 后就可以通过 :Mason 命令进行 LSP 服务器的安装了
![[neovim_01.png]]
在光标所在行按 i 进行 LSP 服务器的安装
需要注意安装 LSP 服务器前需要相关语言的编译器能在path中找到，比如在安装 python 的 LSP 服务器前需要将 python.exe 放入 path 中
## LSP客户端配置
使用的工具是 [nvim-lspconfig](https://github.com/neovim/nvim-lspconfig)
修改 plugins.lua 文件，增加该插件
```lua
-- ...
-- 省略其他行
require("lazy").setup({
  spec = {
	{
		"neovim/nvim-lspconfig",
		config = function()
			-- 通过lspconfig.<server_name>.setup()配置特定服务器
			--（如 lspconfig.tsserver.setup()配置 TypeScript 服务器）
			local lspconfig = require("lspconfig")
			lspconfig.pylsp.setup({})
		end,
	},
  },
})
```
## 配置文件的选项
options.lua
```lua
-- Hint: use `:h <option>` to figure out the meaning if needed
vim.opt.clipboard = 'unnamedplus' -- use system clipboard
vim.opt.completeopt = { 'menu', 'menuone', 'noselect' }
vim.opt.mouse = 'a' -- allow the mouse to be used in Nvim

-- Tab
vim.opt.tabstop = 4 -- number of visual spaces per TAB
vim.opt.softtabstop = 4 -- number of spacesin tab when editing
vim.opt.shiftwidth = 4 -- insert 4 spaces on a tab
vim.opt.expandtab = true -- tabs are spaces, mainly because of python

-- UI config
vim.opt.number = true -- show absolute number
vim.opt.relativenumber = true -- add numbers to each line on the left side
vim.opt.cursorline = true -- highlight cursor line underneath the cursor horizontally
vim.opt.splitbelow = true -- open new vertical split bottom
vim.opt.splitright = true -- open new horizontal splits right
-- vim.opt.termguicolors = true        -- enabl 24-bit RGB color in the TUI
vim.opt.showmode = false -- we are experienced, wo don't need the "-- INSERT --" mode hint

-- Searching
vim.opt.incsearch = true -- search as characters are entered
vim.opt.hlsearch = false -- do not highlight matches
vim.opt.ignorecase = true -- ignore case in searches by default
vim.opt.smartcase = true -- but make it case sensitive if an uppercase is entered
```
keymaps.lua
```lua
-- define common options
local opts = {
    noremap = true,      -- non-recursive
    silent = true,       -- do not show message
}

-----------------
-- Normal mode --
-----------------

-- Hint: see `:h vim.map.set()`
-- Better window navigation
vim.keymap.set('n', '<C-h>', '<C-w>h', opts)
vim.keymap.set('n', '<C-j>', '<C-w>j', opts)
vim.keymap.set('n', '<C-k>', '<C-w>k', opts)
vim.keymap.set('n', '<C-l>', '<C-w>l', opts)

-- Resize with arrows
-- delta: 2 lines
vim.keymap.set('n', '<C-Up>', ':resize -2<CR>', opts)
vim.keymap.set('n', '<C-Down>', ':resize +2<CR>', opts)
vim.keymap.set('n', '<C-Left>', ':vertical resize -2<CR>', opts)
vim.keymap.set('n', '<C-Right>', ':vertical resize +2<CR>', opts)

-----------------
-- Visual mode --
-----------------

-- Hint: start visual mode with the same area as the previous area and the same mode
vim.keymap.set('v', '<', '<gv', opts)
vim.keymap.set('v', '>', '>gv', opts)
```
init.lua
```lua
require("options")
require("keymaps")
require("plugins")
```
到目前为止，便完成了基础的配置。之后可以按自己的喜好添加插件
## 语法高亮
首先需要安装[nvim-treesitter](https://github.com/nvim-treesitter/nvim-treesitter)，主要功能是进行语法分析，生成语法树。之后便可以通过其他插件进行语法高亮。
修改 plugins.lua 文件，增加该插件
```lua
-- ...
-- 省略其他行
require("lazy").setup({
  spec = {
	{
		"nvim-treesitter/nvim-treesitter",
		build = ":TSUpdate",
		config = function () 
		  local configs = require("nvim-treesitter.configs")

		  configs.setup({
			  ensure_installed = { "python" }, -- 在这里添加使用的语言
			  sync_install = false,
			  highlight = { enable = true },
			  indent = { enable = true },  
			})
		end
	}
  },
})
```
安装[nvim](https://github.com/catppuccin/nvim)颜色主题
修改 plugins.lua 文件，增加该插件
```lua
require("lazy").setup({
  spec = {
	{ "catppuccin/nvim", name = "catppuccin", priority = 1000 },
  },
})
```
在 init.lua 文件里面设置一下
```lua
-- ...
-- 省略其他行

-- 白日主题
vim.cmd[[colorscheme catppuccin-latte]]
-- 暗色主题
-- vim.cmd[[colorscheme catppuccin]]
```
## 查找文件
使用的是[telescope](https://github.com/nvim-telescope/telescope.nvim)
```lua
require("lazy").setup({
  spec = {
	{
		'nvim-telescope/telescope.nvim',
		dependencies = { 
			'nvim-lua/plenary.nvim',
			'nvim-treesitter/nvim-treesitter'
		}
    },
  },
})
```
可以使用 :checkhealth telescope 命令查看安装情况
## 文件explorer
使用的是[nvim-tree](https://github.com/nvim-tree/nvim-tree.lua)
```lua
require("lazy").setup({
  spec = {
	{
		"nvim-tree/nvim-tree.lua",
		version = "*",
		lazy = false,
		dependencies = {
			"nvim-tree/nvim-web-devicons",
		},
		config = function()
			require("nvim-tree").setup {}
		end,
	}
  },
})
```
可以使用 :NvimTreeOpen 命令打开文件explorer
## 常用命令
```
输出配置文件所在目录
:echo stdpath('config')
```
