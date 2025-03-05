- [uWebSockets](#uWebSockets)
- [cpp-httplib](#cpp-httplib)
- [nlohmann-json](#nlohmann-json)

## uWebSockets
```
Github仓库：https://github.com/uNetworking/uWebSockets
简介：使用C++17编写的跨平台库，可用于HTTP/HTTPS和WebSocket

LIBUS_NO_SSL       禁用OpenSSL依赖
UWS_NO_ZLIB        禁用Zlib依赖

uWS::App().get("/hello", [](auto *res, auto *req) {
    res->end("Hello World!");
});
```
## 示例
```cpp
/* One app per thread; spawn as many as you have CPU-cores and let uWS share the listening port */
uWS::SSLApp({

    /* These are the most common options, fullchain and key. See uSockets for more options. */
    .cert_file_name = "cert.pem",
    .key_file_name = "key.pem"
    
}).get("/hello/:name", [](auto *res, auto *req) {

    /* You can efficiently stream huge files too */
    res->writeStatus("200 OK")
       ->writeHeader("Content-Type", "text/html; charset=utf-8")
       ->write("<h1>Hello ")
       ->write(req->getParameter("name"))
       ->end("!</h1>");
    
}).ws<UserData>("/*", {

    /* Just a few of the available handlers */
    .open = [](auto *ws) {
        ws->subscribe("oh_interesting_subject");
    },
    .message = [](auto *ws, std::string_view message, uWS::OpCode opCode) {
        ws->send(message, opCode);
    }
    
}).listen(9001, [](auto *listenSocket) {

    if (listenSocket) {
        std::cout << "Listening on port " << 9001 << std::endl;
    } else {
        std::cout << "Failed to load certs or to bind to port" << std::endl;
    }
    
}).run();
```
## cpp-httplib
```
Github仓库：https://github.com/yhirose/cpp-httplib
简介：使用C++11编写的跨平台HTTP/HTTPS服务器/客户端库，使用阻塞套接字I/O，只需一个头文件
```
## 示例
```cpp
// 如果要进行https连接，需要有#define CPPHTTPLIB_OPENSSL_SUPPORT并链接OpenSSL库

#include "httplib.h"
#include <iostream>

int main() {
	httplib::Client cli("http://baidu.com");

	auto res = cli.Get("/");
	std::cout << res->status << std::endl;
	std::cout << res->body << std::endl;
}
```
## nlohmann-json
```cpp
// 包含头文件和使用类型别名
#include <nlohmann/json.hpp>
using json = nlohmann::json;


// 从文件中读取json
std::ifstream f("example.json");
json data = json::parse(f);


// 创建json对象的几种方法
// 第一种
json j;
j["pi"] = 3.141;
j["happy"] = true;
j["name"] = "Niels";
j["nothing"] = nullptr;
j["answer"]["everything"] = 42;
j["list"] = { 1, 0, 2 };
j["object"] = { {"currency", "USD"}, {"value", 42.99} };
// 或者写成这样
json j2 = {
  {"pi", 3.141},
  {"happy", true},
  {"name", "Niels"},
  {"nothing", nullptr},
  {"answer", {
    {"everything", 42}
  }},
  {"list", {1, 0, 2}},
  {"object", {
    {"currency", "USD"},
    {"value", 42.99}
  }}
};
// 使用原始字符串字面值和json::parse()函数
json ex1 = json::parse(R"(
  {
    "pi": 3.141,
    "happy": true
  }
)");
// 使用用户自定义字符串字面值
using namespace nlohmann::literals;
json ex2 = R"(
  {
    "pi": 3.141,
    "happy": true
  }
)"_json;
// 使用初始化列表
json ex3 = {
  {"happy", true},
  {"pi", 3.141},
};


// 序列化
std::string s = j.dump();    // {"happy":true,"pi":3.141}

// pretty printing
std::cout << j.dump(4) << std::endl;
// {
//     "happy": true,
//     "pi": 3.141
// }
```