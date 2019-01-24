# DigiFinex API Documentation
> Version：1.1.9, Update: 2019-01-23, ©️DigiFinex
> 
> Api接口调用频率上限为：GET接口180次/min，POST接口60次/min，超过上限后将暂停调用5分钟
> 
> Post请求采用：content-type: application/x-www-form-urlencoded
> 
> 更多问题可加入电报群交流：https://t.me/digifinex_api

## 开通API
前往DigiFinex官网（www.digifinex.com），注册账号，登录后进入“个人中心”-“API管理”页面，获取ApiKey和ApiSecret。


## 错误码

调用每一个接口的返回值中都会包含code参数。code取0表示接口调用成功，可正常返回结果数据。若code取值非0，表示接口调用出错，则不会返回结果数据，请参照下表查看错误原因。

| 错误代码   | 详细描述  |
| :-----    | :-----   |
|0|成功|
|10001|请求方法错误，注意是Get还是Post方法|
|10002|ApiKey错误|
|10003|签名错误|
|10004|参数错误|
|10005|频率限制|
|10006|无权限|
|10007|访问者IP不在白名单内|
|10008|时间戳不在1分钟内|
|10009|接口endpoint不存在，请注意检查endpoint URL|
|20001|交易未开启，请在交易时间内进行交易|
|20002|该币种对暂时不允许交易|
|20003|您的挂单价格或数量有误|
|20007|价格精度限制|
|20008|数量精度限制|
|20009|最小交易数量限制|
|20010|最小交易额限制|
|20011|账户余额不足|
|20012|无效的交易类型，正确类型：buy/sell|
|20013|没有找到该订单|
|20014|日期格式不对，正确格式：2018-07-25|
|20015|order_history接口查询的订单日期超过限制|
|20018|您的交易权限已被系统限制|
|20019|交易对symbol错误，正确格式："usdt_btc"，计价币在前交易币在后|

## 参数签名
在调用本文档接口时，会多次使用到参数签名sigh，该签名是将调用接口时使用到的参数（不包括sign）和私钥apiSecret按照参数名排序，再把参数值拼接成一个字符串进行MD5运算得到。

具体实例如下，以获取K线接口为例：

```
[PHP]
<?php
    $arr = [
        'type' => 'kline_1m',
        'symbol' => 'usdt_btc',
        'timestamp' => TimeStampNow,
        'apiKey' => 'YOUR_APIKEY',
        'apiSecret' => 'YOUR_APISECRET'
    ];
    ksort($arr);
    $string = implode('', array_values($arr));
    $sign = md5($string);
    echo $sign;
?>

[NodeJS]
	let md5 = require('md5');
	const APIKEY = 'YOUR_APIKEY';
	const APISECRET = 'YOUR_APISECRET';
	let params = {type: 'kline_1m', symbol: 'usdt_btc', timestamp: TimeStampNow, apiKey: APIKEY, apiSecret: APISECRET}; 
	let keys = Object.keys(params).sort(), arr = [];
	keys.forEach(function(key){
		arr.push(params[key]);
	});
	let sign = md5(arr.join(''));
	console.log(sign);

[Python2.7]
    #!/usr/bin/python
    import hashlib
    m = hashlib.md5()
    APIKEY = 'YOUR_APIKEY'
    APISECRET = 'YOUR_APISECRET'
    params = {'symbol': 'usdt_btc', 'type': 'kline_1m', 'timestamp': TimeStampNow, 'apiKey': APIKEY, 'apiSecret': APISECRET}
    keys = sorted(params.keys())
    str = ''
    for key in keys:
        str = str + params[key]
    m.update(str)
    encodestr = m.hexdigest()
    print encodestr

[JAVA]
	import java.util.*;
	import java.security.MessageDigest;
	
	public class Demo {
		private static String md5(String s) {
			try {
				MessageDigest md = MessageDigest.getInstance("MD5");
				byte[] bytes = md.digest(s.getBytes("utf-8"));
				return toHex(bytes);
			}
			catch (Exception e) {
				throw new RuntimeException(e);
			}
		}
	
		private static String toHex(byte[] bytes) {
			final char[] hexDigits = "0123456789abcdef".toCharArray();
			StringBuilder ret = new StringBuilder(bytes.length * 2);
			for (int i=0; i<bytes.length; i++) {
				ret.append(hexDigits[(bytes[i] >> 4) & 0x0f]);
				ret.append(hexDigits[bytes[i] & 0x0f]);
			}
			return ret.toString();
		}
	
		public static void main(String[] args) {
			Map<String, Object> treemap = new TreeMap<String, Object>();
			treemap.put("symbol", "usdt_btc");
			treemap.put("type", "kline_1m");
			treemap.put("timestamp", new Date().getTime()/1000);
			treemap.put("apiKey", "YOUR_APIKEY");
			treemap.put("apiSecret", "YOUR_APISECRET");
	
			String str = "";
			Iterator titer = treemap.entrySet().iterator();
			while(titer.hasNext()){
				Map.Entry ent = (Map.Entry)titer.next();
		   		String key = ent.getKey().toString();
		   		String value = ent.getValue().toString();
				str += value;
			}
			System.out.println(str);
			System.out.println(md5(str));
	   }
	}
```

