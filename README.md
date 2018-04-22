# Server-Sent Event [![GoDoc](https://godoc.org/github.com/go-mego/sse?status.svg)](https://godoc.org/github.com/go-mego/sse)

Server-Sent Event 是單向事件串流，這會讓伺服器不斷地發送帶有指定格式的事件至客戶端中。這和 Stream 套件類似，但不同的是 Server-Sent Event 是固定格式的資料並且以字串發送。

# 索引

* [安裝方式](#安裝方式)
* [使用方式](#使用方式)
	* [發布事件](#發布事件)

# 安裝方式

打開終端機並且透過 `go get` 安裝此套件即可。

```bash
$ go get github.com/go-mego/sse
```

# 使用方式

將 `sse.New` 傳入 Mego 引擎中的 `Use` 即能將 Server-Sent Event 中介軟體作為全域中介軟體在所有路由中使用。

```go
package main

import (
	"github.com/go-mego/mego"
	"github.com/go-mego/sse"
)

func main() {
	m := mego.New()
	// 套用 Server-Sent Event 作為全域中介軟體在所有路由中使用。
	m.Use(sse.New())
	m.Run()
}
```

Server-Sent Event 中介軟體也能夠僅用於單個路由中，這讓你可以替不同路由設置不同的 Server-Sent Event 配置。

```go
func main() {
	m := mego.New()
	// 將 Server-Sent Event 中介軟體獨立用於此路由。
	m.GET("/", sse.New(), func(s *sse.Stream) {
		// ...
	})
	m.Run()
}
```

## 發布事件

透過 `Publish` 並傳入一個 `&sse.Event` 來將事件發佈至客戶端。

```go
func main() {
	m := mego.New()
	m.GET("/", sse.New(), func(s *sse.Stream) {
		// 透過 `Publish` 來將一個 `&sse.Event` 事件發布到客戶端中。
		s.Publish(&sse.Event{
			// `Data` 是事件的主要資料內容。
			Data: `{"result": 3}`,
		})
	})
	m.Run()
}
```