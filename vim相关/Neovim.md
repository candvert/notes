
- [vim.diagnostic](#vim.diagnostic)
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

```sh
:e $MYVIMRC 打开init.lua配置文件
:term 打开终端窗口
```
## vim.diagnostic
```lua
-- 诊断信息来自外部工具，比如 LSP 服务器，linters

-- 默认的按键映射
]d 跳转到下一个诊断信息
[d 跳转到上一个诊断信息
]D 跳转到最后一个诊断信息
[D 跳转到第一个诊断信息
<C-w>d 在一个悬浮窗显示光标处的诊断信息

-- 创建命名空间
vim.api.nvim_create_namespace("my_namespace")

-- 设置快捷键显示/隐藏virtual_lines
vim.keymap.set('n', 'gK', function()
	local new_config = not vim.diagnostic.config().virtual_lines
	vim.diagnostic.config({ virtual_lines = new_config })
end, { desc = 'Toggle diagnostic virtual_lines' })


vim.diagnostic.config({
	-- Nivm默认提供这些handler："virtual_text", "virtual_lines", "signs", "underline"
	-- 显示诊断信息
    virtual_text = true,
	-- 编辑过程中实时显示诊断信息
    update_in_insert = true,
	-- 有警告，错误的地方不使用下划线
	-- underline = false,

	signs = {
		-- 警告、错误时最左侧显示的字符，默认为W、E
		--[[
		text = {
			[vim.diagnostic.severity.ERROR] = '',
			[vim.diagnostic.severity.WARN] = '',
		},
		--]]
		-- 有错误高亮整行内容
		linehl = {
			[vim.diagnostic.severity.ERROR] = 'ErrorMsg',
		},
		-- 有警告高亮行号
		numhl = {
			[vim.diagnostic.severity.WARN] = 'WarningMsg',
		},
	},
})
```
## 设置按键映射
```lua
-- mode参数为哪种模式，n为普通模式，i为插入模式，v为视图模式，c为命令行模式
-- lhs为要更改的按键
-- rhs为更改后的按键或者一个lua函数
-- opts为一些选项
vim.keymap.set({mode}, {lhs}, {rhs}, {opts})

-- 普通模式和编辑模式下按Ctrl + a后b就运行:lua print('hello world')
vim.keymap.set({ "n", "i" }, "<C-a>b", "<Cmd>lua print('hello world')<CR>", { silent = true, desc = "print" })

-- 将:help命令设置为快捷键good
vim.keymap.set('n', 'good', ':help<CR>')


-- 禁用某个快捷键
vim.keymap.del({modes}, {key})

vim.keymap.del('n', 'Q')
```