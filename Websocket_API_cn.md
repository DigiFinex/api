# Websocket API

### URL

- `wss://openapi.digifinex.com/ws/v1/`

如果您在中国大陆地区使用，请用 `openapi.digifinex.vip` 代替 `openapi.digifinex.com`

### 压缩算法

请使用 `zlib deflate`

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

### Python

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

### Python

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