## 市场行情
### 交易对当前价格

* URL：`https://openapi.digifinex.com/v2/ticker`
* 请求方法: GET
* 请求参数: 

|参数名			|参数类型		|必填		|描述|
| :-----   	| :-----   	| :-----  | :-----   |
|symbol		|string		|0			|要查询的交易对，例如usdt_btc(计价币在前交易币在后)，不填则返回全部授权交易对|
|apiKey		|string		|1			|你的APIKEY|

* 示例：

```
# Request
GET https://openapi.digifinex.com/v2/ticker?apiKey=59328e10e296a

# Response
{
	"code":0,
	"date":1410431266,
	"ticker":{
		"usdt_btc": {
			"buy":6713.12,
			"high":6751.00,
			"last":6713.95,
			"low":6617.60,
			"sell":6714.12,
			"vol":15091.39199642,
			"change":0.0108
		},
		"btc_eth":{
			"buy":0.119241,
			"high":0.121186,
			"last":0.119341,
			"low":0.117405,
			"sell":0.119341,
			"vol":39159.39199642,
			"change":-0.0812
		},
		...
	}
}
```

* 返回值说明：

```
code: 错误码
date: 服务器返回数据时的时间戳（UTC+8） 
usdt_btc: 交易对symbol，表示以usdt作为计价币，btc作为交易币
buy: 买一价
high: 24h最高价
last: 最新成交价
low: 24h最低价
sell: 卖一价
vol: 24h成交量
change: 24h涨跌幅百分比（当前价格与24h前价格相比）, 取值0.0108表示价格上涨1.08%

```

### OTC市场价查询
* URL：`https://openapi.digifinex.com/v2/otc_market_price`
* 请求方法: GET
* 请求参数: 

|参数名			|参数类型		|必填		|描述|
| :-----   	| :-----   	| :-----  | :-----   |
|apiKey		|string		|1			|你的APIKEY|
|timestamp	|int			|1			|请求接口时的UTC+8时间戳，例timestamp=1410431266|
|sign			|string		|1			|参数签名|

* 示例：

```
# Request
GET https://openapi.digifinex.com/v2/otc_market_price?apiKey=59328e10e296a&timestamp=1410431266&sign=0a8d39b515fd8f3f8b848a4c459884c2

# Response
{
	"code":0,
	"date":1410431266,
	"price":{
		"cny_usdt":6.4
	}
}

```

* 返回值说明：

```
code: 错误码
date: 服务器返回数据时的时间戳（UTC+8）
price: 市场价
	cny_usdt:通过otc代理购买出售usdt的中间价
```


### 买卖盘深度
* URL：`https://openapi.digifinex.com/v2/depth`
* 请求方法: GET
* 请求参数: 

|参数名			|参数类型		|必填		|描述|
| :-----   	| :-----   	| :-----  | :-----   |
|symbol		|string		|1			|要查询的交易对，例如usdt_btc|
|apiKey		|string		|1			|你的APIKEY|
|timestamp	|int			|1			|请求接口时的UTC+8时间戳，例timestamp=1410431266|
|sign			|string		|1			|参数签名|

* 示例：

```
# Request
GET https://openapi.digifinex.com/v2/depth?symbol=usdt_btc&apiKey=59328e10e296a&timestamp=1410431266&sign=0a8d39b515fd8f3f8b848a4c459884c2

# Response
{
	"code":0,
	"date":1410431266,
	"asks": [
		[6967.15, 0.1234],  //[价格, 数量]
		[6967.14, 0.1234],
		...
	],
	"bids":[
		[6967.13, 0.1234],  //[价格, 数量]
		[6967.12, 0.1234],
		...
	]
}
```

