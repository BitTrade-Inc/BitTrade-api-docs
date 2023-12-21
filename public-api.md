# Public API
-----------------------------------------------
公開されている取引所の注文状況や取引履歴、注文板を確認することができます。


---------------------------------------------
## ティッカー


各取引ペアのティッカー情報を取得することができます。

**HTTP REQUEST**

```txt
GET /market/detail/merged
```
```txt
curl -X GET "https://api-cloud.bittrade.co.jp/market/detail/merged?symbol=btcjpy"
```

**Parameters**

Parameter | Required | Description
------------ | ------------ | ------------
symbol | true | 取引ペア: [ペア一覧](assets.md)

```
{
    "ch": "market.btcjpy.detail.merged",
    "status": "ok",
    "ts": 1701829427499,
    "tick": {
        "id": 400751827463,
        "version": 400751827463,
        "open": 6153770,
        "close": 6461462,
        "low": 6103231,
        "high": 6556474,
        "amount": 122.17410241972473,
        "vol": 768756402.9727342,
        "count": 25178,
        "bid": [
            6461461,
            0.00002
        ],
        "ask": [
            6461647,
            0.00002
        ]
    }
}
```

**Response**

Parameter | Description
------------ | ------------
status |リクエスト処理結果
ts | 現在の時刻
open | 24時間前の始値
close | 最新取引価格
low | 過去24時間の最安値取引価格
high | 過去24時間の最高値取引価格
amount | 過去24時間の取引量
vol | 過去24時間の取引金額
bid | 現在の最高売値
ask | 現在の最低買値

---------------------------------------------
## 取引履歴


最新の取引履歴を取得できます。

**HTTP REQUEST**

```txt
GET /market/trade
```
```txt
curl -X GET "https://api-cloud.bittrade.co.jp/market/trade?symbol=btcjpy"
```

**Parameters**

Parameter | Required | Description
------------ | ------------ | ------------
symbol | true | 取引ペア: [ペア一覧](assets.md)

```
{
    "ch": "market.btcjpy.trade.detail",
    "status": "ok",
    "ts": 1701830677712,
    "tick": {
        "id": 200377643658,
        "ts": 1701830673572,
        "data": [
            {
                "id": 2.0037764365891505e+26,
                "ts": 1701830673572,
                "trade-id": 200002304175,
                "amount": 0.008,
                "price": 6468820,
                "direction": "buy"
            }
        ]
    }
}
```

**Response**

Parameter | Description
------------ | ------------
status | リクエスト処理結果
ts | 現在の時刻
open | 24時間前の始値
close | 最新取引価格
low | 過去24時間の最安値取引価格
high | 過去24時間の最高値取引価格
amount | 過去24時間の取引量
vol | 過去24時間の取引金額
bid | 現在の最高売値
ask | 現在の最低買値

---------------------------------------------
## 板情報


板情報を取得できます。

**HTTP REQUEST**

```txt
GET /market/depth
```
```txt
curl -X GET "https://api-cloud.bittrade.co.jp/market/depth?symbol=btcjpy&type=step1"
```

**Parameters**

Parameter | Required | Description
------------ | ------------ | ------------
symbol | true | 取引ペア: [ペア一覧](pairs.md)
Type | true | グルーピングレベル[step0〜step5]


```
 "ch": "market.btcjpy.depth.step1",
    "status": "ok",
    "ts": 1701831020445,
    "tick": {
        "bids": [
                6416340,
                0.04
            ],
            [
                6400000,
                1.79037
            ]
        ],
        "asks": [
            [
                6477710,
                0.00002
            ],
            [
                6477880,
                0.00001
            ],
```

**Response**

Parameter | Description
------------ | ------------
asks | 売り注文の情報
bids | 買い注文の情報


---------------------------------------------
## 取引ペアの相場情報


取引ペアの相場情報を取得できます。

```txt
GET /market/tickers
```
```txt
curl -X GET "https://api-cloud.bittrade.co.jp/market/tickers"
```
```
  {
            "symbol": "btcjpy",
            "open": 6227123,
            "high": 6556474,
            "low": 6200001,
            "close": 6488192,
            "amount": 122.54635504247356,
            "vol": 772135204.1874942,
            "count": 25379,
            "bid": 6483525,
            "bidSize": 0.00001,
            "ask": 6488452,
            "askSize": 0.04
        },
```

---------------------------------------------
## ローソク足


ローソク足のデータを取得

```txt
GET /market/history/kline
```
```txt
https://api-cloud.bittrade.co.jp/market/history/kline?period=1day&size=2&symbol=btcjpy
```

**Parameters**

Parameter | Description
------------ | ------------ | ------------
symbol | 取引ペア: [ペア一覧](assets.md)
period | チャートタイプ
size | チャートサイズ[150〜2,000]

```
{
    "ch": "market.btcjpy.kline.1day",
    "status": "ok",
    "ts": 1701916672242,
    "data": [
        {
            "id": 1701878400,
            "open": 6461593,
            "close": 6470269,
            "low": 6433803,
            "high": 6519367,
            "amount": 50.64285219438104,
            "vol": 327991371.12802655,
            "count": 11008
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

**Response**

Field | Description
------------ | ------------
status | リクエスト処理結果
ts | 現在の時刻
tick | ローソク足データ

