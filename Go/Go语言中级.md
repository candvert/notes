- [标准库](#标准库)
	- [io](#io)
	- [net](#net)

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

source := rand.NewSource(time.Now().UnixNano())
r := rand.New(source)
i := r.Intn(5)


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
## time
```go
import "time"


// 如果参数 d 为负数或零，则立即返回
func Sleep(d Duration)


time.Sleep(time.Second)
```
## math
```go
math.MaxFloat32
math.MaxFloat64
```