* 返回值说明：

```
code: 错误码
date: 服务器返回数据时的时间戳（UTC+8） 
asks: 卖方深度，按价格倒序排列
	[价格，数量]
bids: 买方深度，按价格倒序排列
	[价格，数量]

```


### 最新成交记录
* URL：`https://openapi.digifinex.com/v2/trade_detail`
* 请求方法: GET
* 请求参数: 

|参数名			|参数类型		|必填		|描述|
| :-----   	| :-----   	| :-----  | :-----   |
|symbol		|string		|1			|要查询的交易对，例如usdt_btc|
|apiKey		|string		|1			|你的APIKEY|
|timestamp	|int			|1			|请求接口时的UTC+8时间戳，例timestamp=1410431266|
|sign			|string		|1			|参数签名|

* 示例：

```
# Request
GET https://openapi.digifinex.com/v2/trade_detail?symbol=usdt_btc&apiKey=59328e10e296a&timestamp=1410431266&sign=0a8d39b515fd8f3f8b848a4c459884c2

# Response
{
	"code":0,
	"data":[			//按时间倒序排列
		{
			"date": 1420431266,
			"price": 787.71,
			"amount": 0.003,
			"type": "sell"
		},
		{
			"date": 1410431266,
			"price": 787.5,
			"amount": 0.091,
			"type": "buy"
		},
		...
	]
}

```

* 返回值说明：

```
code: 错误码
date: 成交时间
price: 交易价格
amount: 交易数量
type: buy/sell

```



### K线数据
* URL：`https://openapi.digifinex.com/v2/kline`
* 请求方法: GET
* 请求参数: 

|参数名			|参数类型		|必填		|描述|
| :-----   	| :-----   	| :-----  | :-----   |
|symbol		|string		|1			|要查询的交易对，例如usdt_btc|
|type			|string		|1			|K线类型: kline\_1m/kline\_5m/kline\_15m/kline\_30m/kline\_1h/kline\_4h/kline\_12h/kline\_1d/kline\_1w|
|apiKey		|string		|1			|你的APIKEY|
|timestamp	|int			|1			|请求接口时的UTC+8时间戳，例timestamp=1410431266|
|sign			|string		|1			|参数签名|

* 示例：

```
# Request
GET https://openapi.digifinex.com/v2/kline?symbol=usdt_btc&type=kline_1m&apiKey=59328e10e296a&timestamp=1410431266&sign=0a8d39b515fd8f3f8b848a4c459884c2

# Response
{
	"code":0,
	"data":[		//按时间正序排列
		[
			1410431226, //起始时间
			0.81428, //交易量 
			0.00929514, //收盘 
			0.00939414, //最高 
			0.00929618, //最低 
			0.00929313 //开盘
		],
		[
			1410431366, //起始时间
			0.81428, //交易量 
			0.00929414, //收盘
			0.00949888, //最高 
			0.00929999, //最低 
			0.00929111 //开盘
		],
		...
	]
}

```

* 返回值说明：

```
code: 错误码
1410431266, //起始时间
0.81428, //交易量 
0.00929514, //收盘 
0.00939514, //最高 
0.00929371, //最低 
0.00929371 //开盘	

```


## 交易
### 交易对信息查询
> 获取开通权限的交易对，以及每个交易对的下单精度限制

* URL：`https://openapi.digifinex.com/v2/trade_pairs`
* 请求方法: GET
* 请求参数: 

|参数名			|参数类型		|必填		|描述|
| :-----   	| :-----   	| :-----  | :-----   |
|apiKey		|string		|1			|你的APIKEY|
|timestamp	|int			|1			|请求接口时的UTC+8时间戳，例timestamp=1410431266|
|sign			|string		|1			|参数签名|

* 示例：

```
# Request
GET https://openapi.digifinex.com/v2/trade_pairs?apiKey=59328e10e296a&timestamp=1410431266&sign=0a8d39b515fd8f3f8b848a4c459884c2

# Response
{
	"code":0,
	"date":1410431266,
	"data":{
		"usdt_btc":[4，2，0.001，10.0],
		"usdt_eth":[4，2，0.01，10.0],
		"btc_eth":[4，4，0.01，0.001],
		...
	}
}

```

* 返回值说明：

```
code: 错误码
date: 服务器返回数据时的时间戳（UTC+8）
[4，2，0.001，10.0]：数量精度，价格精度，最小下单数量，最小下单金额
例："usdt_btc":[4，2，0.001，10.0]
BTC对USDT交易对，下单数量（BTC量）支持4位小数，下单价格（以USDT计价）支持2位小数，最小下单0.001BTC，最小下单总金额10USDT。必须同时满足以上条件才可下单成功。

```

