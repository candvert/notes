```go
package main

import (
	"context"
	"encoding/json"
	"fmt"
	"log"
	"net/http"
	aurl "net/url"
	"time"

	"github.com/gorilla/websocket"
)

// Binance WebSocket base URL
const wsBaseURL = "wss://stream.binance.com:9443/stream"

// -------------------------------------------------------------
// WebSocket 消息结构（以 Kline 为例）
// -------------------------------------------------------------
type KlineMessage struct {
	Stream string `json:"stream"`
	Data   struct {
		EventType string `json:"e"`
		EventTime int64  `json:"E"`
		Symbol    string `json:"s"`
		Kline     struct {
			StartTime    int64  `json:"t"`
			CloseTime    int64  `json:"T"`
			Symbol       string `json:"s"`
			Interval     string `json:"i"`
			FirstTradeID int64  `json:"f"`
			LastTradeID  int64  `json:"L"`
			Open         string `json:"o"`
			Close        string `json:"c"`
			High         string `json:"h"`
			Low          string `json:"l"`
			Volume       string `json:"v"`
			Trades       int64  `json:"n"`
			IsClosed     bool   `json:"x"`
		} `json:"k"`
	} `json:"data"`
}

// -------------------------------------------------------------
// 连接 WebSocket，支持自动重连
// -------------------------------------------------------------
func connect(ctx context.Context, streams []string) (*websocket.Conn, error) {
	// combined streams: /stream?streams=btcusdt@kline_1m/ethusdt@trade
	streamParam := ""
	for i, st := range streams {
		if i != 0 {
			streamParam += "/"
		}
		streamParam += st
	}
	url := fmt.Sprintf("%s?streams=%s", wsBaseURL, streamParam)

	proxyURL := "http://127.0.0.1:7897"
	dialer := websocket.Dialer{
		Proxy: func(_ *http.Request) (*aurl.URL, error) {
			return aurl.Parse(proxyURL)
		},
	}

	log.Printf("Connecting: %s", url)
	conn, _, err := dialer.Dial(url, nil)
	if err != nil {
		return nil, err
	}

	// 设置读超时（Binance 每20秒 ping 一次；1分钟无 ping/pong 就断）
	conn.SetReadDeadline(time.Now().Add(60 * time.Second))
	conn.SetPongHandler(func(appData string) error {
		conn.SetReadDeadline(time.Now().Add(60 * time.Second))
		return nil
	})

	return conn, nil
}

// -------------------------------------------------------------
// 自动重连的读取循环
// -------------------------------------------------------------
func readLoop(ctx context.Context, streams []string) {
	for {
		select {
		case <-ctx.Done():
			return
		default:
		}

		conn, err := connect(ctx, streams)
		if err != nil {
			log.Printf("Connect error: %v, retrying in 3s...", err)
			time.Sleep(3 * time.Second)
			continue
		}

		log.Println("Connected successfully")

		for {
			_, msg, err := conn.ReadMessage()
			if err != nil {
				log.Printf("Read error: %v, reconnecting...", err)
				conn.Close()
				break
			}

			// 解析 JSON
			var k KlineMessage
			if err := json.Unmarshal(msg, &k); err != nil {
				log.Printf("JSON Unmarshal error: %v", err)
				continue
			}

			// 示例打印 kline 的收盘价和是否收线
			if k.Data.EventType == "kline" {
				log.Printf("[%s] %s close=%s closed=%v",
					k.Data.Symbol,
					k.Data.Kline.Interval,
					k.Data.Kline.Close,
					k.Data.Kline.IsClosed,
				)
			}
		}
	}
}

// -------------------------------------------------------------
// 使用 SUBSCRIBE 动态订阅
// -------------------------------------------------------------
func subscribe(conn *websocket.Conn, params []string) error {
	req := map[string]interface{}{
		"method": "SUBSCRIBE",
		"params": params,
		"id":     time.Now().Unix(),
	}
	return conn.WriteJSON(req)
}

// 退订
func unsubscribe(conn *websocket.Conn, params []string) error {
	req := map[string]interface{}{
		"method": "UNSUBSCRIBE",
		"params": params,
		"id":     time.Now().Unix(),
	}
	return conn.WriteJSON(req)
}

// -------------------------------------------------------------
// 主程序
// -------------------------------------------------------------
func main() {
	ctx := context.Background()

	// 订阅多个流（小写）
	streams := []string{
		"btcusdt@kline_1m",
		"ethusdt@kline_1m",
	}

	readLoop(ctx, streams)
}
```