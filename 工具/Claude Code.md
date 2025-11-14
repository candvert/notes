```sh
npm install -g @anthropic-ai/claude-code
npm install -g @musistudio/claude-code-router
```
配置 `~/.claude-code-router/config.json` 文件
```json
{
  "Providers": [
    {
      "name": "deepseek",
      "api_base_url": "https://api.deepseek.com/chat/completions",
      "api_key": "sk-xxx",
      "models": ["deepseek-chat", "deepseek-reasoner"],
      "transformer": {
        "use": ["deepseek"],
        "deepseek-chat": {
          "use": ["tooluse"]
        }
      }
    }
  ],
  "Router": {
    "default": "deepseek,deepseek-chat",
    "think": "deepseek,deepseek-reasoner"
  }
}
```
每次修改完 config.json 后，都要运行
```sh
ccr restart
```
在命令行输入启动 claude code cli
```sh
ccr code
```
## 添加MCP
```sh
claude mcp add stripe --transport http https://mcp.stripe.com/
```