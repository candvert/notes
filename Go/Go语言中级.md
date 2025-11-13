- [添加包](#添加包)
- [go mod tidy](#go%20mod%20tidy)

- [使用http包创建本地服务器](#使用http包创建本地服务器)
- [自动打开浏览器并跳转](#自动打开浏览器并跳转)
- [打包为exe](#打包为exe)

- [标准库](#标准库)
	- [io](#io)
	- [net](#net)
		- [net/http](#net/http)
	- [time](#time)
	- [bufio](#bufio)
	- [math](#math)

- [openai库](#openai库)

var buf bytes.Buffer
buf.WriteByte('[')
buf.WriteString(", ")
buf.WriteRune('世')
## 添加包
```go
// 默认安装到 $GOPATH/pkg/mod 目录下，一般为 C:\Users\leiyu\go\pkg\mod
// go env GOPATH 命令打印 GOPATH 变量的值
go get github.com/gin-gonic/gin
go get go.uber.org/zap
```
## go mod tidy
```go
// go mod tidy 会递归分析项目中所有 .go 源文件（包括测试文件）的 import 语句，然后自动下载添加缺失的模块和移除未被使用的模块
// -v 选项输出具体添加或移除了哪些模块
```
## 使用http包创建本地服务器
```go
import "net/http"

// 注册路由
// 第二个参数为函数
http.HandleFunc("/", helloHandler)
http.HandleFunc("/api/info", infoHandler)

// 监听端口
port := ":8880"
err := http.ListenAndServe(port, nil)
if err != nil {
	fmt.Printf("Server error: %v\n", err)
}
fmt.Printf("Server starting on http://localhost%s\n", port)

// 处理函数
func helloHandler(w http.ResponseWriter, r *http.Request) {
	if r.Method != "GET" {
        http.Error(w, "Method not allowed", http.StatusMethodNotAllowed)
        return
    }

	// 发送文本数据
	w.Header().Set("Content-Type", "text/plain; charset=utf-8")
	w.Write([]byte("This is another line of text\n"))
	// 发送 json 数据
	// w.Header().Set("Content-Type", "application/json")
    // fmt.Fprintf(w, `{"status": "ok", "message": "Server is running"}`)
}
```
静态文件目录服务：
```go
// 创建文件服务器实例
// http.FileServer 遵循常见的 Web 服务器约定：当请求的路径对应一个目录时，它会自动查找并返回该目录下的 index.html 文件作为默认页面
// http.FileServer 会隐藏以点（.）开头的文件和目录
fs := http.FileServer(http.Dir("./static"))
// 访问路由 / 时，显示 ./static/index.html 文件
http.Handle("/", fs)
log.Fatal(http.ListenAndServe(":8880", nil))
```
流式传输
```go
w.Header().Set("Content-Type", "text/event-stream")
w.Header().Set("Cache-Control", "no-cache")
w.Header().Set("Connection", "keep-alive")

// 获取Flusher支持流式输出
flusher, ok := w.(http.Flusher)
if !ok {
	http.Error(w, "Streaming unsupported", http.StatusInternalServerError)
	return
}
for {
	fmt.Fprintf(w, "data: {\"person\": \"John\"}\n\n")
	flusher.Flush() // 立即发送数据到客户端
	fmt.Fprintf(w, "data: {\"person\": \"Amily\"}\n\n")
	flusher.Flush()
}
```

```go
http.HandleFunc("/welcome", func(w http.ResponseWriter, r *http.Request) {
	// 从 URL 参数获取数据
	name := r.URL.Query().Get("name")
	if name == "" {
		name = "Guest"
	}
})
```
## 自动打开浏览器并跳转
```go
import (
	"os/exec"
	"runtime"
	"log"
)

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
## 打包为exe
单个文件打包为 exe（在 Windows PowerShell 中）
```go
// 先设置 GOOS 和 GOARCH 环境变量
$env:GOOS = "windows"
$env:GOARCH = "amd64"
// 再运行该命令，-ldflags="-H windowsgui" 的作用是隐藏默认打开的终端
go build -ldflags="-H windowsgui" -o myapp.exe
```
多个文件打包成 exe，比如该目录
```go
chat/
├── main.go
└── static/
		└── index.html
		├── style.css
		└── script.js
```
需要使用 embed 标准库将静态文件嵌入到二进制中，在 main.go 中需要有这些代码：
```go
// //go:embed static/* 嵌入 static/ 目录下的所有文件
// http.FS(staticFiles)：将嵌入的文件系统转换为 http.FileServer 可用的格式
package main
import (
	"io/fs"
    "embed"
    "log"
    "net/http"
)

//go:embed static/*
var staticFiles embed.FS
func main() {
	// 使用 fs.Sub 将文件系统的根目录定位到 static 目录
	// 这样访问路由 / 显示的就是 index.html 页面
	staticFS, err := fs.Sub(staticFiles, "static")
	if err != nil {
        log.Fatal("无法加载静态文件:", err)
    }
	
    http.Handle("/", http.FileServer(http.FS(staticFS)))
    if err := http.ListenAndServe(":8080", nil); err != nil {
        log.Fatal("服务器启动失败:", err)
    }
}
```
打包为 exe（在 Windows PowerShell 中）
```go
// 先设置 GOOS 和 GOARCH 环境变量
$env:GOOS = "windows"
$env:GOARCH = "amd64"
// 再运行该命令，-ldflags="-H windowsgui" 的作用是隐藏默认打开的终端
go build -ldflags="-H windowsgui" -o myapp.exe
```
## go list
```go
// 可以使用 go list 命令列出一个包目录下的哪些文件是源文件，哪些是测试文件，哪些是外部测试文件

// 列出 fmt 包目录下的源文件
go list -f={{.GoFiles}} fmt

// 列出 fmt 包目录下的测试文件
go list -f={{.TestGoFiles}} fmt

// 列出 fmt 包目录下的外部测试文件
 go list -f={{.XTestGoFiles}} fmt
```

## string操作
```go
import "strings"

// 将字符串切片连接为一个字符串，第二个参数为分隔符
func Join(elems []string, sep string) string


// 如果字符串 s 不包含分隔符 sep，则返回的切片长度为 1，只包含元素 s
// 如果 sep 为空，则 Split 会在每个 UTF-8 序列后进行分割
// 如果 s 和 sep 都为空，则 Split 返回一个空切片
func Split(s, sep string) []string
```
## 字符串和字节切片之间类型转换
```go
// 将字符串 "hello" 转换为字节切片
r := []byte("hello")


// 将字节切片转换为字符串
b := string(r)
```
## 文件操作
```go
import "os"

// 读取文件内容
file, err := os.Open("file.go")
data := make([]byte, 100)
count, err := file.Read(data)
fmt.Printf("read %d bytes: %q\n", count, data[:count])


// 如果文件不存在，WriteFile 函数会创建该文件
// 如果文件存在，WriteFile 函数会在写入前 truncate 文件
func WriteFile(name string, data []byte, perm FileMode) error



func ReadFile(name string) ([]byte, error)


// 程序立即终止，defer 的函数不会运行
// 为了便于移植，状态码应在 [0, 125] 范围内
func Exit(code int)


// 删除指定的文件或空目录
func Remove(name string) error
```
## log日志
```go
file, err := os.Open("file.go")
if err != nil {
	log.Fatal(err)
}
```
## 随机数
```go
// 如果直接使用 rand.Intn(5)，则每次生成的序列相同
// 所以如果想每次生成的序列不同，必须提供随机数生成器 rand.NewSource()

import "math/rand"

seed := time.Now().UTC().UnixNano()
rng := rand.New(rand.NewSource(seed))
i := rng.Intn(5)


// 返回一个介于 [0,n) 之间的伪随机数
// 如果 n <= 0，则函数会引发 panic
func Intn(n int) int
```
## io
```go
// io 包保证任何由文件结束引起的读取失败都返回同一个错误 io.EOF
```
## net
```go
import "net"

listener, err := net.Listen("tcp", "localhost:8000")
for {
	conn, err := listener.Accept()
	io.WriteString(conn, time.Now().Format("15:04:05\n"))
}


conn, err := net.Dial("tcp", "localhost:8000")
```
## net/http
```go
import "net/http"

// 当 err 为 nil 时，resp 始终包含一个非 nil 的 resp.Body。调用者在读取完 resp.Body 后应将其关闭
func Get(url string) (resp *Response, err error)


resp, err := http.Get("http://baidu.com/")
b := make([]byte, 9999)
resp.Body.Read(b)
fmt.Println(string(b))
```
发送 POST 请求
```go
import (
	"context"
	"time"
	"net/http"
	"strings"
)

client := &http.Client{
	Timeout: 60 * time.Second,
}

reqBody := `{"user": "John"}`
// context.Background() 表示空的上下文，没有超时、取消等限制
req, err := http.NewRequestWithContext(context.Background(), "POST", "https://api.deepseek.com/chat/completions", strings.NewReader(reqBody))
// 设置请求头
req.Header.Set("Content-Type", "application/json")
// 发送请求
resp, err := client.Do(req)
if err != nil {
	log.Fatal(err)
}
defer resp.Body.Close()

// 错误处理
if resp.StatusCode != http.StatusOK {
	// TODO
}
```
接收客户端发送的 POST 请求
```go
http.HandleFunc("/api/chat", chatHandler)
http.ListenAndServe(":8880", nil)

type ChatRequest struct {
	UserPrompt string `json:"userPrompt"`
}

func chatHandler(w http.ResponseWriter, r *http.Request) {
	var reqData ChatRequest
	if err := json.NewDecoder(r.Body).Decode(&reqData); err != nil {
		http.Error(w, "Bad request", http.StatusBadRequest)
		return
	}
	defer r.Body.Close()
}
```
## time
```go
import "time"


// 如果参数 d 为负数或零，则立即返回
func Sleep(d Duration)


time.Sleep(time.Second)
```
## bufio
bufio 包实现了带缓冲的 I/O 操作，可以提高读写效率
对于大量小 I/O 操作，bufio 能显著提升性能
读取：
```go
import (
    "bufio"
    "fmt"
    "strings"
)

// 创建字符串读取器
str := "Hello World\nGolang bufio package\nThird line"
reader := bufio.NewReader(strings.NewReader(str))

// 逐行读取
for {
	line, err := reader.ReadString('\n')
	if err != nil {
		break
	}
	fmt.Printf("读取到: %s", line)
}

// 重置读取器
reader.Reset(strings.NewReader("New content"))
data, _ := reader.ReadString(' ')
fmt.Printf("重置后读取: %s\n", data)
```
写入：
```go
// 创建文件并写入
file, err := os.Create("test.txt")
if err != nil {
	panic(err)
}
defer file.Close()

writer := bufio.NewWriter(file)
// 批量写入
lines := []string{"第一行\n", "第二行\n", "第三行\n"}
for _, line := range lines {
	writer.WriteString(line)
}

// 必须调用 Flush 确保数据写入
writer.Flush()
fmt.Println("文件写入完成")
```
## math
```go
math.MaxFloat32
math.MaxFloat64
```
## openai库
```go
import (
	"github.com/openai/openai-go/v3" // imported as openai
)

func 
client := openai.NewClient(
	option.WithAPIKey("My API Key"),
)
chatCompletion, err := client.Chat.Completions.New(context.TODO(), openai.ChatCompletionNewParams{
	Messages: []openai.ChatCompletionMessageParamUnion{
		openai.UserMessage("Say this is a test"),
	},
	Model: openai.ChatModelGPT4o,
})
if err != nil {
	panic(err.Error())
}
println(chatCompletion.Choices[0].Message.Content)
```