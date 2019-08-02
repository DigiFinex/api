# Websocket API

### URL

- `wss://openapi.digifinex.com/ws/v1/`

If you are in mainland China, please use `openapi.digifinex.vip` replace `openapi.digifinex.com`

### Compress

Use `zlib deflate`

## System API

Provides system status check, such as ping-pong and server time query.

## Ping

Check server connectivity.

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

The above command returns JSON structured like this:

```json
{
  "error": null,
  "result": "pong",
  "id": 12312
}
```

## Time

Acquire server time.

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

The above command returns JSON structured like this:

```json
{
   "error": null,
   "result": 1523348055,
   "id": 12312
}
```

## Trade API

This channel sends a trade message whenever a trade occurs at DigiFinex. It includes details of the trade, such as price, amount, time and type.

## Trades subscription

Subscribe trades update notification.

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

The above command returns JSON structured like this:

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

## Trades notification

Notify latest trades update.

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

The above command returns JSON structured like this:

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

## Cancel subscription

Unsubscribe trades update notification.

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

The above command returns JSON structured like this:

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

## Depth API

The depth channel allow you to keep track of the state of the DigiFinex order book depth. It is provided on a price aggregated basis, with customizable precision.

## Depth subscription

Subscribe depth.

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

The above command returns JSON structured like this:

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

## Depth notification

Notify market depth update information

### Notify

- method

`depth.update`

- params

|  field | type  | description  |
| ------------ | ------------ | ------------ |
|  clean |  Boolean |  true: is complete result false: is last updated result |
|  depth |  JSON object |  depth json object, refer to query response |
| market  | String  | market name  |

### Python

The above command returns JSON structured like this:

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

## Cancel subscription

Unsbscribe all market depth.

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

The above command returns JSON structured like this:

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

