- [vim.diagnostic](#vim.diagnostic)
```
:e $MYVIMRC 打开init.lua配置文件
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