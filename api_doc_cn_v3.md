### 错误代码	详细描述
```
0	    成功

10001   请求method错误
10002	ApiKey错误
10003	签名错误
10004	参数错误
10005	频率限制
10006	无权限
10007	访问者IP不在白名单内
10008	时间戳不在1分钟内
10009   没有该方法------------------
10011   该apikey已经过期，不能使用，为了安全请创建新的api

20002	该币种对暂时不允许交易
20007	价格精度限制
20008	数量精度限制
20009	最小交易数量限制
20010	最小交易额限制
20011	账户余额不足
20012	无效的交易类型（buy/sell)
20013	没有找到该订单
20014	日期格式不对，例：2018-07-25
20015	查询的订单日期超过限制
20018   您的交易权限已被系统限制
20019   交易对错误，注意交易币在前，基础币在后
20020   违反了操作交易规则，暂时禁止交易。目前对成交率、成交权重、撤单率等有限制
20021   币种错误
20022   结束时间应该大于或等于开始时间

50000   系统错误（新增）
```


### 获取交易对

> 默认返回支持的所有交易对

If you are in mainland China, please use`openapi.digifinex.vip` `replace openapi.digifinex.com`

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

### 获取交易对K线信息

> 默认返回100条数据

* URL：`https://openapi.digifinex.com/v3/kline?symbol=BTC_USDT&period=1&start_time=1559354460&end_time=1559365260`
* 请求方法: GET
* 请求参数: 
* 请求说明：价格以CNY计价

|参数名			|参数类型		|必填		|描述|
| :-----   	| :-----   	| :-----  | :-----   |
|symbol		|string		|1			|交易对，交易币在前，基础币在后|
|period		|string		|1			|K线类型，1,5,15,30,60,240,720,1D,1W|
|start_time		|int		|0			|K线开始时间，有效时间戳。缺省，则返回end_time之前的200根k线。|
|end_time		|int		|0			|K线结束时间，有效时间戳，⼤于start_time。缺省为当前时间戳。|


* 示例：

```
# Request
GET https://openapi.digifinex.com/v3/kline?symbol=BTC_USDT&period=1&start_time=1559354460&end_time=1559365260

# Response
{
  "code": 0,
  "data": [
    [
      1559354760,
      0,
      13827.224895,
      13827.224895,
      13827.224895,
      13827.224895
    ],
    [
      1559354820,
      0,
      13827.224895,
      13827.224895,
      13827.224895,
      13827.224895
    ],
    [
      1559354940,
      0,
      13827.224895,
      13827.224895,
      13827.224895,
      13827.224895
    ],
    [
      1559355360,
      0,
      13827.224895,
      13827.224895,
      13827.224895,
      13827.224895
    ],
    [
      1559355660,
      0,
      13827.224895,
      13827.224895,
      13827.224895,
      13827.224895
    ],
    [
      1559355720,
      0,
      13827.224895,
      13827.224895,
      13827.224895,
      13827.224895
    ],
    [
      1559355840,
      0,
      13827.224895,
      13827.224895,
      13827.224895,
      13827.224895
    ],
    [
      1559357940,
      0,
      13827.224895,
      13827.224895,
      13827.224895,
      13827.224895
    ],
    [
      1559362140,
      0,
      13827.224895,
      13827.224895,
      13827.224895,
      13827.224895
    ],
    [
      1559363220,
      0,
      13827.224895,
      13827.224895,
      13827.224895,
      13827.224895
    ]
  ]
}
```

* 返回值说明：

```
code: 错误码
data: K线列表
	[0] 时间戳
	[1] 交易量
	[2] 收盘价
	[3] 最高价
	[4] 最低价
	[5] 开盘价
```