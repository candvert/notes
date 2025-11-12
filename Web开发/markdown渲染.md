- [使用](#使用)
- [css样式](#css样式)
	- [GitHub样式](#GitHub样式)
	- [赛博朋克样式](#赛博朋克样式)
	- [简约博客样式](#简约博客样式)
	- [羊皮纸样式](#羊皮纸样式)
	- [普通样式](#普通样式)

使用 shiki 库和 markdown-it 库进行 markdown 的渲染
## 使用
markdown-it 库最简单使用：
```js
import markdownit from 'markdown-it';

const md = markdownit();
const result = md.render(`## 你好`);
console.log(result); // '<h2>你好</h2>'
```
使用 shiki 库高亮代码块：
```ts
import markdownit from 'markdown-it';
import { createHighlighter } from 'shiki';

const highlighter = await createHighlighter({
  themes: ["one-light"],
  // 所有可能高亮的语言必须在这里进行加载
  langs: ["javascript", "python"],
});

const md = markdownit({
  highlight: function (str: string, lang: string) {
    try {
	  // 如果没有指定语言，使用默认处理
	  if (!lang) {
	    return "";
	  }

	  // 使用 Shiki 进行语法高亮
	  const highlightedCode = highlighter.codeToHtml(str, {
	    lang: lang,
	    theme: "one-light",
	  });

	  return highlightedCode;
    } catch (error) {
	  // 如果高亮失败，返回原始代码
	  console.error("Shiki highlighting error:", error);
	  return "";
    }
  },
});

const result = md.render(`xxxxx`);
console.log(result);
```


markdownit 所有选项：
```js
// full options list (defaults)
const md = markdownit({
  // Enable HTML tags in source
  html:         false,

  // Use '/' to close single tags (<br />).
  // This is only for full CommonMark compatibility.
  xhtmlOut:     false,

  // Convert '\n' in paragraphs into <br>
  breaks:       false,

  // CSS language prefix for fenced blocks. Can be
  // useful for external highlighters.
  langPrefix:   'language-',

  // Autoconvert URL-like text to links
  linkify:      false,

  // Enable some language-neutral replacement + quotes beautification
  // For the full list of replacements, see https://github.com/markdown-it/markdown-it/blob/master/lib/rules_core/replacements.mjs
  typographer:  false,

  // Double + single quotes replacement pairs, when typographer enabled,
  // and smartquotes on. Could be either a String or an Array.
  //
  // For example, you can use '«»„“' for Russian, '„“‚‘' for German,
  // and ['«\xA0', '\xA0»', '‹\xA0', '\xA0›'] for French (including nbsp).
  quotes: '“”‘’',

  // Highlighter function. Should return escaped HTML,
  // or '' if the source string is not changed and should be escaped externally.
  // If result starts with <pre... internal wrapper is skipped.
  highlight: function (/*str, lang*/) { return ''; }
});
```
# css样式
## GitHub样式
一种非常干净、专业、易于阅读的样式，类似于 GitHub、Bootstrap 文档或许多现代技术博客的外观
```css
/*
 * Markdown 渲染样式 - "现代系统 / 文档"
 * * - 使用系统原生字体栈，性能最佳
 * - 干净的白色背景和深灰色 (#212529) 文本
 * - 标准的蓝色 (#0d6efd) 作为点缀色
 * - 风格简洁、专业，类似于文档网站
 */
.rendered-content {
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif, "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol";
  background: #ffffff;
  color: #212529; /* 几乎纯黑，但不是 100% 黑，更柔和 */
  line-height: 1.6;
}

.rendered-content h1 {
  font-size: 2.1rem;
  font-weight: 600;
  margin: 2rem 0 1rem 0;
  border-bottom: 1px solid #dee2e6; /* 浅灰色分隔线 */
  padding-bottom: 0.5rem;
}

.rendered-content h2 {
  font-size: 1.7rem;
  font-weight: 600;
  margin: 1.75rem 0 0.75rem 0;
  border-bottom: 1px solid #dee2e6; /* 浅灰色分隔线 */
  padding-bottom: 0.25rem;
}

.rendered-content h3 {
  font-size: 1.3rem;
  font-weight: 600;
  margin: 1.5rem 0 0.5rem 0;
}

.rendered-content p {
  margin: 1rem 0;
}

.rendered-content a {
  color: #0d6efd; /* 标准的“Bootstrap”蓝 */
  text-decoration: none;
  transition: text-decoration 0.2s ease;
}

.rendered-content a:hover {
  text-decoration: underline; /* 悬停时显示下划线 */
}

.rendered-content ul, .rendered-content ol {
  margin: 1rem 0;
  padding-left: 2rem;
}

.rendered-content li {
  margin: 0.5rem 0;
}

.rendered-content blockquote {
  border-left: 4px solid #adb5bd; /* 中性灰色边框 */
  margin: 1.5rem 0;
  padding: 1rem 1.5rem;
  background: #f8f9fa; /* 非常浅的灰色背景 */
  color: #6c757d; /* 灰色文本 */
  border-radius: 0 4px 4px 0;
}

.rendered-content table {
  width: 100%;
  border-collapse: collapse;
  margin: 1.5rem 0;
  border: 1px solid #dee2e6;
  border-radius: 4px;
  overflow: hidden; /* 保证圆角生效 */
}

.rendered-content th,
.rendered-content td {
  padding: 12px 15px;
  text-align: left;
  border: 1px solid #dee2e6;
}

.rendered-content th {
  background: #f8f9fa; /* 浅灰色表头 */
  font-weight: 600;
}

/* 斑马条纹 */
.rendered-content tr:nth-child(even) {
  background: #f8f9fa;
}

/* 行内代码 - GitHub 风格 */
.rendered-content code {
  background: rgba(175, 184, 193, 0.2); /* 浅灰色背景 */
  padding: 3px 6px;
  border-radius: 4px;
  font-family: "SFMono-Regular", Consolas, "Liberation Mono", Menlo, monospace;
  font-size: 0.9em;
  color: #d63384; /* 醒目的洋红色 */
}

/* 代码块 - 亮色 */
.rendered-content pre {
  background: #f8f9fa; /* 浅灰色背景 */
  border: 1px solid #dee2e6;
  padding: 1rem;
  border-radius: 4px;
  overflow-x: auto;
  margin: 1rem 0;
  font-family: "SFMono-Regular", Consolas, "Liberation Mono", Menlo, monospace;
}

.rendered-content pre code {
  background: none;
  padding: 0;
  border: none;
  color: inherit; /* 继承 <pre> 的颜色 */
  font-size: 1em; /* 恢复正常大小 */
}
```
## 赛博朋克样式
"赛博朋克 / 霓虹" 风格
```css
/*
 * Markdown 渲染样式 - "赛博朋克 / 霓虹" (Cyberpunk / Neon)
 * * - 极深的蓝紫色背景
 * - 亮粉色、电蓝色、霓虹黄作为点缀
 * - 标题使用等宽字体，营造“终端”感
 * - 元素带有“发光”效果
 */
.rendered-content {
  /* 深邃的“午夜”蓝紫背景 */
  background: #0d0221; 
  color: #e0e0ff; /* 基础文字为淡紫色，而不是纯白，更柔和 */
  font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
  line-height: 1.7;
  padding: 1rem; /* 为整个容器添加一点内边距，使其与背景区分 */
}

.rendered-content h1 {
  font-family: 'Consolas', 'Menlo', 'monospace';
  font-size: 2.5rem;
  margin: 2rem 0 1rem 0;
  color: #f92672; /* 亮粉色 (Hot Pink) */
  /* 关键：使用 text-shadow 制造霓虹灯发光效果 */
  text-shadow: 0 0 5px #f92672, 0 0 10px #f92672, 0 0 15px rgba(249, 38, 114, 0.5);
  border-bottom: 2px solid #f92672;
  padding-bottom: 0.5rem;
}

.rendered-content h2 {
  font-family: 'Consolas', 'Menlo', 'monospace';
  font-size: 1.8rem;
  margin: 1.75rem 0 0.75rem 0;
  color: #00e5ff; /* 电蓝色 (Electric Blue) */
  text-shadow: 0 0 3px #00e5ff, 0 0 5px rgba(0, 229, 255, 0.5);
  border-bottom: 1px solid #00e5ff;
}

.rendered-content h3 {
  font-family: 'Consolas', 'Menlo', 'monospace';
  font-size: 1.4rem;
  margin: 1.5rem 0 0.5rem 0;
  color: #e6db74; /* 霓虹黄 (Neon Yellow) */
}

.rendered-content p {
  margin: 1.1rem 0;
  font-size: 1rem;
}

.rendered-content a {
  color: #00e5ff; /* 链接使用电蓝色 */
  text-decoration: none;
  transition: all 0.2s ease;
}
.rendered-content a:hover {
  color: #fff;
  text-shadow: 0 0 5px #00e5ff; /* 悬停时发光 */
}

.rendered-content ul, .rendered-content ol {
  margin: 1.1rem 0;
  padding-left: 2.5rem;
}

.rendered-content li::marker {
  color: #f92672; /* 列表标记使用亮粉色 */
}

.rendered-content blockquote {
  border-left: 5px solid #00e5ff; /* 电蓝色边框 */
  margin: 1.5rem 0;
  padding: 1rem 1.5rem;
  background: rgba(0, 229, 255, 0.05); /* 非常微弱的“全息”背景 */
  color: #f0f0ff;
  font-style: italic;
  border-radius: 0 4px 4px 0;
}

.rendered-content table {
  width: 100%;
  border-collapse: collapse; /* 使用 collapse 来实现网格线效果 */
  margin: 1.5rem 0;
  background: transparent;
}

.rendered-content th,
.rendered-content td {
  padding: 12px 15px;
  text-align: left;
  /* 关键：发光的“网格”边框 */
  border: 1px solid #f92672; 
  box-shadow: 0 0 5px rgba(249, 38, 114, 0.3) inset; /* 单元格内发光 */
}

.rendered-content th {
  background: rgba(249, 38, 114, 0.15); /* 粉色表头背景 */
  color: #f92672; /* 粉色文字 */
  font-family: 'Consolas', 'Menlo', 'monospace';
  font-weight: 600;
}

.rendered-content tr:hover {
  background: rgba(255, 255, 255, 0.05); /* 悬停时高亮 */
}

/* 行内代码 - 霓虹绿，像旧式终端 */
.rendered-content code {
  background: #1e1e1e;
  border: 1px solid #333;
  padding: 3px 6px;
  border-radius: 4px;
  font-family: 'Fira Code', 'Consolas', monospace;
  font-size: 0.9em;
  color: #a6e22e; /* 霓虹绿 */
}

/* 代码块 - 发光边框 */
.rendered-content pre {
  background: #100330; /* 比主背景稍亮的深紫色 */
  padding: 1rem;
  border-radius: 8px;
  overflow-x: auto;
  margin: 1rem 0;
  /* 关键：发光边框 */
  border: 1px solid #00e5ff;
  box-shadow: 0 0 15px rgba(0, 229, 255, 0.5);
  transition: all 0.3s ease;
}
.rendered-content pre:hover {
  box-shadow: 0 0 25px rgba(0, 229, 255, 0.7); /* 悬停时更亮 */
}

.rendered-content pre code {
  background: none;
  padding: 0;
  border: none;
  color: inherit; /* 继承 pre 的颜色 */
  font-size: 1em;
}
```
## 简约博客样式
偏向于“学术”或“简约博客”的外观
```css
/*
 * Markdown 渲染样式 - "现代学术风"
 * * - 标题使用衬线字体 (Georgia, Times)
 * - 主体使用无衬线字体 (Lato, System)
 * - 点缀色为沉稳的青色 (#00796b)
 * - 代码块使用亮色主题
 */
.rendered-content {
  /* 为所有内容设置一个可读性好的字体和颜色 */
  font-family: 'Lato', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
  color: #333333;
  line-height: 1.65;
}

.rendered-content h1 {
  font-family: 'Georgia', 'Times New Roman', serif;
  font-size: 2.25rem;
  font-weight: 600;
  margin: 2rem 0 1rem 0;
  color: #2a2a2a;
  border-bottom: 2px solid #e0e0e0;
  padding-bottom: 0.5rem;
}

.rendered-content h2 {
  font-family: 'Georgia', 'Times New Roman', serif;
  font-size: 1.75rem;
  font-weight: 600;
  margin: 1.75rem 0 0.75rem 0;
  color: #2a2a2a;
  border-bottom: 1px solid #e0e0e0;
}

.rendered-content h3 {
  font-family: 'Georgia', 'Times New Roman', serif;
  font-size: 1.35rem;
  font-weight: 600;
  margin: 1.5rem 0 0.5rem 0;
  color: #333;
}

.rendered-content p {
  margin: 1.1rem 0;
  font-size: 1rem;
}

.rendered-content ul, .rendered-content ol {
  margin: 1.1rem 0;
  padding-left: 2.5rem;
}

.rendered-content li {
  margin: 0.5rem 0;
}

.rendered-content li::marker {
  color: #00796b; /* 列表标记的点缀色 */
}

.rendered-content blockquote {
  border-left: 5px solid #00796b; /* 青色边框 */
  margin: 1.5rem 0;
  padding: 1rem 1.5rem;
  background: #f8fcfb; /* 非常浅的青色背景 */
  color: #444;
  font-style: italic; /* 引用块使用斜体 */
  border-radius: 0 4px 4px 0;
}

.rendered-content table {
  width: 100%;
  border-collapse: collapse;
  margin: 1.5rem 0;
  box-shadow: 0 0 8px rgba(0,0,0,0.06);
  border: 1px solid #e0e0e0;
  border-radius: 8px;
  overflow: hidden; /* 保证圆角生效 */
}

.rendered-content th,
.rendered-content td {
  padding: 12px 15px;
  text-align: left;
  border-bottom: 1px solid #e0e0e0;
}

.rendered-content th {
  background: #f1f8f7; /* 浅青色表头 */
  color: #005f5f; /* 深青色文字 */
  font-weight: 600;
  font-family: 'Lato', sans-serif; /* 表头使用无衬线 */
}

/* 斑马条纹 */
.rendered-content tr:nth-of-type(even) {
  background: #fdfdfd;
}

.rendered-content tr:hover {
  background: #f8fcfb; /* 悬停时高亮 */
}

/* 行内代码 - 亮色 */
.rendered-content code {
  background: #fdfbf6; /* 暖色象牙白 */
  border: 1px solid #eee;
  padding: 3px 5px;
  border-radius: 4px;
  font-family: 'Fira Code', 'Consolas', 'Menlo', monospace;
  font-size: 0.9em;
  color: #c7254e; /* 用红色系来突出代码 */
}

/* 代码块 - 亮色主题 (例如 GitHub Gist) */
.rendered-content pre {
  background: #f6f8fa; /* 经典的亮色代码背景 */
  border: 1px solid #e0e0e0;
  padding: 1rem;
  border-radius: 8px;
  overflow-x: auto;
  margin: 1rem 0;
  font-family: 'Fira Code', 'Consolas', 'Menlo', monospace;
}

/* 重置 pre 里的 code 样式 */
.rendered-content pre code {
  background: none;
  padding: 0;
  border: none;
  color: inherit; /* 继承 <pre> 的颜色 */
  font-size: 1em; /* 恢复正常大小 */
}
```
## 羊皮纸样式
温暖、复古、适合阅读的“羊皮纸”或“旧书”风格
```css
/*
 * Markdown 渲染样式 - "温暖读者 / 旧书"
 * * - 背景使用暖色调 (象牙白 / 羊皮纸)
 * - 文本使用深棕色，而非纯黑
 * - 主体使用易读的衬线字体 (Merriweather, Georgia)
 * - 标题使用无衬线字体 (Lato) 以产生对比
 * - 点缀色为沉稳的赤褐色/棕红色 (#8c4333)
 */
.rendered-content {
  background: #fdfaf5; /* 温暖的象牙白背景 */
  color: #3a312a; /* 深棕色文字 */
  font-family: 'Merriweather', 'Georgia', serif; /* 易读的衬线字体 */
  line-height: 1.75; /* 更大的行高，提升阅读舒适度 */
}

.rendered-content h1,
.rendered-content h2,
.rendered-content h3 {
  /* 标题使用无衬线字体，与正文形成对比 */
  font-family: 'Lato', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
  color: #2a211b; /* 稍深一点的棕色 */
  font-weight: 700;
}

.rendered-content h1 {
  font-size: 2.2rem;
  margin: 2rem 0 1rem 0;
  border-bottom: 2px solid #dcd1c9; /* 柔和的暖灰色下划线 */
  padding-bottom: 0.5rem;
}

.rendered-content h2 {
  font-size: 1.6rem;
  margin: 1.75rem 0 0.75rem 0;
  border-bottom: 1px solid #dcd1c9;
}

.rendered-content h3 {
  font-size: 1.3rem;
  margin: 1.5rem 0 0.5rem 0;
}

.rendered-content p {
  margin: 1.2rem 0;
}

.rendered-content a {
  color: #8c4333; /* 赤褐色/棕红色链接 */
  text-decoration: none;
  border-bottom: 1px dotted #8c4333;
  transition: all 0.2s ease;
}

.rendered-content a:hover {
  color: #5a2b20;
  border-bottom: 1px solid #5a2b20;
}

.rendered-content ul, .rendered-content ol {
  margin: 1.2rem 0;
  padding-left: 2.5rem;
}

.rendered-content li::marker {
  color: #8c4333; /* 列表标记使用点缀色 */
}

.rendered-content blockquote {
  border-left: 4px solid #8c4333; /* 点缀色边框 */
  margin: 1.5rem 0;
  padding: 1rem 1.5rem;
  background: #fbf6f0; /* 稍深的背景色 */
  color: #5a4b41; /* 稍浅的文本色 */
  font-style: italic; /* 传统引用样式 */
  border-radius: 0 4px 4px 0;
}

.rendered-content table {
  width: 100%;
  border-collapse: collapse;
  margin: 1.5rem 0;
  border: 1px solid #dcd1c9; /* 柔和的暖灰色边框 */
  border-radius: 4px;
  overflow: hidden;
}

.rendered-content th,
.rendered-content td {
  padding: 10px 14px;
  text-align: left;
  border: 1px solid #dcd1c9;
}

.rendered-content th {
  background: #fbf6f0; /* 表头背景 */
  color: #8c4333; /* 表头文字使用点缀色 */
  font-family: 'Lato', sans-serif; /* 匹配标题字体 */
  font-weight: 600;
}

.rendered-content tr:nth-of-type(even) {
  background: #fdfaf5; /* 保持基础背景色 */
}
.rendered-content tr:nth-of-type(odd) {
  background: #fbf6f0; /* 斑马条纹 */
}

/* 行内代码 - 配合暖色调 */
.rendered-content code {
  background: #f4eee6; /* 暖色代码背景 */
  border: 1px solid #dcd1c9;
  padding: 2px 5px;
  border-radius: 4px;
  font-family: 'Fira Code', 'Menlo', monospace;
  font-size: 0.9em;
  color: #7a3a2a; /* muted red-brown color */
}

/* 代码块 - 亮色 Sepia 主题 */
.rendered-content pre {
  background: #f4eee6; /* 匹配行内代码的背景 */
  border: 1px solid #dcd1c9;
  padding: 1rem;
  border-radius: 4px;
  overflow-x: auto;
  margin: 1rem 0;
  color: #5a4b41; /* 代码块基础文字颜色 */
  font-family: 'Fira Code', 'Menlo', monospace;
}

.rendered-content pre code {
  background: none;
  padding: 0;
  border: none;
  color: inherit; /* 继承 pre 的颜色 */
}
```
## 普通样式
```css
/* Markdown 渲染样式 */
.rendered-content h1 {
    font-size: 2rem;
    margin: 1.5rem 0 1rem 0;
    color: #2d3748;
    border-bottom: 2px solid #e2e8f0;
    padding-bottom: 0.5rem;
}

.rendered-content h2 {
    font-size: 1.5rem;
    margin: 1.25rem 0 0.75rem 0;
    color: #2d3748;
}

.rendered-content h3 {
    font-size: 1.25rem;
    margin: 1rem 0 0.5rem 0;
    color: #2d3748;
}

.rendered-content p {
    margin: 1rem 0;
    line-height: 1.7;
}

.rendered-content ul, .rendered-content ol {
    margin: 1rem 0;
    padding-left: 2rem;
}

.rendered-content li {
    margin: 0.5rem 0;
}

.rendered-content blockquote {
    border-left: 4px solid #667eea;
    padding-left: 1rem;
    margin: 1rem 0;
    color: #4a5568;
    background: #f7fafc;
    padding: 1rem;
    border-radius: 0 8px 8px 0;
}

.rendered-content table {
    width: 100%;
    border-collapse: collapse;
    margin: 1rem 0;
    background: white;
    border-radius: 8px;
    overflow: hidden;
    box-shadow: 0 1px 3px rgba(0,0,0,0.1);
}

.rendered-content th,
.rendered-content td {
    padding: 12px;
    text-align: left;
    border-bottom: 1px solid #e2e8f0;
}

.rendered-content th {
    background: #667eea;
    color: white;
    font-weight: 600;
}

.rendered-content tr:hover {
    background: #f7fafc;
}

.rendered-content code {
    background: #edf2f7;
    padding: 2px 6px;
    border-radius: 4px;
    font-family: 'Monaco', 'Menlo', 'Ubuntu Mono', monospace;
    font-size: 0.9em;
}

.rendered-content pre {
    background: #1a1b26;
    color: white;
    padding: 1rem;
    border-radius: 8px;
    overflow-x: auto;
    margin: 1rem 0;
    border: 1px solid #2d3748;
}

.rendered-content pre code {
    background: none;
    padding: 0;
    border-radius: 0;
}
```