### 限价下单

* URL：`https://openapi.digifinex.com/v2/trade`
* 请求方法: POST
* 请求参数: 

|参数名			|参数类型		|必填		|描述|
| :-----   	| :-----   	| :-----  | :-----   |
|symbol		|string		|1			|要交易的交易对，例：usdt_btc|
|price			|float			|1			|交易价格|
|amount		|float			|1			|交易数量|
|type			|string		|1			|交易类型：buy/sell|
|post_only	|int			|0			|1:使用，0:不使用。post_only订单将保证该订单总是支付maker手续费，该订单如果不能进入买卖盘则会自动取消。|
|apiKey		|string		|1			|你的APIKEY|
|timestamp	|int			|1			|请求接口时的UTC+8时间戳，例timestamp=1410431266|
|sign			|string		|1			|参数签名|

* 示例：

```
# Request
POST https://openapi.digifinex.com/v2/trade
POST参数: 
	symbol=usdt_btc
	price=6000.12
	amount=0.1
	type=buy
	apiKey=59328e10e296a
	timestamp=1410431266
	sign=0a8d39b515fd8f3f8b848a4c459884c2

# Response
{
	"code":0,
	"order_id":"1000001"
}

```

* 返回值说明：

```
code: 错误码
order_id: 订单ID
```

### 活跃订单查询
* URL：`https://openapi.digifinex.com/v2/open_orders`
* 请求方法: GET
* 请求参数: 

|参数名			|参数类型		|必填		|描述|
| :-----   	| :-----   	| :-----  | :-----   |
|symbol		|string		|0			|要查询交易对，例：usdt_btc，不填返回全部交易对|
|type			|string		|0			|交易类型：buy/sell/buy\_market/sell\_market，不填则返回全部类型|
|page			|int			|0			|要查询的订单页码，page=1表示第一页，不填默认返回第1页|
|apiKey		|string		|1			|你的APIKEY|
|timestamp	|int			|1			|请求接口时的UTC+8时间戳，例timestamp=1410431266|
|sign			|string		|1			|参数签名|

* 示例：

```
# Request
GET https://openapi.digifinex.com/v2/open_orders?symbol=usdt_btc&page=1&apiKey=59328e10e296a&timestamp=1410431266&sign=0a8d39b515fd8f3f8b848a4c459884c2

# Response
{
	"code":0,
	"date":1410431266,
	“total”:123,
	"page":1,
	"num_per_page":20,
	"orders":[			//按时间倒序排列
		{
			"order_id":"1234567",
			"created_date":1420431266,
			"symbol": "usdt_btc",
			"price":6000.12,
			"amount":0.0,
			"executed_amount":0.1,
			"cash_amount":1000.12,
			"avg_price":6000.12,
			"type":"buy_market",
			"status":1
		},
		{
			"order_id":"1234568",
			"created_date":1410431266,
			"symbol": "usdt_btc",
			"price":6001.12,
			"amount":0.2,
			"executed_amount":0.0,
			"cash_amount":0.0,
			"avg_price":0.0,
			"type":"sell",
			"status":0
		},
		...
	]
}

```

* 返回值说明：

```
code: 错误码
date: 服务器返回数据时的时间戳（UTC+8）
total: 活跃订单总数
page: 当前页码
num_per_page: 每页返回的记录条数
orders: 订单信息按照下单时间倒序排列
	order_id: 订单号
	created_date: 下单时间
	symbol: 交易对
	price: 订单价格
	amount: 订单数量
	executed_amount: 已成交数量
	cash_amount: 委托金额，不适用显示0.0
	avg_price: 平均成交价格，未成交显示0.0
	type: buy限价买单，sell限价卖单，buy_market市价买单，sell_market市价卖单
	status: 订单状态：0未成交 1部分成交
```


### 历史订单查询
> 不包括活跃订单，只支持查询最近3天的历史订单

* URL：`https://openapi.digifinex.com/v2/order_history`
* 请求方法: GET
* 请求参数: 

