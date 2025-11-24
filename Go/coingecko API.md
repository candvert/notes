- [查询某个加密货币的价格](#查询某个加密货币的价格)
- [查询某个加密货币的价格、市值、交易量和市场相关数据](#查询某个加密货币的价格、市值、交易量和市场相关数据)
- [查询全局数据](#查询全局数据)
API网址： https://docs.coingecko.com/v3.0.1/reference/authentication
免费API的数据更新频率：每 60 秒一次
## 查询某个加密货币的价格
```go
package main

import (
	"fmt"
	"net/http"
	"log"
	"io"
	"net/url"
)

func main() {
	var base_url = "https://api.coingecko.com/api/v3/"
	var price_endpoint = "simple/price"
	var api_key = "CG-VQCFp5W7kK47MA2pi3xnSR6F"

	proxyUrl, err := url.Parse("http://127.0.0.1:7897") 
	if err != nil {
		log.Fatal("代理地址解析错误:", err)
	}
	// 创建自定义 Transport
	myTransport := &http.Transport{
		Proxy: http.ProxyURL(proxyUrl),
	}
	// 将 Transport 注入到 Client 中
	client := &http.Client{
		Transport: myTransport,
	}

	params := url.Values{}
	params.Add("ids", "bitcoin")       // 你想查的币 (ID 来自之前的列表)
	params.Add("vs_currencies", "usd") // 计价货币 (美元)
	// 拼接完整 URL
	fullUrl := base_url + price_endpoint + "?" + params.Encode()

	req, err := http.NewRequest("GET", fullUrl, nil)
	if err != nil {
		log.Fatal("创建请求失败:", err)
	}

	req.Header.Set("x-cg-demo-api-key", api_key)

	resp, err := client.Do(req)
	if err != nil {
		log.Fatal("创建请求失败2:", err)
	}
	defer resp.Body.Close()

	body, err := io.ReadAll(resp.Body)
	if err != nil {
		log.Fatal("读取响应失败:", err)
	}
	fmt.Println(string(body))
}
```
## 查询某个加密货币的价格、市值、交易量和市场相关数据
```go
var coin_market_endpoint = "coins/markets"
params.Add("vs_currency", "usd")
params.Add("ids", "bitcoin")

var result []struct {
	Current_price int
	Market_cap int64
}
if err := json.Unmarshal(body, &result); err != nil {
	log.Fatal("Unmarshal error")
}
fmt.Println(result[0].Current_price)
fmt.Println(result[0].Market_cap)
```
## 查询全局数据
```go
var global_endpoint = "global"

var result struct {
	Data struct {
		// 以各种货币计价的总市值
		Total_market_cap struct {
			Btc float64
			Eth float64
			Usd float64
		}
		Market_cap_percentage struct {
			Btc float64
			Eth float64
		}
		Market_cap_change_percentage_24h_usd float64
		Updated_at int64
	}
}
if err := json.Unmarshal(body, &result); err != nil {
	log.Fatal("Unmarshal error")
}
fmt.Println(result.Data.Total_market_cap.Usd)
fmt.Println(result.Data.Market_cap_change_percentage_24h_usd)
tm := time.Unix(result.Data.Updated_at, 0)
fmt.Println(tm.Format("2006-01-02 15:04:05"))
```