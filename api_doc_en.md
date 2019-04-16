# DigiFinex API Documentation
> Version：1.1.12, Update: 2018-02-28, ©️DigiFinex
> 
> The upper limit of Api request frequency is 300 times/min (all POST requests are accumulated) for POST request and 900 times/min (all GET requests are accumulated) for GET request. When this limit is exceeded, all Api request will be forbidden for 5 minutes. Be attention, if you exceed this limit multiple times, your account may be blocked. 
> 
> The field contentType in request header should be: application/x-www-form-urlencoded
> 
> Join the Telegram Group for more help: https://t.me/digifinex_api


## Get ApiKey and ApiSecret
Go to DigiFinex's official website (www.digifinex.vip) and register an account. After login, go to 'User Center' -> 'API Setting' to apply your ApiKey and ApiSecret.


## Error Code
The parameter 'code' is included in the response of each Api. If its value is 0, it means that the request is successful and the return data is valid. For non-zero values, please check the table below to find error explanations. 

|code|Description|
|:----|:-----|    
|0|Success|
|10001|Wrong request method, please check it's a GET ot POST request|
|10002|Invalid ApiKey|
|10003|Sign doesn't match|
|10004|Illegal request parameters|
|10005|Request frequency exceeds the limit|
|10006|Unauthorized to execute this request|
|10007|IP address Unauthorized|
|10008|Timestamp for this request is invalid, timestamp must within 1 minute|
|10009|Unexist endpoint, please check endpoint URL|
|10011|ApiKey expired. Please go to client side to re-create an ApiKey.|
|20001|Trade is not open for this trading pair|
|20002|Trade of this trading pair is suspended|
|20003|Invalid price or amount|
|20007|Price precision error|
|20008|Amount precision error|
|20009|Amount is less than the minimum requirement|
|20010|Cash Amount is less than the minimum requirement|
|20011|Insufficient balance|
|20012|Invalid trade type (valid value: buy/sell)|
|20013|No order info found|
|20014|Invalid date (Valid format: 2018-07-25)|
|20015|Date exceeds the limit|
|20018|Your trading rights have been banned by the system|
|20019|Wrong trading pair symbol, correct format:"usdt_btc", quote asset is in the front|
|50000|Exception error|

## Parameter signature 
The 'sign' parameter is demanded for each api request. Firstly sort all needed parameters (without sign) and ApiSecret in ascending order by parameters' name. Then connect all the sorted parameters' value into a string. At last perform a MD5 on this string.

Take the kline api for example:

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

