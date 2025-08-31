我的配置：
```lua
-- airline完全使用vimscript编写，没有任何额外依赖
-- 帮助文档:h airline
-- 状态栏可以分为多个section，每个section可以分为多个part
-- Airline 的状态栏从左到右分为 a, b, c (左半部分) 和 x, y, z (右半部分)
-- :AirlineToggle 切换标准状态栏和airline的状态栏
return { "vim-airline/vim-airline", 
	opts = {},
	config = function()
		-- 启用airline的标签栏
		vim.g['airline#extensions#tabline#enabled'] = 1
		-- 是否显示右侧的"buffers"
		vim.g['airline#extensions#tabline#show_buffers'] = 0
		-- 是否显示左侧的"tab"
		vim.g['airline#extensions#tabline#show_tabs'] = 0
		-- 是否显示左边的"tab"和右边的"tab"数量
		vim.g['airline#extensions#tabline#show_tab_type'] = 0
		vim.g['airline#extensions#tabline#show_tab_count'] = 0

		-- 设置各个section的显示内容
		vim.g['airline_section_c'] = vim.fn['airline#section#create']({'readonly'})
		vim.g['airline_section_x'] = ''
		vim.g['airline_section_y'] = ''
		vim.g['airline_section_z'] = vim.fn['airline#section#create']({'%p%% ', '%l:%c'})
		vim.g['airline_section_error'] = ''
		vim.g['airline_section_warning'] = ''
		-- 是否显示字符计数
		vim.g['airline#extensions#wordcount#enabled'] = 0

		--是否显示git分支
		vim.g['airline#extensions#branch#enabled'] = 1

		-- 设置不同模式（如NORMAL，INSERT）显示的文本，即section_a显示的内容：
		vim.g['airline_mode_map'] = {
			__ = '-',
			c = 'C',
			i = 'I',
			ic = 'I',
			ix = 'I',
			n = 'N',
			multi = 'M',
			ni = 'N',
			no = 'N',
			R = 'R',
			Rv = 'R',
			s = 'S',
			S = 'S',
			['\19'] = 'S',
			t = 'T',
			v = 'V',
			V = 'V',
			['\22'] = 'V',
		}
	end,
	dependencies = {
		-- 要显示git分支需要该插件
		'tpope/vim-fugitive',
	},
}
```