## 安装
```sh
npm init playwright
```

```python
from playwright.sync_api import sync_playwright

with sync_playwright() as p:
    # 启动浏览器（headless=False表示有界面模式，便于调试）
    browser = p.chromium.launch(headless=False)
    # 创建浏览器上下文（可实现多页面隔离，如独立Cookie）
    context = browser.new_context()
    # 创建新页面
    page = context.new_page()
    # 导航到目标网址
    page.goto("https://example.com")
    # ... 后续操作
    # 关闭浏览器
    browser.close()
```