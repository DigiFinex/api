### 获取交易对

> 默认返回支持的所有交易对

* URL：`https://openapi.digifinex.com/v3/markets`
* 请求方法: GET
* 请求参数: 

|参数名			|参数类型		|必填		|描述|
| :-----   	| :-----   	| :-----  | :-----   |
|apikey		|string		|1			|指定 apikey, 可在用户中心生成|

* 示例：

```
# Request
GET https://openapi.digifinex.com/v3/markets

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

* 返回值说明：

```
code: 错误码
date: 服务器数据返回的当前时间戳
data:交易对数据
	market：交易对名称
	volume_precision：数量精度
	price_precision：价格精度
	min_volume：最小交易数量
	min_amount：最小交易额
```

### 获取买卖盘

> 默认返回10条数据，买盘和卖盘都是从高到底

* URL：`https://openapi.digifinex.com/v3/order_book`
* 请求方法: GET
* 请求参数: 

|参数名			|参数类型		|必填		|描述|
| :-----   	| :-----   	| :-----  | :-----   |
|apikey		|string		|1			|指定 apikey, 可在用户中心生成|
|market		|string		|1			|要查询的交易对，例如：btc_usdt，可以通过markets获得|
|limit		|int		|0			|数量，没有该字段返回10条，最长150条|


* 示例：

```
# Request
GET https://openapi.digifinex.com/v3/order_book?market=btc_usdt&limit=30

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

* 返回值说明：

```
code: 错误码
date: 服务器数据返回的当前时间戳
data: 买卖盘数据
	bids：买盘，[价格，数量]
	asks: 卖盘，[价格，数量]
```

### 获取最近成交记录

> 默认返回100条数据

* URL：`https://openapi.digifinex.com/v3/trades`
* 请求方法: GET
* 请求参数: 

|参数名			|参数类型		|必填		|描述|
| :-----   	| :-----   	| :-----  | :-----   |
|apikey		|string		|1			|指定 apikey, 可在用户中心生成|
|market		|string		|1			|要查询的交易对，例如：btc_usdt，可以通过markets获得|
|limit		|int		|0			|数量，没有该字段默认返回100条，最长500条|


* 示例：

```
# Request
GET https://openapi.digifinex.com/v3/trades?market=btc_usdt&limit=30

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

* 返回值说明：

```
code: 错误码
date: 服务器数据返回的当前时间戳
data: 最近成交数据
	id: 记录ID
	date: 成交时间
	price: 交易价格
	amount: 交易数量
	type: buy/sell
```