|参数名			|参数类型		|必填		|描述|
| :-----   	| :-----   	| :-----  | :-----   |
|symbol		|string		|0			|要查询交易对，例：usdt_btc，不填返回全部交易对|
|date			|string		|0			|要查询的下单日期(UTC+8)，例date=2018-07-18，不填默认为当天。|
|type			|string		|0			|交易类型：buy/sell/buy\_market/sell\_market，不填则返回全部类型|
|page			|int			|0			|要查询的订单页码，page=1表示第一页，不填默认返回第1页|
|apiKey		|string		|1			|你的APIKEY|
|timestamp	|int			|1			|请求接口时的UTC+8时间戳，例timestamp=1410431266|
|sign			|string		|1			|参数签名|

* 示例：

```
# Request
GET https://openapi.digifinex.com/v2/order_history?apiKey=59328e10e296a&timestamp=1410431266&sign=0a8d39b515fd8f3f8b848a4c459884c2

# Response
{
	"code":0,
	"date":1410431266,
	"total":123,
	"page":1,
	"num_per_page":20,
	"orders":[			//按时间倒序排列
		{
			"order_id":"1234567",
			"created_date":1430431266,
			"finished_date":1420431266,
			"symbol": "usdt_btc",
			"price":6000.12,
			"amount":0.2,
			"executed_amount":0.1,
			"cash_amount":0.0,
			"avg_price":6000.00,
			"type":"buy",
			"status":1
		},
		{
			"order_id":"1234568",
			"created_date":1410431266,
			"finished_date":1440431266,
			"symbol": "usdt_btc",
			"price":6001.12,
			"amount":0.2,
			"executed_amount":0,
			"cash_amount":0.0,
			"avg_price":0.0,
			"type":"sell",
			"status":0
		},
		{
			"order_id":"1234568",
			"created_date":1400431266,
			"finished_date":1460431266,
			"symbol": "usdt_btc",
			"price":6001.12,
			"amount":0.2,
			"executed_amount":0.2,
			"cash_amount":0.0,
			"avg_price":6000.11,
			"type":"sell",
			"status":2
		},

		...
	]
}

```

* 返回值说明：

```
code: 错误码
date: 服务器返回数据时的时间戳（UTC+8）
total: 所选日期记录总条数
page: 当前页码
num_per_page: 每页返回的记录条数
orders: 订单信息按照下单时间倒序排列
	order_id: 订单号
	created_date: 下单时间
	finished_date: 撤销订单或完全成交时间
	symbol: 交易对
	price: 订单价格
	amount: 订单数量
	executed_amount: 已成交数量
	cash_amount: 委托金额，不适用显示0.0
	avg_price: 平均成交价格，未成交显示0.0
	type: buy限价买单，sell限价卖单，buy_market市价买单，sell_market市价卖单
	status: 订单状态：2全部成交 3已撤销未成交 4已撤销部分成交
```

### 订单信息查询

* URL：`https://openapi.digifinex.com/v2/order_info`
* 请求方法: GET
* 请求参数: 

|参数名			|参数类型		|必填		|描述|
| :-----   	| :-----   	| :-----  | :-----   |
|order_id		|string		|1			|要查询的订单ID，多个订单用逗号分隔，最多支持20个订单|
|apiKey		|string		|1			|你的APIKEY|
|timestamp	|int			|1			|请求接口时的UTC+8时间戳，例timestamp=1410431266|
|sign			|string		|1			|参数签名|

* 示例：

```
# Request
GET https://openapi.digifinex.com/v2/order_info?order_id=1000001,1000002&apiKey=59328e10e296a&timestamp=1410431266&sign=0a8d39b515fd8f3f8b848a4c459884c2

# Response
{
	"code":0,
	"data":[
		{
			"order_id":"1000001",
			"created_date":1410431266,
			"finished_date":0,
			"price":6001.12,
			"amount":0.2,
			"executed_amount":0.2,
			"cash_amount":0.0,
			"avg_price":6000.11,
			"type":"buy",
			"status":2
		},
		{
			"order_id":"1000002",
			"created_date":1510431266,
			"finished_date":0,
			"price":6001.12,
			"amount":0.2,
			"executed_amount":0.2,
			"cash_amount":0.0,
			"avg_price":6000.11,
			"type":"buy",
			"status":2
		}
	]
}
```

* 返回值说明：

```
code: 错误码
order_id: 订单号
created_date: 下单时间
finished_date: 订单全部成交时间或撤单时间
price: 委托价格
amount: 委托数量
executed_amount: 已成交数量
cash_amount: 委托金额，不适用显示0.0
avg_price: 平均成交价格
type: buy限价买单，sell限价卖单，buy_market市价买单，sell_market市价卖单
status: 订单状态：0未成交 1部分成交 2全部成交 3已撤销未成交 4已撤销部分成交
```



