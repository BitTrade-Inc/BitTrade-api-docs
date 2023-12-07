# websocket(Public)
--------------------------------------

## 購読・リクエスト

1.アドレス
・URL：wss://api-cloud.bittrade.co.jp/ws

1.データ圧縮
・WebSocket API 経由で返されるデータはすべてGZIP圧縮されており、データ受信者側のクライアントにて解凍する必要があります。

1.websocketライブラリ
・ws[https://github.com/websockets/ws]のWebSocketライブラリになります。


## ローソク足
--------------------------------------
**トピック**
```txt
market.$symbol.kline.$period
```

**購読**
```txt
{
  "sub": "market.btcjpy.kline.1min",
  "id": "id1"
}
```

key | Description
------------ | ------------
symbol | 取引ペア
period | チャートタイプ

**購読成功時のレスポンス**
```
{
  "id": "id1",
  "status": "ok",
  "subbed": "market.etcjpy.kline.1min",
  "ts": 1489474081631
}
```

**リクエスト送信**

```
{
  "req": "market.$symbol.kline.$period",
  "id": "生成ID",
  "from": "開始タイムスタンプ",
  "to": "終了タイムスタンプ"
}
```

**受信データ**

```
{
  "ch": "market.ethbtc.kline.1min",
  "ts": 1489474082831,
  "tick": {
    "id": 1489464480,
    "amount": 0.0,
    "count": 0,
    "open": 7962.62,
    "close": 7962.62,
    "low": 7962.62,
    "high": 7962.62,
    "vol": 0.0
  }
}
```

## 板情報
------------------------------------

**トピック**
```txt
market.$symbol.depth.$type
```

**購読**

```
{
  "sub": "market.ethbtc.depth.step0",
  "id": "id1"
}
```

**Parameter**

key | Description 
------------ | ------------ 
symbol | 取引ペア
type | グルーピングレベル[step0〜step5]

**リクエスト送信**

```
{
  "req": "market.btcjpy.depth.step0",
  "id": "id10"
}
```

**受信データ**

```
{
  "ch": "market.ethbtc.depth.step0",
  "ts": 1489474082831,
  "data": {
    "bids": [
      [9999.3900,0.0098],
      [9992.5947,0.0560]
    ],
    "asks": [
      [10010.9800,0.0099],
      [10011.3900,2.0000]
    ]
  }
}
```

## BBO
-----------------------------------------
板情報の中で最も近い買い(bid)、売り(ask)の指値注文です。

**トピック**
```txt
market.$symbol.bbo
```

**購読**

```
{
  "sub": "market.btcjpy.bbo",
  "id": "id1"
}
```

**購読成功のレスポンス**

```
{
  "id": "id1",
  "status": "ok",
  "subbed": "market.btcjpy.bbo",
  "ts": 1489474081631
}
```

**受信データ**

```
{
    "ch":"market.btcjpy.bbo",
    "ts":1630994555540,
    "tick":{
        "seqId":137005210233,
        "ask":52665.02,
        "askSize":1.502181,
        "bid":52665.01,
        "bidSize":0.178567,
        "quoteTime":1630994555539,
        "symbol":"btcjpy"
    }
}
```

**Data**

Field | Description 
------------ | ------------ 
seqid | シーケンスID
ask | 売り注文価格
askSize | 売り注文数量
bid | 買い注文価格
askSize | 買い注文数量
quoteTime | 更新時間
symbol | 取引ペア

## ティッカー

**トピック**

```
market.$symbol.trade.detail
```

**購読**

```
{
  "sub": "market.btcjpy.trade.detail",
  "id": "id1"
}
```

**購読成功のレスポンス**

```
{
  "id": "id1",
  "status": "ok",
  "subbed": "market.btcjpy.trade.detail",
  "ts": 1489474081631
}
```

**リクエスト送信**

```
{
  "req": "market.btcjpy.trade.detail",
  "id": "id11"
}
```

**受信データ**

```
{
    "ch":"market.btcjpy.trade.detail",
    "ts":1630994963175,
    "tick":{
        "id":137005445109,
        "ts":1630994963173,
        "data":[
            {
                "id":137005445109359286410323766,
                "ts":1630994963173,
                "tradeId":102523573486,
                "amount":0.006754,
                "price":52648.62,
                "direction":"buy"
            }
        ]
    }
}
```

**Data**

Field | Description 
------------ | ------------ 
id | 識別ID
ts | 取引時間
tradeId | 取引ID
amount | 取引量
price | 価格
direction | 取引の方向[buy/sell]

## マーケット概要
--------------------------------------------------------------
直近24時間のマーケット概要を返します。

**トピック**

```
market.$symbol.detail
```

**リクエスト送信**

```
{
  "req": "market.btcjpy.detail",
  "id": "id12"
}
```
**データ受信**

```
{
  "rep": "market.btcjpy.detail",
  "status": "ok",
  "id": "id12",
  "tick": {
    "amount": 12224.2922,
    "open":   9790.52,
    "close":  10195.00,
    "high":   10300.00,
    "ts":     1494496390000,
    "id":     1494496390,
    "count":  15195,
    "low":    9657.00,
    "vol":    121906001.754751
  }
}
```

**Ticks**

Field | Description 
------------ | ------------ 
amount | 直近24時間の取引量
open | 直近24時間の始値
close | 最新価格
high | 直近24時間の最高価格
ts | データ取得時間
count | 取引回数
low | 直近24時間の最低価格
vol | 直近24時間の取引金額