[C#]
	using System;
	using System.Collections.Generic;
	using System.Security.Cryptography;
	using System.Text;
	
	namespace ConsoleApp1
	{
	    class Program
	    {
	        static void Main(string[] args)
	        {
	            DateTimeOffset epoch = new DateTimeOffset(1970, 1, 1, 0, 0, 0, TimeSpan.Zero);
	            var TimeStampNow = (DateTimeOffset.Now - epoch).TotalSeconds;
	            
	            var param = new SortedDictionary<string, string>
	            {
	                { "type", "kline_1m" },
	                { "symbol", "usdt_btc" },
	                { "timestamp", ((int)TimeStampNow).ToString() },
	                { "apiKey", "YOUR_APIKEY" },
	                { "apiSecret", "YOUR_APISECRET" },
	            };
	            
	            var str = String.Join("", param.Values);
	            StringBuilder sb = new StringBuilder();
	            using (var md5 = MD5.Create())
	            {
	                md5.ComputeHash(Encoding.UTF8.GetBytes(str));
	                for (int i = 0; i < md5.Hash.Length; i++)
	                {
	                    sb.Append(md5.Hash[i].ToString("x2"));
	                }
	            }
	            
	            var result = sb.ToString();
	            Console.WriteLine(result);
	            Console.ReadLine();
	        }
	    }
	}

[C++]
	#include <iostream>
	#include <string>
	#include <map>
	#include <time.h>
	#include <openssl/md5.h>
	
	#pragma comment(lib, "libeay32MDd")
	
	using namespace std;
	
	string calculateMD5(string msg)
	{
	    string result;
	    const char* test = msg.c_str();
	    int i;
	    MD5_CTX md5;
	    char buf[32];
	    unsigned char buffer_md5[16];
	
	    MD5_Init(&md5);
	    MD5_Update(&md5, (const unsigned char *)test, msg.length());
	    MD5_Final(buffer_md5, &md5);
	    for (i = 0; i < 16; i++) {
	        sprintf(buf, "%02x", buffer_md5[i]);
	        result.append(buf);
	    }
	
	    return result;
	}
	
	int main() {
	    char strTimestamp[32];
	    sprintf(strTimestamp, "%d", time(0));
	    string str;
	    map<string, string> params
	    {
	        {"type",        "kline_1m"},
	        {"symbol",      "usdt_btc"},
	        {"timestamp",   strTimestamp},
	        {"apiKey",      "YOUR_APIKEY"},
	        {"apiSecret",   "YOUR_APISECRET"},
	    };
	    for (auto pair : params)
	    {
	        str += pair.second;
	    }
	
	    auto md5 = calculateMD5(str);
	    cout << md5 << endl;
	
	    return 0;
	}
```

## Market Information
### Ticker price

* URL：`https://openapi.digifinex.vip/v2/ticker`
* Request Method: GET
* Request Parameters: 

|Param Name	|Type			|Mandatory|Description|
| :-----   	| :-----   	| :-----  | :-----   |
|symbol		|string		|0			|Specified trading pair, e.g. usdt_btc (quote asset is in the front). None for all authorized trading pairs.|
|apiKey		|string		|1			|Your ApiKey|


* Example：

```
# Request
GET https://openapi.digifinex.vip/v2/ticker?apiKey=59328e10e296a

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

* Return value explanation：

```
code: Error Code
date: Second timestamp whern server returned the response 
usdt_btc: Trading pair symbol, usdt is the quote asset and btc is the base asset
buy: 1st bid price
high: 24h highest price
last: latest price
low: 24h lowest price
sell: 1st ask price
vol: 24h volume
change: 24h Change（compared with price 24h ago）, 0.0108 means +1.08% increasement

```

### OTC Market Price
* URL：`https://openapi.digifinex.vip/v2/otc_market_price`
* Request Method: GET
* Request Parameters: 

|Param Name	|Type			|Mandatory|Description|
| :-----   	| :-----   	| :-----  | :-----   |
|apiKey		|string		|1			|Your ApiKey|
|timestamp	|int			|1			|The second timestamp(UTC+8) when the request was sent，e.g. timestamp=1410431266|
|sign			|string		|1			|Parameter signature|

* Example：

```
# Request
GET https://openapi.digifinex.vip/v2/otc_market_price?apiKey=59328e10e296a&timestamp=1410431266&sign=0a8d39b515fd8f3f8b848a4c459884c2

# Response
{
	"code":0,
	"date":1410431266,
	"price":{
		"cny_usdt":6.4
	}
}

```

* Return value explanation：

```
code: Error Code
date: Second timestamp whern server returned the response
price: Market Price
	cny_usdt: Specified trading pair's market price
```


### Market Depth
* URL：`https://openapi.digifinex.vip/v2/depth`
* Request Method: GET
* Request Parameters: 

|Param Name	|Type			|Mandatory|Description|
| :-----   	| :-----   	| :-----  | :-----   |
|symbol		|string		|1			|Specified trading pair, e.g. usdt_btc|
|apiKey		|string		|1			|Your ApiKey|
|timestamp	|int			|1			|The second timestamp(UTC+8) when the request was sent，e.g. timestamp=1410431266|
|sign			|string		|1			|Parameter signature|

* Example：

```
# Request
GET https://openapi.digifinex.vip/v2/depth?symbol=usdt_btc&apiKey=59328e10e296a&timestamp=1410431266&sign=0a8d39b515fd8f3f8b848a4c459884c2

# Response
{
	"code":0,
	"date":1410431266,
	"asks": [
		[6967.15, 0.1234],  //[Price, Amount]
		[6967.14, 0.1234],
		...
	],
	"bids":[
		[6967.13, 0.1234],  //[Price, Amount]
		[6967.12, 0.1234],
		...
	]
}
```

* Return value explanation：

```
code: Error Code
date: Second timestamp whern server returned the response 
asks: Ask depth in price descending order 
	[Price, Amount]
bids: Bid depth in price descending order
	[Price, Amount]

```


### Recent Trade List
* URL：`https://openapi.digifinex.vip/v2/trade_detail`
* Request Method: GET
* Request Parameters: 

|Param Name	|Type			|Mandatory|Description|
| :-----   	| :-----   	| :-----  | :-----   |
|symbol		|string		|1			|Specified trading pair, e.g. usdt_btc|
|apiKey		|string		|1			|Your ApiKey|
|timestamp	|int			|1			|The second timestamp(UTC+8) when the request was sent，e.g. timestamp=1410431266|
|sign			|string		|1			|Parameter signature|

* Example：

```
# Request
GET https://openapi.digifinex.vip/v2/trade_detail?symbol=usdt_btc&apiKey=59328e10e296a&timestamp=1410431266&sign=0a8d39b515fd8f3f8b848a4c459884c2

# Response
{
	"code":0,
	"data":[			//in time descending order
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

* Return value explanation：

```
code: Error Code
date: Deal second timestamp
price: Executed price
amount: Executed amount
type: buy/sell, buy means the maker is the buyer

```



### K-line data
* URL：`https://openapi.digifinex.vip/v2/kline`
* Request Method: GET
* Request Parameters: 

|Param Name	|Type			|Mandatory|Description|
| :-----   	| :-----   	| :-----  | :-----   |
|symbol		|string		|1			|Specified trading pair, e.g. usdt_btc|
|type			|string		|1			|Kline type: kline\_1m/kline\_5m/kline\_15m/kline\_30m/kline\_1h/kline\_4h/kline\_12h/kline\_1d/kline\_1w|
|apiKey		|string		|1			|Your ApiKey|
|timestamp	|int			|1			|The second timestamp(UTC+8) when the request was sent，e.g. timestamp=1410431266|
|sign			|string		|1			|Parameter signature|

* Example：

```
# Request
GET https://openapi.digifinex.vip/v2/kline?symbol=usdt_btc&type=kline_1m&apiKey=59328e10e296a&timestamp=1410431266&sign=0a8d39b515fd8f3f8b848a4c459884c2

# Response
{
	"code":0,
	"data":[		//in time ascending order
		[
			1410431326, //open timestamp of this line
			0.81428, //volume 
			0.00929514, //close
			0.00939414, //high
			0.00929618, //low
			0.00929313 //open
		],
		[
			1420431266, //open timestamp of this line
			0.81428, //volume 
			0.00929414, //close 
			0.00939888, //high
			0.00929999, //low
			0.00929111 //open
		],
		...
	]
}

```

* Return value explanation：

```
code: Error Code
1410431266, //open timestamp of this line
0.81428, //volume 
0.00929514, //close 
0.00939514, //high
0.00929371, //low
0.00929371 //open

```


## Trade
### Trading pair information
> To obtain authorized trading pairs and the precision limit for placing orders

* URL：`https://openapi.digifinex.vip/v2/trade_pairs`
* Request Method: GET
* Request Parameters: 

|Param Name	|Type			|Mandatory|Description|
| :-----   	| :-----   	| :-----  | :-----   |
|apiKey		|string		|1			|Your ApiKey|
|timestamp	|int			|1			|The second timestamp(UTC+8) when the request was sent，e.g. timestamp=1410431266|
|sign			|string		|1			|Parameter signature|

* Example：

```
# Request
GET https://openapi.digifinex.vip/v2/trade_pairs?apiKey=59328e10e296a&timestamp=1410431266&sign=0a8d39b515fd8f3f8b848a4c459884c2

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

* Return value explanation：

```
code: Error Code
date: Second timestamp whern server returned the response
[4，2，0.001，10.0]：amount precision, price precision, minimum amount, minimum cash amount (cash amount = price * amount) 

E.g. "usdt_btc":[4，2，0.001，10.0]
	Quote asset is usdt, base asset is btc. 
	Amount(BTC) supports 4 decimal places, and price(USDT) supports 2 decimal places. 
	The minimum amount is 0.001BTC and the minimum cash amount is 10USDT. 
	The limit order would be placed successfully if all the requirements above are fulfilled. 
```

### Limit price order

* URL：`https://openapi.digifinex.vip/v2/trade`
* Request Method: POST
* Request Parameters: 

|Param Name	|Type			|Mandatory|Description|
| :-----   	| :-----   	| :-----  | :-----   |
|symbol		|string		|1			|Specified trading pair, e.g. usdt_btc|
|price			|float			|1			|Price|
|amount		|float			|1			|Amount|
|type			|string		|1			|Type: buy/sell|
|post_only	|int			|0			|1: yes, 0: no. post_only order will always pay the maker fee. If it is not sent into order book, this order will be cancelled automatically|
|apiKey		|string		|1			|Your ApiKey|
|timestamp	|int			|1			|The second timestamp(UTC+8) when the request was sent，e.g. timestamp=1410431266|
|sign			|string		|1			|Parameter signature|

* Example：

```
# Request
POST https://openapi.digifinex.vip/v2/trade
POST Parameters: 
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

* Return value explanation：

```
code: Error Code
order_id: Order ID
```

### Open orders
* URL：`https://openapi.digifinex.vip/v2/open_orders`
* Request Method: GET
* Request Parameters: 

|Param Name	|Type			|Mandatory|Description|
| :-----   	| :-----   	| :-----  | :-----   |
|symbol		|string		|0			|Sprcified trading pair, e.g. usdt_btc. None for all trading pair.|
|type			|string		|0			|Type：buy/sell/buy\_market/sell\_market，none for all types|
|page			|int			|0			|Page Num, page=1 for 1st page. None for 1st page by default. |
|apiKey		|string		|1			|Your ApiKey|
|timestamp	|int			|1			|The second timestamp(UTC+8) when the request was sent，e.g. timestamp=1410431266|
|sign			|string		|1			|Parameter signature|

* Example：

```
# Request
GET https://openapi.digifinex.vip/v2/open_orders?symbol=usdt_btc&page=1&apiKey=59328e10e296a&timestamp=1410431266&sign=0a8d39b515fd8f3f8b848a4c459884c2

# Response
{
	"code":0,
	"date":1410431266,
	“total”:123,
	"page":1,
	"num_per_page":20,
	"orders":[			//in time descending order
		{
			"order_id":"1234567",
			"created_date":1420431266,
			"symbol": "usdt_btc",
			"price":6000.12,
			"amount":0.2,
			"executed_amount":0.1,
			"cash_amount":1000.12,
			"avg_price":6000.12,
			"type":"buy",
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

* Return value explanation：

```
code: Error Code
date: Second timestamp whern server returned the response
total: total number of open orders
page: present page number
num_per_page: item quantity per page
orders: order detail in time descending order
	order_id: order ID
	created_date: second timestamp at order placement
	symbol: specified trading pair
	price: order price
	amount: order amount
	executed_amount: executed amount
	cash_amount: cash amount = price * amount
	avg_price: average executed price, 0.0 for unfilled
	type: buy, sell
	status: order status: 
		0: unfilled
		1: partially filled
```


### Order history
> Open order not included. Only supports historical orders of last 3 days

* URL：`https://openapi.digifinex.vip/v2/order_history`
* Request Method: GET
* Request Parameters: 

|Param Name	|Type			|Mandatory|Description|
| :-----   	| :-----   	| :-----  | :-----   |
|symbol		|string		|0			|Sprcified trading pair, e.g. usdt_btc. None for all trading pair.|
|date			|string		|0			|The date(UTC+8) to look up，e.g. date=2018-07-18. None for the present day.|
|type			|string		|0			|Type：buy/sell/buy\_market/sell\_market. None for all types|
|page			|int			|0			|Page Num, page=1 for 1st page. None for 1st page by default. |
|apiKey		|string		|1			|Your ApiKey|
|timestamp	|int			|1			|The second timestamp(UTC+8) when the request was sent，e.g. timestamp=1410431266|
|sign			|string		|1			|Parameter signature|

* Example：

```
# Request
GET https://openapi.digifinex.vip/v2/order_history?apiKey=59328e10e296a&timestamp=1410431266&sign=0a8d39b515fd8f3f8b848a4c459884c2

# Response
{
	"code":0,
	"date":1410431266,
	"total":123,
	"page":1,
	"num_per_page":20,
	"orders":[			//in time descending order
		{
			"order_id":"1234567",
			"created_date":1440431266,
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
			"created_date":1430431266,
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
			"created_date":1410431266,
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

* Return value explanation：

```
code: Error Code
date: Second timestamp whern server returned the response
total: Total number for specified date
page: present page
num_per_page: number per page
orders: order details
	order_id: order ID
	created_date: timestamp at order placement
	finished_date: timestamp at order fulfillment or cancel
	symbol: specified trading pair
	price: price
	amount: amount
	executed_amount: executed amount
	cash_amount: cash amount = price * amount
	avg_price: average executed price，0.0 for unfilled
	type: buy, sell
	status: order status: 
		2: fulfilled 
		3: unfilled and cancelled 
		4: partially filled and cancelled
```

### Order Information
> A quick lookup of order status, mutiple order IDs supported

* URL：`https://openapi.digifinex.vip/v2/order_info`
* Request Method: GET
* Request Parameters: 

|Param Name	|Type			|Mandatory|Description|
| :-----   	| :-----   	| :-----  | :-----   |
|order_id		|string		|1			|Order ID to look up，multiple orders are seperated by a comma ',', 20 IDs for maximum|
|apiKey		|string		|1			|Your ApiKey|
|timestamp	|int			|1			|The second timestamp(UTC+8) when the request was sent，e.g. timestamp=1410431266|
|sign			|string		|1			|Parameter signature|

* Example：

```
# Request
GET https://openapi.digifinex.vip/v2/order_info?order_id=1000001,1000002&apiKey=59328e10e296a&timestamp=1410431266&sign=0a8d39b515fd8f3f8b848a4c459884c2

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

* Return value explanation:

```
code: Error Code
order_id: order ID
created_date: timestamp at order placement
finished_date: timestamp at order fulfillment or cancel
price: price
amount: amount
executed_amount: executed amount
cash_amount: cash amount = price * amount
avg_price: average executed amount
type: buy, sell
status: order status：
	0: unfilled 
	1: partially filled 
	2: fulfilled 
	3: unfilled and cancelled 
	4: partially filled and cancelled
```


### Order detail

* URL：`https://openapi.digifinex.vip/v2/order_detail`
* Request Method: GET
* Request Parameters: 

|Param Name	|Type			|Mandatory|Description|
| :-----   	| :-----   	| :-----  | :-----   |
|order_id		|string		|1			|Order ID to look up|
|apiKey		|string		|1			|Your ApiKey|
|timestamp	|int			|1			|The second timestamp(UTC+8) when the request was sent，e.g. timestamp=1410431266|
|sign			|string		|1			|Parameter signature|

* Example：

```
# Request
GET https://openapi.digifinex.vip/v2/order_detail?order_id=1000001&apiKey=59328e10e296a&timestamp=1410431266&sign=0a8d39b515fd8f3f8b848a4c459884c2

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
	"detail":[	//in time descending order
		{
			"date":1420431266,
			"executed_amount":0.10000000,
			"executed_price":6000.11,
			"tid":"230435"
		},
		{
			"date":1410431266,
			"executed_amount":0.10000000,
			"executed_price":6000.11,
			"tid":"230436"
		},
		...
	]
}
```

* Return value explanation：

```
code: Error Code
order_id: order ID
created_date: timestamp at order placement
finished_date: timestamp at order fulfillment or cancel
price: price
amount: amount
executed_amount: executed amount
cash_amount: cash amount = price * amount
avg_price: average executed amount
type: buy, sell
status: order status：
	0: unfilled 
	1: partially filled 
	2: fulfilled 
	3: unfilled and cancelled 
	4: partially filled and cancelled
detail: transaction detail of last 15 days
	date: executed timestamp
	executed_amount: executed amount
	executed_price: executed_price
	tid: trade ID
```


### Cancel order
* URL：`https://openapi.digifinex.vip/v2/cancel_order`
* Request Method: POST
* Request Parameters: 

|Param Name	|Type			|Mandatory|Description|
| :-----   	| :-----   	| :-----  | :-----   |
|order_id		|string		|1			|Order ID to cancel，multiple orders are seperated by a comma ',', 20 IDs for maximum|
|apiKey		|string		|1			|Your ApiKey|
|timestamp	|int			|1			|The second timestamp(UTC+8) when the request was sent，e.g. timestamp=1410431266|
|sign			|string		|1			|Parameter signature|

* Example：

```
# Request
POST https://openapi.digifinex.vip/v2/order_info
POST Parameters: 
	order_id=1000001,1000002,1000003
	apiKey=59328e10e296a
	timestamp=1410431266
	sign=0a8d39b515fd8f3f8b848a4c459884c2

# Response
{
	"code":0,
	"date":1410431266,
	"success":["1000001","1000002"],
	"error":["1000003","1000004"]
}

```

* Return value explanation：

```
code: Error Code
date: Second timestamp whern server returned the response
success: Order IDs cancelled successfully
error: Order IDs failed to cancel
```


### My Position
* URL：`https://openapi.digifinex.vip/v2/myposition`
* Request Method: GET
* Request Parameters: 

|Param Name	|Type			|Mandatory|Description|
| :-----   	| :-----   	| :-----  | :-----   |
|apiKey		|string		|1			|Your ApiKey|
|timestamp	|int			|1			|The second timestamp(UTC+8) when the request was sent，e.g. timestamp=1410431266|
|sign			|string		|1			|Parameter signature|

* Example：

```
# Request
GET https://openapi.digifinex.vip/v2/myposition?apiKey=59328e10e296a&timestamp=1410431266&sign=0a8d39b515fd8f3f8b848a4c459884c2

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

* Return value explanation：

```
code: Error Code
date: Second timestamp whern server returned the response
free: amount free
frozen: amount frozen

```