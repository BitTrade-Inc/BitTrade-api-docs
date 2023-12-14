# システム情報関連
----------------------------------------------------------------
## 取引ペア情報

このエンドポイントは各取引ペアの精度を返します。

**HTTP Request**

GET /v1/common/symbols

```
curl -X GET \
      "https://api-cloud.bittrade.co.jp/v1/common/symbols"
```

上記のコマンドは次のような構造のJSONを返します。
```
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

## 対応取引通貨
--------------------------------------------
このエンドポイントは取引できる通貨を返します。

**HTTP Request**

GET /v1/common/currencys

```
curl -X GET \
     "https://api-cloud.bittrade.co.jp/v1/common/currencys" 
```

上記のコマンドは、次のような構造のJSONを返します。

```
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

**Response Data**

Parameter | Description
------------ | ------------
status | リクエスト処理結果[ok/error]
data | 利用可能な通貨のリスト

## システム時間を調べる
-----------------------------------------------------
このエンドポイントはシステムのUnix Timestamp(ミリ秒)を返します。

**HTTP Request**

GET /v1/common/timestamp

```
curl -X GET \
      "https://api-cloud.bittrade.co.jp/v1/common/timestamp"
```

上記のコマンドは、次のような構造のJSONを返します。

```
{
  "status": "ok",
  "data": 1555667124908
}
```

**Response Data**

Parameter | Description
------------ | ------------
status | リクエスト処理結果[ok/error]
data | 現在時刻

