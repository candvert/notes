- [配置文件](#配置文件)
- [安装Neovim](#安装Neovim)
- [安装插件管理器](#安装插件管理器)
- [自动补全插件](#自动补全插件)
- [LSP服务器端配置](#LSP服务器端配置)
- [LSP客户端配置](#LSP客户端配置)
- [options.lua文件](#options.lua文件)

- [安装NerdFont字体](#安装NerdFont字体)
- [语法高亮](#语法高亮)
- [查找文件](#查找文件)
- [文件explorer](#文件explorer)
- [按键映射函数](#按键映射函数)
- [Leader键](#Leader键)
- [使用vimscript](#使用vimscript)

- [nvim-lspconfig配置](#nvim-lspconfig配置)
- [Linter](#Linter)
- [Dap](#Dap)
- [插件](#插件)
	- [lazy.nvim](#lazy.nvim)
	- [lualine.nvim](#lualine.nvim)
	- [bufferline.nvim](#bufferline.nvim)
	- [nvim-surround](#nvim-surround)
	- [mini.pairs](#mini.pairs)
	- [indent-blankline](#indent-blankline)
	- [vim-airline](vim-airline.md)
	- [leap.nvim](#leap.nvim)
	- [rainbow-delimiters.nvim](#rainbow-delimiters.nvim)
	- [none-ls](#none-ls)
	- [lspsaga.nvim](#lspsaga.nvim)
	- [toggleterm.nvim](#toggleterm.nvim)
配置完成之后每增加一门语言的支持就要进行下面三项。
一个是 LSP 服务器，提供代码提示等功能。通过 Mason 插件安装。命令为 :Mason。
一个是语法解析器，提供语法高亮等功能。通过修改 nvim-treesitter 配置文件安装。
该语言的编译器，要添加进 path 中。
## 配置文件
Linux上Neovim会默认读取~/.config/nvim/init.lua文件
Windows上会默认读取C:\Users\leiyu\AppData\Local\nvim\init.lua

理论上所有配置都可以放在 init.lua 中，但这样不是一个好的做法，因此划分不同的文件和目录来放不同的配置
我的目录结构：
```sh
nvim
	- init.lua
	lua
		options.lua
		plugins
			- blink.lua
			- mason.lua
			- mason_lspconfig.lua
			- nvim_lspconfig.lua
			- treesitter.lua
			- telescope.lua
			- nvim_tree.lua
			- ...
```
## 安装Neovim
```sh
Window上：
winget install Neovim.Neovim
```
在安装完成之后，可以在终端使用 nvim 命令进入 nvim，然后输入下面命令创建 init.lua
```sh
:exe 'edit' stdpath('config') .. '/init.lua'
```
之后可以通过下面命令打开 init.lua 配置文件
```sh
:e $MYVIMRC
```
可以通过如下命令查看配置文件所在目录
```sh
:echo stdpath('config')
```
## 安装插件管理器
使用的是最流行的[lazy.nvim](https://lazy.folke.io/installation)
在 init.lua 文件中写入如下内容
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
    { import = "plugins" },
  },
  -- Configure any other settings here. See the documentation for more details.
  -- colorscheme that will be used when installing plugins.
  install = { colorscheme = { "habamax" } },
  -- automatically check for plugin updates
  -- checker = { enabled = true },
})
```
现在打开 nvim 后就可以通过 :Lazy 命令进入插件管理页面了
## 自动补全插件
使用的是[blink.cmp](https://github.com/saghen/blink.cmp)
新增plugins/blink.lua文件
```lua
return {
	'saghen/blink.cmp',
	dependencies = { 'rafamadriz/friendly-snippets' },
	version = '1.*',
	opts = {
		-- 设置 tab 键确认补全
		keymap = { preset = 'super-tab' },
		-- 总是显示文档
		completion = { documentation = { auto_show = true } },
		sources = {
		  -- 补全信息来源
		  default = { 'lsp', 'path', 'snippets', 'buffer' },
		},
	},
}
```
## LSP服务器端配置
配置完 blink.cmp 之后，已经有了基本的自动补全功能，但和 IDE 相比，我们还需要定义跳转、代码补全等功能，因此需要配置 LSP。使用的工具是 [mason.nvim](https://github.com/mason-org/mason.nvim?tab=readme-ov-file#recommended-setup-for-lazynvim) 和 [mason-lspconfig.nvim](https://github.com/mason-org/mason.nvim?tab=readme-ov-file#recommended-setup-for-lazynvim)，他们的功能分别是
- mason.nvim: LSP 管理器，可以实现 LSP 的下载、更新等
- mason-lspconfig.nvim: 主要功能是处理 mason.nvim 和 nvim-lspconfig 之间名字不一致的问题。比如，mason.nvim 里面管 Lua 语言的 LSP 叫做 lua-language-server，但是 nvim-lspconfig 叫做 lua_ls。另外，mason-lspconfig.nvim 还会自动调用 vim.lsp.enable 启动安装好的 LSP服务器
新增plugins/mason.lua文件
```lua
return { "mason-org/mason.nvim", opts = {} }
```
新增plugins/mason_lspconfig.lua文件
```lua
return {
	"mason-org/mason-lspconfig.nvim",
	opts = {},
	dependencies = {
		{ "mason-org/mason.nvim", opts = {} },
		"neovim/nvim-lspconfig",
	},
}
```
对于 Windows 系统，使用 mason.nvim 则必须要已安装 powershell、git、GNU tar、[7zip](https://www.7-zip.org/)
现在打开 nvim 后就可以通过 :Mason 命令进行 LSP 服务器的安装了
![](/images/neovim_01.png)
在光标所在行按 i 进行 LSP 服务器的安装
需要注意安装 LSP 服务器前需要相关语言的编译器能在 path 中找到，比如在安装 python 的 LSP 服务器前需要将 python.exe 放入 path 中
## LSP客户端配置
使用的工具是 [nvim-lspconfig](https://github.com/neovim/nvim-lspconfig)
新增plugins/nvim_lspconfig.lua文件
```lua
return {
	"neovim/nvim-lspconfig",
	config = function()
		-- 可以在 https://github.com/neovim/nvim-lspconfig/blob/master/doc/configs.md 找到启动相应 LSP 配置的语句和相关配置选项
		-- 由于 mason-lspconfig 会自动调用 vim.lsp.enable，所以下面代码不需要
		--vim.lsp.enable('lua_ls')
		--vim.lsp.enable('pylsp')

		-- lua_ls 和 pylsp 不需要进行相应配置就可以使用
		-- 但是 ts_ls，也就是 typescript 的 LSP 客户端需要进行配置
		vim.lsp.config['ts_ls'] = {
			cmd = { 'typescript-language-server', '--stdio' },
			filetypes = { "javascript", "javascriptreact", "javascript.jsx", "typescript", "typescriptreact", "typescript.tsx" },
			root_dir = function(bufnr, on_dir)
				local root_markers = { 'package-lock.json', 'yarn.lock', 'pnpm-lock.yaml', 'bun.lockb', 'bun.lock' }
				root_markers = vim.fn.has('nvim-0.11.3') == 1 and { root_markers, { '.git' } }
				or vim.list_extend(root_markers, { '.git' })
				local project_root = vim.fs.root(bufnr, root_markers) or vim.fn.getcwd()
				on_dir(project_root)
			end,
		}
	end,
}
```
之后打开编程语言的文件，就可以通过 :checkhealth vim.lsp 命令查看相应 LSP 服务器是否启动
## options.lua文件
```lua
-- 可以将vim中复制的内容粘贴到如浏览器等其他地方
vim.opt.clipboard = 'unnamedplus'

-- 是否将tab转换为空格
vim.opt.expandtab = true
-- 将tab转换为4个空格
vim.opt.tabstop = 4
-- 自动缩进的空格数量
vim.opt.shiftwidth = 4

-- 忽略大小写
vim.opt.ignorecase = true
-- 智能，如果不输入大写字符，则都搜索。如果输入了大写字符，则只匹配有大写字符的
-- 比如搜索vary，则vary和Vary都可以；但若搜索Vary，则只查找Vary
vim.opt.smartcase = true
-- 搜索的内容不进行高亮显示
vim.opt.hlsearch = false

vim.opt.incsearch = true

-- 不在底部显示NORMAL、INSERT等模式
vim.opt.showmode = false

-- 显示行号
vim.opt.number = true
 -- 显示相对行号
vim.opt.relativenumber = true
 -- 高亮光标所在行
vim.opt.cursorline = true
-- 让新窗格出现在下方
vim.opt.splitbelow = true
-- 让新窗格出现在右方
vim.opt.splitright = true

-- 在其他地方进行的文件的更改自动更新
vim.opt.autoread = true
```
然后在 init.lua 中加入该句代码
```lua
require('options')
```
到目前为止，便完成了基础的配置。之后可以按自己的需求添加相应插件
## 安装NerdFont字体
在[自动补全插件](#自动补全插件)部分安装的 blink.cmp 插件在进行补全提示时，如果图标不能正常显示，则需要安装[Nerd Font](https://www.nerdfonts.com/font-downloads)字体
我安装的是 JetBrainsMono Nerd Font，具体安装的是其中的JetBrainsMonoNerdFontMono-Light.ttf
JetBrainsMonoNerdFontMono-LightItalic.ttf
JetBrainsMonoNerdFontMono-Regular.ttf
JetBrainsMonoNerdFontMono-Italic.ttf
JetBrainsMonoNerdFontMono-Medium.ttf
JetBrainsMonoNerdFontMono-MediumItalic.ttf
JetBrainsMonoNerdFontMono-Bold.ttf
JetBrainsMonoNerdFontMono-BoldItalic.ttf
## 语法高亮
首先需要安装 [nvim-treesitter](https://github.com/nvim-treesitter/nvim-treesitter)，主要功能是进行语法分析，生成语法树。之后便可以通过其他插件进行语法高亮。
新增 plugins/treesitter.lua 文件
```lua
return {
	"nvim-treesitter/nvim-treesitter",
	branch = 'master',
	lazy = false,
	build = ":TSUpdate",
	
	config = function () 
	  local configs = require("nvim-treesitter.configs")

	  configs.setup({
		  -- 在这里添加要语法高亮的编程语言
		  ensure_installed = { "python", "lua", "typescript", "css", "html" },
		  sync_install = false,
		  highlight = { enable = true },
		  indent = { enable = true },  
		})
	end
}
```
安装 [tokyonight](https://github.com/folke/tokyonight.nvim)颜色主题
新增plugins/tokyonight.lua文件
```lua
return {
	"folke/tokyonight.nvim",
	lazy = false,
	priority = 1000,
	opts = {},
}
```
在 init.lua 文件里面设置主题
```lua
-- ...
-- 省略其他行

vim.cmd[[colorscheme tokyonight]]
--vim.cmd[[colorscheme tokyonight-night]]
--vim.cmd[[colorscheme tokyonight-storm]]
--vim.cmd[[colorscheme tokyonight-day]]
--vim.cmd[[colorscheme tokyonight-moon]]
```
## [[查找文件]]
## 文件explorer
使用的是 [nvim-tree](https://github.com/nvim-tree/nvim-tree.lua)
新增plugins/nvim_tree.lua文件
```lua
return {
	"nvim-tree/nvim-tree.lua",
	version = "*",
	dependencies = {
		"nvim-tree/nvim-web-devicons",
	},
	keys = {
		{ "<leader>uf", ":NvimTreeToggle<CR>" },
	},
	config = function()
		require("nvim-tree").setup {
            view = { 
				-- 窗口宽度
                width = 24,
            },
        }
	end,
}
```
可以使用 :NvimTreeOpen 命令打开文件explorer

快捷键：
```sh
使用 :NvimTreeOpen 命令或快捷键 <leader>uf 打开文件explorer
下面都是打开文件explorer之后使用的快捷键
g?                                   显示所有快捷键

双击左键                              展开/折叠文件夹或打开文件
<CR>                                 即Enter键，作用同上
o                                    作用同上
Tab                                  展开/折叠文件夹或预览文件（焦点仍在文件树）
Ctrl + e                             展开/折叠文件夹或直接在文件树的窗口打开文件

Ctrl + ]                             切换到该目录，作用和cd命令相同
Ctrl + k                             显示文件信息

>                                    移动到下一个文件或文件夹
<                                    移动到上一个文件或文件夹
J                                    跳转到文件夹中最后一个文件或文件夹
K                                    跳转到文件夹中第一个文件或文件夹
P                                    跳转到父级目录
]c                                   跳转到下一个被git标记为更改的文件
[c                                   跳转到上一个被git标记为更改的文件
]e                                   跳转到下一个存在代码问题的文件
[e                                   跳转到上一个存在代码问题的文件

f                                    输入并应用过滤条件
F                                    清除过滤条件

a                                    创建文件
d                                    删除文件
E                                    展开所有文件
W                                    折叠所有文件

S                                    搜索文件或文件夹

m                                    标记文件
M                                    显示/隐藏标记文件
bd                                   删除标记文件
bmv                                  移动标记文件
C                                    只显示被git标记为更改的文件

c                                    复制文件
x                                    剪切文件
p                                    粘贴文件

r                                    重命名
e                                    重命名（不改变扩展名）
u                                    重命名（可以重命名整个路径，包含扩展名）
Ctrl + r                             重命名（可以重命名整个路径）

Ctrl + t                             将文件打开到一个新Tab
Ctrl + x                             将文件打开到右侧下方窗口

H                                    显示/隐藏隐藏文件
I                                    显示/隐藏被gitignore的文件或文件夹

y                                    复制文件名
ge                                   复制文件名（不包含扩展名）
Y                                    复制相对路径
gy                                   复制绝对路径
q                                    关闭文件树窗口

s                                    运行脚本文件，或用外部编辑器打开文件
.                                    运行命令
```
## 按键映射函数
```lua
-- mode参数表示哪种模式，n为普通模式，i为插入模式，v为视图模式，c为命令行模式
-- lhs为要更改的按键
-- rhs为更改后的按键或一个lua函数或自定义命令
-- opts为一些选项
vim.keymap.set({mode}, {lhs}, {rhs}, {opts})

-- 普通模式和编辑模式下按Ctrl + a再b就运行:lua print('hello world')
-- <Cmd>表示进入命令模式，<CR>表示命令输入完之后的回车键
vim.keymap.set({ "n", "i" }, "<C-a>b", "<Cmd>lua print('hello world')<CR>", { silent = true })

-- 将:help命令设置为快捷键good
vim.keymap.set('n', 'good', ':help<CR>')
```
## Leader键
```sh
Leader 键是一个自定义快捷键前缀
Leader 键本身不执行操作，而是作为后续按键组合的前缀（例如 <leader>ff）。它允许用户创建大量不与默认快捷键冲突的自定义快捷键

设置 Leader 键的代码，一般设置为空格
vim.g.mapleader = " "
```
## 使用vimscript
```lua
-- vim.cmd可以执行任何vim命令
vim.cmd('set number')

-- 通过 vim.fn 可以调用 Vim 函数，返回值会自动转换为 Lua 的数据类型。
local current_line = vim.fn.getline('.')

-- vim.call，这与 vim.fn 类似，是调用函数的另一种方式
local result = vim.call('my#vimscript#function')
```
## nvim-lspconfig配置
```lua
-- Neovim 包含一个 lua 模块去操作 LSP 客户端，即 vim.lsp

-- 使用vim.lsp.config()为 LSP 客户端设置配置
-- 示例：
vim.lsp.config['luals'] = {
	-- Command and arguments to start the server.
	cmd = { 'lua-language-server' },
	
	-- 应用的文件类型
	filetypes = { 'lua' },
	
	-- 设置根目录，同一根目录下的文件连接同一个 LSP 服务器。
	-- root_markers 是否需要取决于不同的 LSP 服务器
	-- 该句代码意思是不断寻找当前文件的父级目录，直到找到.luarc.json、.luarc.jsonc或.git，将其作为根目录，即root_markers。
	root_markers = { { '.luarc.json', '.luarc.jsonc' }, '.git' },
	
	-- 发送给 LSP 服务器的设置，格式由 LSP 服务器定义，例如 lua-language-server 的可以在 https://raw.githubusercontent.com/LuaLS/vscode-lua/master/setting/schema.json 找到
	settings = {
	 Lua = {
	   runtime = {
		 version = 'LuaJIT',
	   }
	 }
	}
}


-- nvim-0.11.3 版本的 typescript 的 LSP 客户端配置
vim.lsp.config['ts_ls'] = {
	cmd = { 'typescript-language-server', '--stdio' },
	filetypes = { "javascript", "javascriptreact", "javascript.jsx", "typescript", "typescriptreact", "typescript.tsx" },
	root_dir = function(bufnr, on_dir)
		local root_markers = { 'package-lock.json', 'yarn.lock', 'pnpm-lock.yaml', 'bun.lockb', 'bun.lock' }
		root_markers = vim.fn.has('nvim-0.11.3') == 1 and { root_markers, { '.git' } }
		or vim.list_extend(root_markers, { '.git' })
		local project_root = vim.fs.root(bufnr, root_markers) or vim.fn.getcwd()
		on_dir(project_root)
	end,
}


-- 使用vim.lsp.enable()函数启用配置
vim.lsp.enable('luals')

-- 查看 LSP 服务器是否启动
:checkhealth vim.lsp
```
## Linter
```lua
-- Linter 是一款​​静态代码分析工具​​。它会在代码执行前对其进行分析，能帮你找出语法错误、风格不一致、潜在的逻辑问题以及不符合最佳实践的代码
```
## Dap
```lua
-- 通过 Mason 安装的 DAP 调试适配器，使得你能够直接在 Neovim 中​​设置断点、单步调试、查看变量调用栈​​等，为多种编程语言提供统一的调试界面。
```
## 插件
## lazy.nvim
```lua
-- 添加插件时其他插件配置文件的形式
return { 
	"xxx/xxx",
	opts = {}
}
-- opts其实就相当于lazy.nvim自动为我们调用了该函数：
-- require("xxx").setup(opts)
-- 当然我们也可以手动调用该函数：
-- config = function(_, opts)
--     require("xxx").setup(opts)
-- end


-- 像主题类的插件一般只需要加载，不需要setup
-- 有opts选项的插件lazy.nvim会自动setup，或者没有opts选项自己在config中调用setup
-- 也就是说主题类可以直接return { "xxx/xxx" }就可以生效
-- 而必须要setup才能能生效的插件要return { "xxx/xxx", opts = {} }或者
-- return { "xxx/xxx",
-- config = function(_, opts)
--     require("xxx").setup(opts)
-- end }



-- 插件懒加载，插件的配置文件设置了events, commands, ft, keys中之一后则会懒加载
-- event：在某个事件触发的时候加载插件
-- cmd：在某个命令被执行的时候加载插件
-- ft：当前 buffer 为特定文件类型的时候加载插件
-- keys：当触发快捷键时加载插件，如果快捷键不存在则创建快捷键
-- 可以使用 lazy = false 强制不进行懒加载
return {
	"xxx/xxx",
	opts = {},
	event = "InsertEnter",
	keys = {
		{ "<leader>bh", ":BufferLineCyclePrev<CR>", silent = true },
	},
}
```
## lualine.nvim
```lua
return {
    'nvim-lualine/lualine.nvim',
    dependencies = { 'nvim-tree/nvim-web-devicons' },
    opts = {
		-- 对 nvim-tree 插件窗口的显示做优化
		extensions = { "nvim-tree" },
	},
}
```
## bufferline.nvim
```lua
return {
	"akinsho/bufferline.nvim",
	dependencies = {
		"nvim-tree/nvim-web-devicons",
	},
	opts = {},
	keys = {
		{ "<leader>bh", ":BufferLineCyclePrev<CR>", silent = true },
		{ "<leader>bl", ":BufferLineCycleNext<CR>", silent = true },
		{ "<leader>bd", ":bdelete<CR>", silent = true },
		{ "<leader>bo", ":BufferLineCloseOthers<CR>", silent = true },
		{ "<leader>bp", ":BufferLinePick<CR>", silent = true },
		{ "<leader>bc", ":BufferLinePickClose<CR>", silent = true },
	},
	lazy = false,
}
```

```sh
点击关闭图标或右键关闭buffer
:h bufferline.nvim
:h bufferline-configuration
```
## nvim-surround
```lua
return {
    "kylechui/nvim-surround",
    event = "VeryLazy",
    opts = {},
}
```

```lua
-- 如何使用该插件:h nvim-surround.usage
-- 添加括号：ys{motion}{char}
-- 比如ysiw)
-- 删除括号：ds{char}
-- 比如ds]
-- 更改括号：cs{target}{replacement}
-- 比如cs'"
```
## mini.pairs
```lua
return {
	'nvim-mini/mini.pairs',
	version = '*',
	opts = { },
	event = "InsertEnter",
}
```
## indent-blankline
```lua
return {
    "lukas-reineke/indent-blankline.nvim",
    main = "ibl",
    opts = {},
}
```
## leap.nvim
```lua
return {
	'ggandor/leap.nvim',
	opts = {},
    keys = {
        { "s", "<Plug>(leap)", mode = { 'n', 'x', 'o' } },
    },
	config = function()
		require('leap').opts.safe_labels = {}
	end,
}
```
## rainbow-delimiters.nvim
```lua
return {
	"HiPhish/rainbow-delimiters.nvim",
	opts = {},
	config = function()
		require('rainbow-delimiters.setup').setup {
			
		}
	end,
}
```
## none-ls
```lua
return {
	"nvimtools/none-ls.nvim",
	dependencies = { "nvim-lua/plenary.nvim" },
	event = "VeryLazy",
    keys = {
        { "<leader>lf", vim.lsp.buf.format },
    },
	config = function()
		local null_ls = require("null-ls")
		null_ls.setup({
			sources = {
				null_ls.builtins.formatting.prettierd
			},
		})
	end,
}
```
## lspsaga.nvim
```lua
return {
    'nvimdev/lspsaga.nvim',
	dependencies = {
		'nvim-treesitter/nvim-treesitter', -- optional
		'nvim-tree/nvim-web-devicons',     -- optional
	},
	keys = {
		{ "<leader>lr", ":Lspsaga rename<CR>" },
		{ "<leader>lc", ":Lspsaga code_action<CR>" },
		{ "<leader>ld", ":Lspsaga goto_definition<CR>" },
		{ "<leader>lh", ":Lspsaga hover_doc<CR>" },
		{ "<leader>lR", ":Lspsaga finder<CR>" },
		{ "<leader>ln", ":Lspsaga diagnostic_jump_next<CR>" },
		{ "<leader>lp", ":Lspsaga diagnostic_jump_prev<CR>" },
	},
    config = function()
        require('lspsaga').setup({})
    end,

}
```
## toggleterm.nvim
```lua
return {
	'akinsho/toggleterm.nvim',
	version = "*",
	config = function()
		require("toggleterm").setup({
			-- 快捷键 Ctrl + \ 打开终端
			open_mapping = [[<c-\>]],
			--hide_numbers = true,
			--shade_terminals = true,
			--shading_factor = 2,
			--start_in_insert = true,
			--insert_mappings = true,
			--persist_size = true,
			direction = "float",
			--close_on_exit = true,
			--shell = vim.o.shell,
			--float_opts = {
			--	border = "curved",
			--	winblend = 0,
			--	highlights = {
			--		border = "Normal",
			--		background = "Normal",
			--	},
			--},
		})
	end,
}
```