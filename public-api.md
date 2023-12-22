
# Public API

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

 - [システム情報関連](#システム情報関連)
    - [取引ペア情報](#取引ペア情報)
    - [対応取引通貨](#対応取引通貨)
    - [システム時間を調べる](#システム時間を調べる)
    
  - [マーケット関連](#マーケット関連)
    - [ローソク足](#ローソク足)
    - [ティッカー](#ティッカー)
    - [全取引ペアの相場情報](#全取引ペアの相場情報)
    - [板情報](#板情報)
    - [最新の取引データ](#最新の取引データ)
    - [取引履歴の取得](#取引履歴の取得)
 

<!-- END doctoc generated TOC please keep comment here to allow auto update -->



## システム情報関連

### 取引ペア情報



```txt
GET /v1/common/symbols
```



**Reception Data**

Parameter | Description
------------ | ------------
status | リクエスト処理結果[ok/error]
data | 取引ペアの情報

**Data Field**

Field | Description
------------ | ------------
base-currency | ベース通貨
quote-currency | 見積通貨
price-precision | 価格の精度
amount-precision | 数量の精度
symbol-partition | 取引パーティション
symbol | 取引ペア
state | ステータス[online/offline/suspend]
min-order-amt | 最小取引数量
max-order-amt | 最大取引数量
limit-order-min-order-amt | 指値最小注文数量
limit-order-max-order-amt | 指値最大注文数量
limit-order-max-buy-amt | 指値最大注文量(buy)
limit-order-max-sell-amt | 指値最大注文量(sell)
sell-market-min-order-amt | 成行最小注文量(sell)
sell-market-max-order-amt | 成行最大注文量(sell)
buy-market-max-order-value | 成行最大金額
min-order-value | 最小注文金額
api-trading | api取引可否




**サンプルコード:**

```sh
curl -X GET \
      "https://api-cloud.bittrade.co.jp/v1/common/symbols"
```

レスポンスのフォーマット:

```json
{
  "status": "ok",
  "data": [
    {
      "base-currency": "btc",
      "quote-currency": "jpy",
      "price-precision": 0,
      "amount-precision": 4,
      "symbol-partition": "default",
      "symbol": "ethjpy",
      "state": "online",
      "value-precision": 0,
      "min-order-amt": 0.001,
      "max-order-amt": 10000,
      "min-order-value": 2,
      "limit-order-min-order-amt": 0.001,
      "limit-order-max-order-amt": 10000,
      "limit-order-max-buy-amt": 10000,
      "limit-order-max-sell-amt": 10000,
      "sell-market-min-order-amt": 0.001,
      "sell-market-max-order-amt": 1000,
      "buy-market-max-order-value": 10000000,
      "api-trading": "enabled",
      "tags": ""
    },
```



### 対応取引通貨


```txt
GET /v1/common/currencys
```

**Response Data**

Parameter | Description
------------ | ------------
status | リクエスト処理結果[ok/error]
data | 利用可能な通貨のリスト



**サンプルコード:**

```sh
curl -X GET \
     "https://api-cloud.bittrade.co.jp/v1/common/currencys" 
```

レスポンスのフォーマット:

```json
{
    "status": "ok",
    "data": [
        "jpy",
        "btc",
        "xrp",
        "eth",
        "ltc",
        "bch",
        "mona",
        "eos",
        "ada",
        "ht",
        "xem",
        "xlm",
        "lsk",
        "bcha",
        "etc",
        "bat",
        "ont",
        "qtum",
        "trx",
        "xym",
        "enj",
        "dot",
        "iost",
        "bsv",
        "jasmy",
        "omg",
        "cot",
        "usdt",
        "xtz",
        "dep",
        "ethf",
        "ethw",
        "plt",
        "flr",
        "astr",
        "boba",
        "atom",
        "shib",
        "doge",
        "sand",
        "axs",
        "nidt",
        "mkr",
        "usdc",
        "dai",
        "matic"
    ]
}
```

### システム時間を調べる


```txt
GET /v1/common/timestamp

```


**Response Data**

Parameter | Description
------------ | ------------
status | リクエスト処理結果[ok/error]
data | 現在時刻


**サンプルコード:**

```sh
curl -X GET \
      "https://api-cloud.bittrade.co.jp/v1/common/timestamp"
```


レスポンスのフォーマット:

```json
{
  "status": "ok",
  "data": 1555667124908
}
```

## マーケット関連



### ローソク足


```txt
GET /market/history/kline
```

**Request Params**
Params | Description
------------ | ------------
symbol | 取引ペア
period | チャートタイプ
size | サイズ[150〜2000]

**サンプルコード:**

```sh
curl -X GET \
      "https://api-cloud.bittrade.co.jp/market/history/kline?period=1day&size=2&symbol=btcjpy"
```

レスポンスのフォーマット:

```json
{
    "ch": "market.btcjpy.kline.1day",
    "status": "ok",
    "ts": 1701942774185,
    "data": [
        {
            "id": 1701878400,
            "open": 6461593,
            "close": 6327519,
            "low": 6233624,
            "high": 6519367,
            "amount": 88.15350606739558,
            "vol": 568746111.4168766,
            "count": 16964
        },
        {
            "id": 1701792000,
            "open": 6227123,
            "close": 6461593,
            "low": 6200001,
            "high": 6556474,
            "amount": 125.30771402526693,
            "vol": 808691546.00449,
            "count": 29331
        }
    ]
}
```

### ティッカー

```txt
GET /market/detail/merged

```

**Query Params**
Parameter | Description
------------ | ------------
symbol | 取引ペア


**Response Data**
Parameter | Description
------------ | ------------
status | リクエスト処理結果[ok/error]
ts | 現在時刻
ch | データの所属
tick | チャートデータ

**Tick Data**
Field | Description
------------ | ------------
status | リクエスト処理結果[ok/error]
ts | 現在時刻
id | ティッカーID
amount | 取引数量
count | 取引回数
open | 開始価格
close | 最新価格
low | 最安値
high | 最高値
vol | 取引金額
bid | 現在の最高買い価格
ask | 現在の最低売り価格

**サンプルコード:**

```sh
curl -X GET "https://api-cloud.bittrade.co.jp/market/detail/merged?symbol=btcjpy"
```

レスポンスのフォーマット:

```json
{
    "ch": "market.btcjpy.detail.merged",
    "status": "ok",
    "ts": 1701943141194,
    "tick": {
        "id": 400760536228,
        "version": 400760536228,
        "open": 6480755,
        "close": 6306170,
        "low": 6233624,
        "high": 6530650,
        "amount": 117.52586405199149,
        "vol": 759265788.8925165,
        "count": 25267,
        "bid": [
            6306169,
            0.00002
        ],
        "ask": [
            6306420,
            0.00002
        ]
    }
}
```

### 全取引ペアの相場情報


```txt
GET /market/tickers
```

**Response Data**
Parameter | Description
------------ | ------------
status | リクエスト処理結果[ok/error]
ts | 現在時刻
data | 取引相場のリスト

**サンプルコード:**

```sh
curl -X GET "https://api-cloud.bittrade.co.jp/market/tickers"
```


レスポンスのフォーマット:

```json
{
            "symbol": "btcjpy",
            "open": 6461593,
            "high": 6519367,
            "low": 6233624,
            "close": 6287241,
            "amount": 117.44015405199148,
            "vol": 758503535.8502165,
            "count": 25313,
            "bid": 6274831,
            "bidSize": 0.008,
            "ask": 6300013,
            "askSize": 0.008
        },
```

### 板情報


```txt
GET /market/depth
```

**Query Parameters**
Parameter | Description
------------ | ------------
symbol | 取引ペア
type | グルーピングレベル[step1～step5]


Group | Description
------------ | ------------
step0 | グルーピングしない
step1 | 価格精度 * 10
step2 | 価格精度 * 100
step3 | 価格精度 * 1000
step4 | 価格精度 * 10000
step5 | 価格精度 * 100000

**Response Data**
Parameter | Description
------------ | ------------
status | リクエスト処理結果[ok/error]
ts | 現在時刻
tick | ティッカー

**Tick Data**
Field | Description
------------ | ------------
bid | 現在の最高買値
ask | 現在の最低売値

**サンプルコード:**

```sh
curl -X GET "https://api-cloud.bittrade.co.jp/market/depth?symbol=btcjpy&type=step1"
```


レスポンスのフォーマット:

```json
curl -X GET "https://api-cloud.bittrade.co.jp/market/depth?symbol=btcjpy&type=step1"
```





ここまでやった20231221
-----------------------------------------------------------






### 直近の取引データ


```txt
GET /market/trade
```

**Query Parameters**
Parameter | Description
------------ | ------------
symbol | 取引ペア

**Response Data**
Parameter | Description
------------ | ------------
status | リクエスト処理結果[ok/error]
ts | 現在時刻
tick | 取引データ

**Tick Data**
Field | Description
------------ | ------------
id | ティックID
ts | 取引時間
date | 取引データ

**Data**
Field | Description
------------ | ------------
id | 個別ID
ts | 取引時間
trade-id | 取引ID
amount | 取引量
price | 価格
direction | 取引方向[by/sell]


**サンプルコード:**

```sh
curl -X GET "https://api-cloud.bittrade.co.jp/market/trade?symbol=btcjpy"
```


```json
{
    "ch": "market.btcjpy.trade.detail",
    "status": "ok",
    "ts": 1702253859051,
    "tick": {
        "id": 200392673445,
        "ts": 1702253853735,
        "data": [
            {
                "id": 2.0039267344591513e+26,
                "ts": 1702253853735,
                "trade-id": 200002419784,
                "amount": 0.009,
                "price": 6369998,
                "direction": "buy"
            }
        ]
    }
}
```

### 取引履歴の取得


```txt
GET /market/history/trade
```

**Query Parameters**
Parameter | Description
------------ | ------------
symbol | 取引ペア
size | データサイズ

**Response Data**
Parameter | Description
------------ | ------------
status | リクエスト処理結果[ok/error]
ts | 現在時刻
data | 約定履歴のリスト

**Data**
Field | Description
------------ | ------------
id | 個別ID
ts | 取引時間
trade-id | 取引ID
amount | 取引量
price | 価格
direction | 取引方向[by/sell]


**サンプルコード:**

```sh
curl -X GET \
      "https://api-cloud.bittrade.co.jp/market/history/trade?symbol=btcjpy"
```


```json
{
    "ch": "market.btcjpy.trade.detail",
    "status": "ok",
    "ts": 1702254563729,
    "data": [
        {
            "id": 200392698008,
            "ts": 1702254561129,
            "data": [
                {
                    "id": 2.003926980089151e+26,
                    "ts": 1702254561129,
                    "trade-id": 200002420002,
                    "amount": 0.009,
                    "price": 6369586,
                    "direction": "sell"
                }
            ]
        }
    ]
}
```
