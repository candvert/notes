- [vim.diagnostic](#vim.diagnostic)
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