### 订单成交明细

* URL：`https://openapi.digifinex.com/v2/order_detail`
* 请求方法: GET
* 请求参数: 

|参数名			|参数类型		|必填		|描述|
| :-----   	| :-----   	| :-----  | :-----   |
|order_id		|string		|1			|要查询的订单ID|
|apiKey		|string		|1			|你的APIKEY|
|timestamp	|int			|1			|请求接口时的UTC+8时间戳，例timestamp=1410431266|
|sign			|string		|1			|参数签名|

* 示例：

```
# Request
GET https://openapi.digifinex.com/v2/order_detail?order_id=1000001&apiKey=59328e10e296a&timestamp=1410431266&sign=0a8d39b515fd8f3f8b848a4c459884c2

# Response
{
	"code":0,
	"order_id":"1234568",
	"created_date":1410431266,
	"finished_date":0,
	"price":6001.12,
	"amount":0.2,
	"executed_amount":0.2,
	"cash_amount":0.0,
	"avg_price":6000.11,
	"type":"buy",
	"status":2
	"detail":[	//按时间倒序排列
		{
			"date":1430431266,
			"executed_amount":0.10000000,
			"executed_price":6000.11,
			"tid":"230435"
		},
		{
			"date":1420431266,
			"executed_amount":0.10000000,
			"executed_price":6000.11,
			"tid":"230436"
		},
		...
	]
}
```

* 返回值说明：

```
code: 错误码
order_id: 订单号
created_date: 下单时间
finished_date: 订单全部成交时间或撤单时间
price: 委托价格
amount: 委托数量
executed_amount: 已成交数量
cash_amount: 委托金额，不适用显示0.0
avg_price: 平均成交价格
type: buy限价买单，sell限价卖单，buy_market市价买单，sell_market市价卖单
status: 订单状态：0未成交 1部分成交 2全部成交 3已撤销未成交 4已撤销部分成交
detail: 具体成交明细（仅支持最近15天的数据）
	date: 成交时间
	executed_amount: 成交数量
	executed_price: 成交价格
	tid: 交易ID
```


### 撤销订单
* URL：`https://openapi.digifinex.com/v2/cancel_order`
* 请求方法: POST
* 请求参数: 

|参数名			|参数类型		|必填		|描述|
| :-----   	| :-----   	| :-----  | :-----   |
|order_id		|string		|1			|要撤销的订单ID，多个订单用逗号分隔，最多支持20个订单|
|apiKey		|string		|1			|你的APIKEY|
|timestamp	|int			|1			|请求接口时的UTC+8时间戳，例timestamp=1410431266|
|sign			|string		|1			|参数签名|

* 示例：

```
# Request
POST https://openapi.digifinex.com/v2/cancel_order
POST参数: 
	order_id=1000001,1000002,1000003
	apiKey=59328e10e296a
	timestamp=1410431266
	sign=0a8d39b515fd8f3f8b848a4c459884c2

# Response
{
	"code":0,
	"date":1410431266,
	"success":["1000001", "1000002"],
	"error":["1000003", "1000004"]
}

```

* 返回值说明：

```
code: 错误码
date: 服务器返回数据时的时间戳（UTC+8）
success: 撤销成功的订单ID
error: 撤销失败的订单ID
```


### 查询自己的持仓
* URL：`https://openapi.digifinex.com/v2/myposition`
* 请求方法: GET
* 请求参数: 

|参数名			|参数类型		|必填		|描述|
| :-----   	| :-----   	| :-----  | :-----   |
|apiKey		|string		|1			|你的APIKEY|
|timestamp	|int			|1			|请求接口时的UTC+8时间戳，例timestamp=1410431266|
|sign			|string		|1			|参数签名|

* 示例：

```
# Request
GET https://openapi.digifinex.com/v2/myposition?apiKey=59328e10e296a&timestamp=1410431266&sign=0a8d39b515fd8f3f8b848a4c459884c2

# Response
{
	"code":0,
	"date":1410431266,
	"free":{
		"usdt":123.12345678,
		"btc":0.12345678,
		...
	},
	"frozen":{
		"usdt":123.12345678,
		"btc":0.12345678,
		...
	}
}

```

* 返回值说明：

```
code: 错误码
date: 服务器返回数据时的时间戳（UTC+8）
free: 账户可用余额
frozen: 账户冻结余额

```