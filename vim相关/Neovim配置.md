- [配置文件](#配置文件)
- [配置文件相关命令](#配置文件相关命令)
- [安装插件管理器](#安装插件管理器)
- [自动补全插件](#自动补全插件)
- [LSP服务器端配置](#LSP服务器端配置)
- [LSP客户端配置](#LSP客户端配置)
- [options.lua文件](#options.lua文件)

- [安装NerdFont字体](#安装NerdFont字体)
- [语法高亮](#语法高亮)
- [查找文件](#查找文件)
- [文件explorer](#文件explorer)

- [插件](#插件)
	- [lualine.nvim](#lualine.nvim)
	- [bufferline.nvim](#bufferline.nvim)
	- [nvim-surround](#nvim-surround)
	- [mini.pairs](#mini.pairs)
	- [indent-blankline](#indent-blankline)
	- [leap.nvim](#leap.nvim)
	- [rainbow-delimiters.nvim](#rainbow-delimiters.nvim)
	- [none-ls](#none-ls)
	- [lspsaga.nvim](#lspsaga.nvim)
	- [toggleterm.nvim](#toggleterm.nvim)
	- [auto-save.nvim](#auto-save.nvim)
	- [trouble.nvim](#trouble.nvim)

- [c++的配置](#c++的配置)

配置完成之后每增加一门语言的支持就要进行下面三项。
一个是 LSP 服务器，提供代码提示等功能。通过 Mason 插件安装。命令为 :Mason。
一个是语法解析器，提供语法高亮等功能。通过修改 nvim-treesitter 配置文件安装。
该语言的编译器，要添加进 path 中。
## 配置文件
Linux上Neovim会默认读取~/.config/nvim/init.lua文件
Windows上会默认读取C:\Users\leiyu\AppData\Local\nvim\init.lua

可以将所有配置放在 init.lua 中，但这样不是一个好的做法，因此划分不同的文件和目录来放不同的配置
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
## 配置文件相关命令
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
    keymap = {
        preset = 'super-tab',
        ['<Up>'] = false,
        ['<Down>'] = false,
    },
    completion = { documentation = { auto_show = true } },
    sources = {
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
return { "mason-org/mason.nvim", opts = {}, }
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
对于 Unix 系统，使用 mason.nvim 则必须要已安装 git、curl 或 GNU wget、unzip、GNU tar（tar 或 gtar）、gzip
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


		-- 启用 rust_analyzer 的嵌入提示（即以灰色字体提示类型）
        vim.lsp.config['rust_analyzer'] = {
            on_attach = function(client, bufnr)
                vim.lsp.inlay_hint.enable(true, { bufnr = bufnr })
            end
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

安装完之后需要在 Windows Terminal 的设置中更换字体
![](/images/neovim_02.png)
## 语法高亮
首先需要安装 [nvim-treesitter](https://github.com/nvim-treesitter/nvim-treesitter)，主要功能是进行语法分析，生成语法树。之后便可以通过其他插件进行语法高亮。
使用 nvim-treesitter 需要有 gcc 编译器
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
		  ensure_installed = { "python", "lua", "go", "typescript", "css", "html", "javascript", "rust", "tsx" },
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
	-- 将背景设置为透明
	opts = {
		transparent = true,
		styles = {
			sidebars = "transparent",
			floats = "transparent",
		},
	},
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
## 查找文件
使用的是[telescope](https://github.com/nvim-telescope/telescope.nvim)
新增 plugins/telescope.lua 文件
```lua
return {
	'nvim-telescope/telescope.nvim', tag = '0.1.8',
	dependencies = { 
		'nvim-lua/plenary.nvim',
	},
	config = function()
		local builtin = require('telescope.builtin')
		require('telescope').setup{
			defaults = {
				borderchars = { "─", "│", "─", "│", "┌", "┐", "┘", "└" },
				results_title = false,
				dynamic_preview_title = true,
				mappings = {
					i = {
					
					}
				},
				
				layout_strategy = "vertical",
				layout_config = {
					vertical = {
						preview_height = 0.5,
						preview_cutoff = 20,
						prompt_position = "top",
					},
					-- 将屏幕撑满
					width = { padding = 0 },
					height = { padding = 0 },
				},
			},
		}
		vim.keymap.set('n', '<leader>ff', builtin.find_files, { desc = 'Telescope find files' })
		vim.keymap.set('n', '<leader>fg', builtin.live_grep, { desc = 'Telescope live grep' })
		vim.keymap.set('n', '<leader>fb', builtin.buffers, { desc = 'Telescope buffers' })
		vim.keymap.set('n', '<leader>fh', builtin.help_tags, { desc = 'Telescope help tags' })
		vim.keymap.set('n', '<space>en', function()
			require('telescope.builtin').find_files {
				cwd = vim.fn.stdpath('config')
			}
		end)
	end,
}
```
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
## 插件
## lualine.nvim
新增 plugins/lualine.lua 文件
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
新增 plugins/bufferline.lua 文件
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
新增 plugins/nvim_surround.lua 文件
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
新增 plugins/mini_pairs.lua 文件
```lua
return {
	'nvim-mini/mini.pairs',
	version = '*',
	opts = { },
	event = "InsertEnter",
}
```
## indent-blankline
新增 plugins/indent_blankline.lua 文件
```lua
return {
    "lukas-reineke/indent-blankline.nvim",
    main = "ibl",
    opts = {},
}
```
## leap.nvim
新增 plugins/leap.lua 文件
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
新增 plugins/rainbow.lua 文件
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
新增 plugins/none_ls.lua 文件
```lua
return {
	"nvimtools/none-ls.nvim",
	dependencies = { "nvim-lua/plenary.nvim" },
	event = "VeryLazy",
    keys = {
		-- 设置按键 <leader>lf 格式化代码
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
新增 plugins/lspsaga.lua 文件
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
新增 plugins/toggleterm.lua 文件
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
## auto-save.nvim
新增 plugins/auto_save.lua 文件
```lua
return {
  "okuuva/auto-save.nvim",
  version = '^1.0.0',
  cmd = "ASToggle",
  event = { "InsertLeave", "TextChanged" },
  opts = {
    debounce_delay = 500,
  },
}
```
## trouble.nvim
```lua
return {
  "folke/trouble.nvim",
  opts = {}, -- for default options, refer to the configuration section for custom setup.
  cmd = "Trouble",
  keys = {
    {
      "<leader>xx",
      "<cmd>Trouble diagnostics toggle<cr>",
      desc = "Diagnostics (Trouble)",
    },
    {
      "<leader>xX",
      "<cmd>Trouble diagnostics toggle filter.buf=0<cr>",
      desc = "Buffer Diagnostics (Trouble)",
    },
    {
      "<leader>cs",
      "<cmd>Trouble symbols toggle focus=false<cr>",
      desc = "Symbols (Trouble)",
    },
    {
      "<leader>cl",
      "<cmd>Trouble lsp toggle focus=false win.position=right<cr>",
      desc = "LSP Definitions / references / ... (Trouble)",
    },
    {
      "<leader>xL",
      "<cmd>Trouble loclist toggle<cr>",
      desc = "Location List (Trouble)",
    },
    {
      "<leader>xQ",
      "<cmd>Trouble qflist toggle<cr>",
      desc = "Quickfix List (Trouble)",
    },
  },
}
```
## c++的配置
只推荐在 wsl 和 linux 中使用，不推荐在 windows 上使用
lsp 服务器为 clangd
需要在项目根目录创建 .clangd 文件，用来指定 include 路径
```sh
CompileFlags:
  Add: [
    '-I/usr/include/opencv4',
    '-I/usr/include/opencv4/opencv'
  ]
```