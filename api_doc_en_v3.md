### Get All Trading Pairs

> Return all supported trading pairs by default

`If you are in mainland China, please use openapi.digifinex.vip replace openapi.digifinex.com`

* URL：`https://openapi.digifinex.com/v3/markets`
* Request Method: GET
* Request Parameters: 

|Param Name			|Type		|Mandatory		|Description|
| :-----   	| :-----   	| :-----  | :-----   |
|apikey		|string		|1			|Specified the apikey generated in usercenter|

* Example：

```
# Request
GET https://openapi.digifinex.com/v3/markets?apikey=5aefffcc11212

# Response
{
	"code": 0,
	"date": 1557224303,
	"data": [
		{
			"market": "btc_usdt",
			"volume_precision": 4,
			"price_precision": 2,
			"min_volume": 0.0001
			"min_amount": 2,
		},
		......
		{
			"market": "doge_eth",
			"volume_precision": 2,
			"price_precision": 8,
			"min_volume": 100
			"min_amount": 0.005,
		}
	]
}
```

* Return Values：

```
code: Error code
date: Current timestamp returned by server
data: Trading pairs
	market：Name of trading pair
	volume_precision：Volume precision
	price_precision：Price precision
	min_volume：Minimum trading volume
	min_amount：Minimum trading amount
```

### Get Orderbook

> Return 10 depth by default, ranked by price descending

* URL：`https://openapi.digifinex.com/v3/order_book`
* Request Method: GET
* Request Parameters: 

|Param Name			|Type		|Mandatory		|Description|
| :-----   	| :-----   	| :-----  | :-----   |
|apikey		|string		|1			|Specified the apikey generated in usercenter|
|market		|string		|1			|Specified trading pair, e.g. btc_usdt, all pairs can be obtained through markets|
|limit		|int		|0			|Depth limit, 10 by default, maximum 150|


* Example：

```
# Request
GET https://openapi.digifinex.com/v3/order_book?apikey=5aefffcc11212&market=btc_usdt&limit=10

# Response
{
	"code": 0,
	"date": 1557223838,
	"bids": [
		[5956.66, 0.3227],
		[5956.62, 0.3169],
		...
	],
	"asks": [
		[5963.57, 0.5264952],
		[5963.42, 0.099],
		...
	]
}
```

* Return Values：

```
code: Error code
date: Current timestamp returned by server
data: Orderbook data
	bids：bids，[price，amount]
	asks: asks，[price，amount]
```

### Get All Trades

> Return 100 trades by default

* URL：`https://openapi.digifinex.com/v3/trades`
* Request Method: GET
* Request Parameters: 

|Param Name			|Type		|Mandatory		|Description|
| :-----   	| :-----   	| :-----  | :-----   |
|apikey		|string		|1			|Specified the apikey generated in usercenter|
|market		|string		|1			|Specified trading pair, e.g. btc_usdt, all pairs can be obtained through markets|
|limit		|int		|0			|Request limit, 100 by default, maximum 500|


* Example：

```
# Request
GET https://openapi.digifinex.com/v3/trades?apikey=5aefffcc11212&market=btc_usdt&limit=10

# Response
{
	"code": 0,
	"date": 1557224784,
	"data": [
		{
			"id": 1000,
			"amount": 0.0061,
			"type": "buy",
			"date": 1557224784,
			"price": 5944.12
		},
		{
			"id": 999,
			"amount": 0.0038,
			"type": "buy",
			"date": 1557224779,
			"price": 5944.03
		},
		...
	]
}
```

* Return Values：

```
code: Error code
date: Current timestamp returned by server
data: All trades data
	id: Trades record ID
	date: Trading time
	price: Trading price
	amount: Trading amount
	type: buy/sell
```
