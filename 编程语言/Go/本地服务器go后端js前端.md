网址为：http://127.0.0.1:8880/
实现的功能：访问网址后，js 代码向路由 /api/chat 发送 GET 请求，go 代码收到请求后发送流式信息，js 代码收到信息后将其打印到控制台

目录结构：
```go
chat/
├── main.go
└── static/
		└── index.html
		├── style.css
		└── script.js
```
main.go 文件：
```go
package main

import (
	"fmt"
	"log"
	"net/http"
    "runtime"
    "os/exec"
)

func main() {
	fs := http.FileServer(http.Dir("./static"))
	http.Handle("/", fs)
	http.HandleFunc("/api/chat", chatHandler)
	go openBrowser()
	log.Fatal(http.ListenAndServe(":8880", nil))
}

func chatHandler(w http.ResponseWriter, r *http.Request) {
	w.Header().Set("Content-Type", "text/event-stream")
	w.Header().Set("Cache-Control", "no-cache")
	w.Header().Set("Connection", "keep-alive")

	flusher, ok := w.(http.Flusher)
	if !ok {
		http.Error(w, "Streaming unsupported", http.StatusInternalServerError)
		return
	}

	fmt.Fprintf(w, "data: {\"person\": \"John\"}\n\n")
	flusher.Flush()
	fmt.Fprintf(w, "data: {\"person\": \"Amily\"}\n\n")
	flusher.Flush()
}

// 自动跳转到浏览器网址
func openBrowser() {
    url := "http://127.0.0.1:8880/" // 替换为你的目标网址
    
    var cmd *exec.Cmd
    
    switch runtime.GOOS {
    case "windows":
        cmd = exec.Command("cmd", "/c", "start", url)
    case "darwin": // macOS
        cmd = exec.Command("open", url)
    case "linux":
        cmd = exec.Command("xdg-open", url)
    default:
        log.Fatalf("Unsupported platform: %s", runtime.GOOS)
    }
    
    err := cmd.Start()
    if err != nil {
        log.Fatal(err)
    }
}
```
script.js 文件：
```js
const response = await fetch("/api/chat");
if (!response.ok) {
  throw new Error(`HTTP error! status: ${response.status}`);
}

const reader = response.body.getReader();
const decoder = new TextDecoder();

while (true) {
  const { done, value } = await reader.read();
  if (done) break;

  const chunk = decoder.decode(value, { stream: true });
  const lines = chunk.split("\n\n");
  for (const line of lines) {
    if (line.startsWith("data: ")) {
      try {
        const data = JSON.parse(line.slice(6));
        console.log(data.person);
      } catch (e) {
        console.error("解析JSON失败:", e);
      }
    }
  }
}
```
