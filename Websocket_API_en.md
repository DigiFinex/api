# Websocket API

## URL

- `wss://openapi.digifinex.com/ws/v1/`

If youa are a user of Chinese mainland，please use `openapi.digifinex.vip` instead of `openapi.digifinex.com`

## Compression algorithm

please use `zlib deflate`

## Catalogue

public

- [server.ping](#Ping)-
- [server.time](#Time)
- [trades.subscribe](#Trades-subscription)
- [trades.update](#Trades-push)
- [trades.unsubscribe](#Cancel-trades-subscription)
- [depth.subscribe](#Depth-subscription)
- [depth.update](#Depth-push)
- [depth.unsubscribe](#Cancel-depth-subscription)
- [all_ticker.subscribe](#All-Ticker-subscriptions)
- [all_ticker.update](#All-Ticker-push)
- [all_ticker.unsubscribe](#Cancel-all-Ticker-subscriptions)
- [ticker.subscribe](#Ticker-subscription)
- [ticker.update](#Ticker-push)
- [ticker.unsubscribe](#Cancel-Ticker-subscription)

private

- [server.auth](#Auth)
- [order.subscribe](#Order-subscription)
- [order.update](#Order-push)
- [order.unsubscribe](#Cancel-order-subscription)
- [order_algo.subscribe](#OrderAlgo-subscription)
- [order_algo.update](#OrderAlgo-push)
- [order_algo.unsubscribe](#Cancel-OrderAlgo-subscription)
- [balance.subscribe](#Balance-subscription)
- [balance.update](#Balance-push)
- [balance.unsubscribe](#Cancel-balance-subscription)

## System state API

Interfaces are provided to check system status, such as ping-pong and server time queries

## Ping

### View server connectivity

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

The above request returns JSON data for the following structure：

```json
{
  "error": null,
  "result": "pong",
  "id": 12312
}
```

## Time

Get server time

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

The above request returns JSON data for the following structure：

```json
{
   "error": null,
   "result": 1523348055,
   "id": 12312
}
```

## Auth

Authorization is obtained through apikey/secret and the private interface can only be used if the authorization succeeds

### Request

- method

`server.auth`

- params = [apikey,timestamp,sign]
 - apikey: applied apikey
 - timestamp: millisecond timestamp
 - sign: CryptoJS.enc.Base64.Stringify(CryptoJS.HmacSHA256(timestamp, secret))

### Python

```python
from websocket import create_connection
ws = create_connection("wss://openapi.digifinex.com/ws/v1/")
ws.send('{"id":12312, "method":"server.auth", "params":["5a8e6b662ddb0","1583135616239","lXU0njGw4F+89g+EMxWtS9cLjKOqvUQ3+vsKK2yda40="]}')
print(ws.recv())
```

The above request returns JSON data for the following structure：

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

## Transaction API

Transaction channel pushes transaction information, including transaction price, transaction volume, time and type, etc

## Trades subscription

Subscribe to transaction update

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

The above request returns JSON data for the following structure：

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

## Trades push

push updates of recent trades

### Notify

- method

`trades.update`

- params


|  field | type  | description  |
| ------------ | ------------ | ------------ |
|  clean |  Boolean |  true: is complete result false: is last updated result |
|  trades list | List  |  list of trade info object, refer to query response |
| market  | String  | market name  |


The above request returns JSON data for the following structure：

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

## Cancel trades subscription

cancel pushes of trade update

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

The above request returns JSON data for the following structure：

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

## Depth information API

The depth Information channel reveals order book depth 

## Depth subscription

depth subscription

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

The above request returns JSON data for the following structure：

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

## Depth push

push book depth updates

### Push

- method

`depth.update`

- params

|  field | type  | description  |
| ------------ | ------------ | ------------ |
|  clean |  Boolean |  true: is complete result false: is last updated result |
|  depth |  JSON object |  depth json object, refer to query response |
| market  | String  | market name  |

The above request returns JSON data for the following structure：

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

## Cancel depth subscription

cancel depth subscription

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

The above request returns JSON data for the following structure：

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

Get the latest transaction price, bid price, ask price and 24-hour trading volume of all coin pairs on the platform.

## All Ticker subscriptions

subscribe all Ticker

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

The above request returns JSON data for the following structure：

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

## All Ticker push

push all Ticker updates

### Push

- method

`all_ticker.update`

- params

The above request returns JSON data for the following structure：

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

## Cancel all Ticker subscriptions 

cancel all Ticker subscriptions 

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

The above request returns JSON data for the following structure：

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

## Ticker subscription

subscribe single Ticker update

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

The above request returns JSON data for the following structure：

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

## Ticker push

push single Ticker update

### push

- method

`ticker.update`

- params

The above request returns JSON data for the following structure：

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

## Cancel Ticker subscription

cancel subscription for single Ticker update

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

The above request returns JSON data for the following structure：

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



## Order subscription

Subscribe for single order information

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

The above request returns JSON data for the following structure：

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

## Order push

push single order update

### Push

- method

`order.update`

- params

The above request returns JSON data for the following structure：

- mode: 1-spot, 2-margin
- side: buy, sell
- type: limit, market
- status: 0-unfilled，1-partially filled，2-fully filled，3-canceled unfilled，4-canceled partially filled

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

## Cancel order subscription

cancel single order subscription

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

The above request returns JSON data for the following structure：

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



## OrderAlgo subscription

subscribe for single OrderAlgo informaion

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

The above request returns JSON data for the following structure：

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

## OrderAlgo push

push single OrderAlgo update

### push

- method

`order_algo.update`

- params

The above request returns JSON data for the following structure：

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

## Cancel OrderAlgo subscription

cancel single OrderAlgo subscription

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

The above request returns JSON data for the following structure：

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



## Balance subscription

Subscribe single balance information 

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

The above request returns JSON data for the following structure：

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

## Balance push

push single balance update

### push

- method

`balance.update`

- params

The above request returns JSON data for the following structure：

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

## Cancel balance subscription

cancel single balance subscription

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

The above request returns JSON data for the following structure：

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
