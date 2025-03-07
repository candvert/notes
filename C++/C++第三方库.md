- [cpp-httplib](#cpp-httplib)
- [nlohmann-json](#nlohmann-json)
- [libuv](#libuv)
- [libhv](#libhv)

## cpp-httplib
```
Github仓库：https://github.com/yhirose/cpp-httplib
简介：使用C++11编写的跨平台HTTP/HTTPS服务器/客户端库，使用阻塞套接字I/O，只需一个头文件。但是如果使用https，则还需要openssl库
```
## 示例
```cpp
// 如果要进行https连接，需要有#define CPPHTTPLIB_OPENSSL_SUPPORT并链接OpenSSL库

//客户端（访问http网站）
#include <httplib.h>
#include <iostream>
int main() {
	httplib::Client cli("http://127.0.0.1:8000");
	auto res = cli.Get("/");
	std::cout << res->status << std::endl;
	std::cout << res->body << std::endl;
}


//客户端（访问https网站）
#define CPPHTTPLIB_OPENSSL_SUPPORT
#include <httplib.h>
#include <iostream>
int main() {
    httplib::Client client("https://baidu.com");
    auto res = client.Get("/");
    std::cout << res->status << std::endl;
    std::cout << res->body << std::endl;
}


// 服务端
#include <httplib.h>
#include <nlohmann/json.hpp>
using json = nlohmann::json;
int main() {
    httplib::Server svr;
    svr.Get("/", [](const httplib::Request& req, httplib::Response& res) {
        json response;
        response["today"] = "Sunday";
        res.set_content(response.dump(), "application/json");
        });
    svr.listen("127.0.0.1", 8000);
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
## libuv
```

```
## libhv
```
libhv提供了非阻塞IO
```
