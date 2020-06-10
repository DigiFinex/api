# Websocket API

## URL

- `wss://openapi.digifinex.com/ws/v1/`

如果您在中国大陆地区使用，请用 `openapi.digifinex.vip` 代替 `openapi.digifinex.com`

## 压缩算法

请使用 `zlib deflate`

## 目录

public

- [server.ping](#Ping) 
- [server.time](#Time)
- [trades.subscribe](#成交订阅)
- [trades.update](#成交推送)
- [trades.unsubscribe](#取消成交订阅)
- [depth.subscribe](#深度订阅)
- [depth.update](#深度推送)
- [depth.unsubscribe](#取消深度订阅)
- [all_ticker.subscribe](#全部Ticker订阅)
- [all_ticker.update](#全部Ticker推送)
- [all_ticker.unsubscribe](#取消全部Ticker订阅)
- [ticker.subscribe](#Ticker订阅)
- [ticker.update](#Ticker推送)
- [ticker.unsubscribe](#取消Ticker订阅)

private

- [server.auth](#Auth)
- [order.subscribe](#Order订阅)
- [order.update](#Order推送)
- [order.unsubscribe](#Order取消订阅)
- [order_algo.subscribe](#OrderAlgo订阅)
- [order_algo.update](#OrderAlgo推送)
- [order_algo.unsubscribe](#OrderAlgo取消订阅)
- [balance.subscribe](#Balance订阅)
- [balance.update](#Balance推送)
- [balance.unsubscribe](#Balance取消订阅)

## 系统状态 API

提供系统状态检查接口，如 ping-pong 和服务器时间查询

## Ping

查看服务器连通性

### Request

- method

`server.ping`

### Response

- result

|  field | type  |  description |
| ------------ | ------------ | ------------ |
|  pong | String  |  pong, ack of ping |


### Python

```python
from websocket import create_connection
ws = create_connection("wss://openapi.digifinex.com/ws/v1/")
ws.send('{"id":12312, "method":"server.ping", "params":[]}')
print(ws.recv())
```

以上请求返回以下结构的 JSON 数据：

```json
{
  "error": null,
  "result": "pong",
  "id": 12312
}
```

## Time

获取服务器时间

### Request

- method

`server.time`

### Response

- result

|  field | type  |  description |
| ------------ | ------------ | ------------ |
|  timestamp | Integer  |  timestamp |

### Python

```python
from websocket import create_connection
ws = create_connection("wss://openapi.digifinex.com/ws/v1/")
ws.send('{"id":12312, "method":"server.time", "params":[]}')
print(ws.recv())
```

以上请求返回以下结构的 JSON 数据：

```json
{
   "error": null,
   "result": 1523348055,
   "id": 12312
}
```

## Auth

通过 apikey/secret 获取授权，授权成功才可以使用私有接口

### Request

- method

`server.auth`

- params = [apikey,timestamp,sign]
 - apikey: 申请的 apikey
 - timestamp: 毫秒时间戳
 - sign: CryptoJS.enc.Base64.Stringify(CryptoJS.HmacSHA256(timestamp, secret))

### Python

```python
from websocket import create_connection
ws = create_connection("wss://openapi.digifinex.com/ws/v1/")
ws.send('{"id":12312, "method":"server.auth", "params":["5a8e6b662ddb0","1583135616239","lXU0njGw4F+89g+EMxWtS9cLjKOqvUQ3+vsKK2yda40="]}')
print(ws.recv())
```

以上请求返回以下结构的 JSON 数据：

```json
{
    "error": null, 
    "result": 
    {
        "status": "success"
    },
    "id": 12312
}
```

## 交易 API

交易 channel 推送成交信息，包括成交价格、交易量、时间和类型等

## 成交订阅

订阅成交更新提醒

### Request

- method

`trades.subscribe`

- params

|  parameter | type  |  required | description  |
| ------------ | ------------ | ------------ | ------------ |
|  market list | list  |  Yes |  market list |

### Python

```python
from websocket import create_connection
ws = create_connection("wss://openapi.digifinex.com/ws/v1/")
ws.send('{"id":12312, "method":"trades.subscribe", "params":["ETH_USDT", "BTC_USDT"]}')
print(ws.recv())
```

以上请求返回以下结构的 JSON 数据：

```json
{
    "error": null, 
    "result": 
    {
        "status": "success"
    },
    "id": 12312
}
```

## 成交推送

推送最近成交更新

### Notify

- method

`trades.update`

- params


|  field | type  | description  |
| ------------ | ------------ | ------------ |
|  clean |  Boolean |  true: is complete result false: is last updated result |
|  trades list | List  |  list of trade info object, refer to query response |
| market  | String  | market name  |


以上请求返回以下结构的 JSON 数据：

```json
{
    "method": "trades.update",
    "params": 
        [
             true,
             [
                 {
                 "id": 7172173,
                 "time": 1523339279.761838,
                 "price": "398.59",
                 "amount": "0.027",
                 "type": "buy"
                 }
             ],
			 "ETH_USDT"
         ],
     "id": null
 }
```

## 取消成交订阅

取消成交更新推送

### Request

- method

`trades.unsubscribe`

### Python

```python
from websocket import create_connection
ws = create_connection("wss://openapi.digifinex.com/ws/v1/")
ws.send('{"id":12312, "method":"trades.unsubscribe", "params":[]}')
print(ws.recv())
```

以上请求返回以下结构的 JSON 数据：

```json
{
    "error": null, 
    "result": 
    {
        "status": "success"
    },
    "id": 12312
}
```

## 深度信息 API

深度信息 channel 提供订单薄深度信息

## 深度订阅

订阅深度

## Request

- method

`depth.subscribe`

- params

|  parameter | type  |  required | description  |
| ------------ | ------------ | ------------ | ------------ |
|  market list | list  |  Yes |  market list |

### Python

```python
from websocket import create_connection
ws = create_connection("wss://openapi.digifinex.com/ws/v1/")
ws.send('{"id":12312, "method":"depth.subscribe", "params":["ETH_USDT", "BTC_USDT"]}')
print(ws.recv())
```

以上请求返回以下结构的 JSON 数据：

```json
{
    "error": null, 
    "result": 
    {
        "status": "success"
    },
    "id": 12312
}
```

## 深度推送

推送深度更新信息

### 推送

- method

`depth.update`

- params

|  field | type  | description  |
| ------------ | ------------ | ------------ |
|  clean |  Boolean |  true: is complete result false: is last updated result |
|  depth |  JSON object |  depth json object, refer to query response |
| market  | String  | market name  |

以上请求返回以下结构的 JSON 数据：

```json
{
    "method": "depth.update", 
    "params": [
        true, 
        {
            "asks": [
                [                    
                    "8000.00",
                    "9.6250"
                ]
            ],
            "bids": [                
                [                    
                    "8000.00",
                    "9.6250"
                ]                
            ]
         }, 
         "EOS_USDT"
    ],
    "id": null
 }
```

## 取消深度订阅

取消订阅深度信息

### Request

- method

`depth.unsubscribe`

### Python

```python
from websocket import create_connection
ws = create_connection("wss://openapi.digifinex.com/ws/v1/")
ws.send('{"id":12312, "method":"depth.unsubscribe", "params":[]}')
print(ws.recv())
```

以上请求返回以下结构的 JSON 数据：

```json
{
    "error": null, 
    "result": 
    {
        "status": "success"
    },
    "id": 12312
}
```

## Ticker

获取平台全部币对的最新成交价、买一价、卖一价和24小时交易量的快照信息。

## 全部Ticker订阅

订阅全部全部Ticker

## Request

- method

`all_ticker.subscribe`

### Python

```python
from websocket import create_connection
ws = create_connection("wss://openapi.digifinex.com/ws/v1/")
ws.send('{"id":12312, "method":"all_ticker.subscribe", "params":[]}')
print(ws.recv())
```

以上请求返回以下结构的 JSON 数据：

```json
{
    "error": null, 
    "result": 
    {
        "status": "success"
    },
    "id": 12312
}
```

## 全部Ticker推送

推送全部Ticker更新信息

### 推送

- method

`all_ticker.update`

- params

以上请求返回以下结构的 JSON 数据：

```json
{
	"method": "all_ticker.update",
	"params": [{
		"symbol": "BTC_USDT",
		"open_24h": "1760",
		"low_24h": "1.00",
		"base_volume_24h": "9.40088557",
		"quote_volume_24h": "21786.30588557",
		"last": "4000",
		"last_qty": "1",
		"best_bid": "3397",
		"best_bid_size": "0.85",
		"best_ask": "4000",
		"best_ask_size": "109.2542",
		"timestamp": 1586762372864
	}],
	"id": null
}
```

## 取消全部Ticker订阅

取消订阅全部Ticker

### Request

- method

`all_ticker.unsubscribe`

### Python

```python
from websocket import create_connection
ws = create_connection("wss://openapi.digifinex.com/ws/v1/")
ws.send('{"id":12312, "method":"all_ticker.unsubscribe", "params":[]}')
print(ws.recv())
```

以上请求返回以下结构的 JSON 数据：

```json
{
    "error": null, 
    "result": 
    {
        "status": "success"
    },
    "id": 12312
}
```

## Ticker订阅

订阅单个Ticker信息

## Request

- method

`ticker.subscribe`

### Python

```python
from websocket import create_connection
ws = create_connection("wss://openapi.digifinex.com/ws/v1/")
ws.send('{"id":12312, "method":"ticker.subscribe", "params":["ETH_USDT", "BTC_USDT"]}')
print(ws.recv())
```

以上请求返回以下结构的 JSON 数据：

```json
{
    "error": null, 
    "result": 
    {
        "status": "success"
    },
    "id": 12312
}
```

## Ticker推送

推送单个Ticker更新信息

### 推送

- method

`ticker.update`

- params

以上请求返回以下结构的 JSON 数据：

```json
{
	"method": "ticker.update",
	"params": [{
		"symbol": "BTC_USDT",
		"open_24h": "1760",
		"low_24h": "1.00",
		"base_volume_24h": "11.40088557",
		"quote_volume_24h": "29786.30588557",
		"last": "4000",
		"last_qty": "1",
		"best_bid": "3375",
		"best_bid_size": "0.003",
		"best_ask": "4000",
		"best_ask_size": "108.2542",
		"timestamp": 1586762545336
	}],
	"id": null
}
```

## 取消Ticker订阅

取消订阅单个Ticker信息

### Request

- method

`ticker.unsubscribe`

### Python

```python
from websocket import create_connection
ws = create_connection("wss://openapi.digifinex.com/ws/v1/")
ws.send('{"id":12312, "method":"ticker.unsubscribe", "params":["ETH_USDT", "BTC_USDT"]}')
print(ws.recv())
```

以上请求返回以下结构的 JSON 数据：

```json
{
    "error": null, 
    "result": 
    {
        "status": "success"
    },
    "id": 12312
}
```



## Order订阅

订阅单个Order信息

## Request

- method

`order.subscribe`

### Python

```python
from websocket import create_connection
ws = create_connection("wss://openapi.digifinex.com/ws/v1/")
ws.send('{"id":12312, "method":"order.subscribe", "params":["ETH_USDT", "BTC_USDT"]}')
print(ws.recv())
```

以上请求返回以下结构的 JSON 数据：

```json
{
    "error": null, 
    "result": 
    {
        "status": "success"
    },
    "id": 12312
}
```

## Order推送

推送单个Order更新信息

### 推送

- method

`order.update`

- params

以上请求返回以下结构的 JSON 数据：

- mode: 1-spot, 2-margin
- side: buy, sell
- type: limit, market
- status: 0-未成交，1-部分成交，2-完全成交，3-已撤销未成交，4-已撤销部分成交

```json
{
	"method": "order.update",
	"params": [{
		"amount": "0",
		"filled": "0.9969",
		"id": "31b7f41c81787b4200c75eb074e6edc9",
		"mode": 1,
		"notional": "9713.82",
		"price": "0",
		"price_avg": "9700",
		"side": "buy",
		"status": 2,
		"symbol": "BTC_USDT",
		"timestamp": 1589272547000,
		"type": "market"
	}],
	"id": null
}
```

## Order取消订阅

取消订阅单个Order信息

### Request

- method

`order.unsubscribe`

### Python

```python
from websocket import create_connection
ws = create_connection("wss://openapi.digifinex.com/ws/v1/")
ws.send('{"id":12312, "method":"order.unsubscribe", "params":["ETH_USDT", "BTC_USDT"]}')
print(ws.recv())
```

以上请求返回以下结构的 JSON 数据：

```json
{
    "error": null, 
    "result": 
    {
        "status": "success"
    },
    "id": 12312
}
```



## OrderAlgo订阅

订阅单个计划委托信息

## Request

- method

`order_algo.subscribe`

### Python

```python
from websocket import create_connection
ws = create_connection("wss://openapi.digifinex.com/ws/v1/")
ws.send('{"id":12312, "method":"order_algo.subscribe", "params":["ETH_USDT", "BTC_USDT"]}')
print(ws.recv())
```

以上请求返回以下结构的 JSON 数据：

```json
{
    "error": null, 
    "result": 
    {
        "status": "success"
    },
    "id": 12312
}
```

## OrderAlgo推送

推送单个计划委托更新信息

### 推送

- method

`order_algo.update`

- params

以上请求返回以下结构的 JSON 数据：

- mode: 1-spot, 2-margin

```json
{
	"method": "order_algo.update",
	"params": [{
		"algo_price": "4000",
		"amount": "1",
		"id": "ff0738804cc3361fa91cb7c84feb113c",
		"mode": 1,
		"side": "buy",
		"status": 0,
		"symbol": "BTC_USDT",
		"timestamp": 1586767202000,
		"trigger_price": "4000"
	}],
	"id": null
}
```

## OrderAlgo取消订阅

取消订阅单个OrderAlgo信息

### Request

- method

`order.unsubscribe`

### Python

```python
from websocket import create_connection
ws = create_connection("wss://openapi.digifinex.com/ws/v1/")
ws.send('{"id":12312, "method":"order_algo.unsubscribe", "params":["ETH_USDT", "BTC_USDT"]}')
print(ws.recv())
```

以上请求返回以下结构的 JSON 数据：

```json
{
    "error": null, 
    "result": 
    {
        "status": "success"
    },
    "id": 12312
}
```



## Balance订阅

订阅单个Balance信息

## Request

- method

`balance.subscribe`

### Python

```python
from websocket import create_connection
ws = create_connection("wss://openapi.digifinex.com/ws/v1/")
ws.send('{"id":12312, "method":"balance.subscribe", "params":["ETH", "USDT"]}')
print(ws.recv())
```

以上请求返回以下结构的 JSON 数据：

```json
{
    "error": null, 
    "result": 
    {
        "status": "success"
    },
    "id": 12312
}
```

## Balance推送

推送单个Balance更新信息

### 推送

- method

`balance.update`

- params

以上请求返回以下结构的 JSON 数据：

```json
{
	"method": "balance.update",
	"params": [{
		"currency": "USDT",
		"free": "99944652.8478545303601106",
		"total": "99944652.8478545303601106",
		"used": "0.0000000000"
	}],
	"id": null
}
```

## Balance取消订阅

取消订阅单个Balance信息

### Request

- method

`balance.unsubscribe`

### Python

```python
from websocket import create_connection
ws = create_connection("wss://openapi.digifinex.com/ws/v1/")
ws.send('{"id":12312, "method":"balance.unsubscribe", "params":["ETH", "USDT"]}')
print(ws.recv())
```

以上请求返回以下结构的 JSON 数据：

```json
{
    "error": null, 
    "result": 
    {
        "status": "success"
    },
    "id": 12312
}
```
