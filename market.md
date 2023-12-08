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


