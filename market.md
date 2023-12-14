# マーケット関連
---------------------------------------------------------------


## ローソク足

**URI**

GET /market/history/kline

```
curl -X GET \
      "https://api-cloud.bittrade.co.jp/market/history/kline?period=1day&size=2&symbol=btcjpy"
```

上記のコマンドは、次のような構造のJSONを返します。

```
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
**Request Params**
Params | Description
------------ | ------------
symbol | 取引ペア
period | チャートタイプ
size | サイズ[150〜2000]

## ティッカー
--------------------------------------------------------------------------------------------
このエンドポイントは直近24時間のティッカー情報を返します。

**HTTP Request**

GET /market/detail/merged

```
curl -X GET "https://api-cloud.bittrade.co.jp/market/detail/merged?symbol=btcjpy"
```

上記のコマンドは、次のような構造のJSONを返します。

```
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

## 全取引ペアの相場情報
----------------------------------------------------------------
このエンドポイントはすべての取引ペアの最新の取引相場を取得できます。

**HTTP Request**

GET /market/tickers

```
curl -X GET "https://api-cloud.bittrade.co.jp/market/tickers"
```

上記のコマンドは、次のような構造のJSONを返します。

```
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

**Response Data**
Parameter | Description
------------ | ------------
status | リクエスト処理結果[ok/error]
ts | 現在時刻
data |　取引相場のリスト


## 板情報
------------------------------------------------------------
このエンドポイントは板情報を返します。

**HTTP Request**

```
GET /market/depth
```

```
curl -X GET "https://api-cloud.bittrade.co.jp/market/depth?symbol=btcjpy&type=step1"
```

上記のコマンドは、次のような構造のJSONを返します。

```
{
    "ch": "market.btcjpy.depth.step1",
    "status": "ok",
    "ts": 1702253054148,
    "tick": {
        "bids": [ [
                6303810,
                0.04
            ]
        ],
        "asks": [ [
                6368050,
                0.00002
            ],  ],
        "version": 200392644603,
        "ts": 1702253053014
    }
}

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


## 直近の取引データ
---------------------------------------------------
このエンドポイントは直近の取引履歴を返します。

**HTTP Request**
```
GET /market/trade
```

```
curl -X GET "https://api-cloud.bittrade.co.jp/market/trade?symbol=btcjpy"
```

上記のコマンドは、次のような構造のJSONを返します。

```
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


## 取引履歴の取得
-------------------------------------
このエンドポイントは集約されたチャート情報を返します。

**HTTP Request**

```
GET /market/history/trade
```

```
curl -X GET \
      "https://api-cloud.bittrade.co.jp/market/history/trade?symbol=btcjpy"
```

上記のコマンドは、次のような構造のJSONを返します。

```
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
