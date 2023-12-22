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
```sh
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
```json
{
  "id": "id1",
  "status": "ok",
  "subbed": "market.btcjpy.kline.1min",
  "ts": 1489474081631
}
```

**リクエスト送信**

```json
{
  "req": "market.$symbol.kline.$period",
  "id": "生成ID",
  "from": "開始タイムスタンプ",
  "to": "終了タイムスタンプ"
}
```

**受信データ**

```json
{
  "ch": "market.btcjpy.kline.1min",
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

```sh
{
  "sub": "market.btcjpy.depth.step0",
  "id": "id1"
}
```

**Parameter**

key | Description 
------------ | ------------ 
symbol | 取引ペア
type | グルーピングレベル[step0〜step5]

**リクエスト送信**

```json
{
  "req": "market.btcjpy.depth.step0",
  "id": "id10"
}
```

**受信データ**

```json
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

```sh
{
  "sub": "market.btcjpy.bbo",
  "id": "id1"
}
```

**購読成功のレスポンス**

```json
{
  "id": "id1",
  "status": "ok",
  "subbed": "market.btcjpy.bbo",
  "ts": 1489474081631
}
```

**受信データ**

```json
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

```txt
market.$symbol.trade.detail
```

**購読**

```sh
{
  "sub": "market.btcjpy.trade.detail",
  "id": "id1"
}
```

**購読成功のレスポンス**

```json
{
  "id": "id1",
  "status": "ok",
  "subbed": "market.btcjpy.trade.detail",
  "ts": 1489474081631
}
```

**リクエスト送信**

```json
{
  "req": "market.btcjpy.trade.detail",
  "id": "id11"
}
```

**受信データ**

```json
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

```txt
market.$symbol.detail
```

**リクエスト送信**

```sh
{
  "req": "market.btcjpy.detail",
  "id": "id12"
}
```
**データ受信**

```json
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


# websocket(Private)
--------------------------------------
APIキーの署名により口座の取引情報、資産情報などのデータが取得できます。

**エントリポイント**
```txt
wss://api-cloud.bittrade.co.jp/ws/v2
```

**制限**

・接続及びリクエストを1秒あたり50回に制限されます。この制限を超えると、「リクエストが多すぎます」というエラーメッセージが返されます。

・APIキー毎に、最大１０回接続できます。この制限を超えると、「接続が多すぎます」というエラーメッセージが返されます。

・IP毎に確立される接続数が1秒あたり100回に制限されます。この制限を超えると、「リクエストが多すぎます」というエラーメッセージが返されます。

**認証ロジック**

接続確立された後、署名情報をサーバーに送らなければなりません。認証が通ると、顧客のアカウント・取引データがwebsocket経由で送信されます。


**手順**

websocketの署名は、RestAPIの署名と似ていますが、いくつかの違いがあります。

Field | value | Description
------------ | ------------ | ------------
path | /ws/v2 | 固定値
action | req | 固定値
signatureVersion | 2.1 | 固定値
ch | auth | 固定値
signatureVersion | HmacSHA256 | 暗号化メゾット

※JSONのURLのエンコーディングが不要

**署名用リクエスト**

```sh
{
    "action": "req", 
    "ch": "auth",
    "params": { 
        "authType":"api",
        "accessKey": "e2xxxxxx-99xxxxxx-84xxxxxx-7xxxx",
        "signatureMethod": "HmacSHA256",
        "signatureVersion": "2.1",
        "timestamp": "2019-09-01T18:16:16",
        "signature": "4F65x5A2bLyMWVQj3Aqp+B4w+ivaA7n5Oi2SuYtCJ9o="
    }
}
```

**レスポンス**
```json
{
    "action": "req",
    "code": 200,
    "ch": "auth",
    "data": {}
}
```

**署名のサンプルコード**

authの結果をシリアライズしてサーバーに送る

```sh
func hmac256(base string, key string) string {
  h := hmac.New(sha256.New, []byte(key))
    h.Write([]byte(base))
    return base64.StdEncoding.EncodeToString(h.Sum(nil))
}

func createAuthJson(accessKey, secretKey string) authJson {
    authParams := url.Values{}
    utc := time.Now().UTC().Format("2006-01-02T15:04:05")
    authParams.Set("accessKey", accessKey)
    authParams.Set("signatureMethod", "HmacSHA256")
    authParams.Set("signatureVersion", "2.1")
    authParams.Set("timestamp", utc)
    host := "api-cloud.bittrade.co.jp"
    path := "/ws/v2"
    s := fmt.Sprintf("GET\n%s\n%s\n%s", host, path, authParams.Encode())
    signature := hmac256(s, secretKey)
    auth := authJson{
        Action: "req",
        Ch:     "auth",
        Params: {
            AuthType:         "api",
            AccessKey:        accessKey,
            SignatureMethod:  "HmacSHA256",
            SignatureVersion: "2.1",
            Timestamp:        utc,
            Signature:        signature,
        },
    }
    return auth
}
```

# 注文データ
-------------------------------------------

**トピック**
```txt
orders#${symbol}
```

**購読**

```sh
{
    "action": "sub",
    "ch": "orders#btcjpy"
}
```

**Parameter**

Parameter | Description
------------ | ------------ 
symbol | 取引ペア

**購読成功のレスポンス**

```json
{
    "action": "sub",
    "code": 200,
    "ch": "orders#btcjpy",
    "data": {}
}
```

**受信データ**

```json
{
    "action":"push",
    "ch":"orders#btcjpy",
    "data":
    {
        "orderSize":"2.000000000000000000",
        "orderCreateTime":1583853365586,
        "accountId":992701,
        "orderPrice":"77.000000000000000000",
        "type":"sell-limit",
        "orderId":27163533,
        "clientOrderId":"abc123",
        "orderSource":"spot-api",
        "orderStatus":"submitted",
        "symbol":"btcjpy",
        "eventType":"creation"
    }
}
```

**注文作成**

```json
{
    "action":"push",
    "ch":"orders#btcjpy",
    "data":
    {
        "orderSize":"2.000000000000000000",
        "orderCreateTime":1583853365586,
        "accountId":992701,
        "orderPrice":"77.000000000000000000",
        "type":"sell-limit",
        "orderId":27163533,
        "clientOrderId":"abc123",
        "orderSource":"spot-api",
        "orderStatus":"submitted",
        "symbol":"btcjpy",
        "eventType":"creation"
    }
}
```

**注文トリガー失敗時**

Field | Description
------------ | ------------ 
eventType | 操作種類[trigger]
symbol | 取引ペア
clientOrderid | 注文ID
orderSide | 注文方向[buv/sell]
orderStatus | 注文状況[rejected]
errorCode | 注文トリガーエラーコード
errorMessage | エラーメッセージ
lastActTime | 注文トリガー失敗時間

**キャンセル時**

Field | Description
------------ | ------------ 
eventType | 操作種類[delete]
symbol | 取引ペア
clientOrderid | 注文ID
orderSide | 注文方向[buv/sell]
orderStatus | 注文状況[canceled]
lastActTime | 注文キャンセル時間

**注文レコードの作成**

Field | Description
------------ | ------------ 
eventType | 操作種類[creation]
symbol | 取引ペア
accountid | アカウントID
orderid | 注文ID
clientOrderid | 
orderSauce | 注文元
orderPrice | 注文価格
orderSize | 注文数量
orderValue | 注文金額
type | 注文タイプ[buy-market, sell-market, buy-limit, sell-limit, buy-limit-maker, sell-limit-maker, buy-ioc, sell-ioc, buy-limit-fok, sell-limit-fok]
orderStatus | 注文状況[submitted]
orderCreateTime | 注文作成時間


## 注文状態更新
------------------------------------------------------
注文が約定またはキャンセルされたときに通知されます。

※受信の順序が保証されません。例えば、IOC注文の場合、成約後、残りの部分はキャンセルされます。キャンセルの通知が先に受信される場合もあります。

※順序が依存する場合は、注文データ(orders#${symobl})のトピックを使ってください。


**トピック**

```txt
trade.clearing#${symbol}#${mode}
```

**Parameter**

Parameter | Description
------------ | ------------ 
symbol | 取引ペア
mode | プッシュモード[0-成約時のみ通知/1-成約時とキャンセル時通知]


**購読**

```sh
{
    "action": "sub",
    "ch": "trade.clearing#btcjpy#1"
}
```

**購読成功のレスポンス**

```json
{
    "action": "sub",
    "code": 200,
    "ch": "trade.clearing#btcjpy#1",
    "data": {}
}
````

**受信データ**

```josn
{
    "ch": "trade.clearing#trxjpy#1",
    "data": {
          "accountId": 5566913,
          "aggressor": true,
          "eventType": "trade",
          "feeCurrency": "btc",
          "feeDeduct": "0",
          "feeDeductType": "",
          "orderCreateTime": 1634275849427,
          "orderId": 388370646839430,
          "orderPrice": "6000000",
          "orderSide": "buy",
          "orderSize": "1",
          "orderStatus": "filled",
          "orderType": "buy-limit",
          "source": "web",
          "symbol": "btcjpy",
          "tradeId": 2516,
          "tradePrice": "6000000",
          "tradeTime": 1634275849431,
          "tradeVolume": "1",
          "transactFee": "0"
    }
}
```

**約定時のデータ**

Parameter | Description
------------ | ------------ 
eventType | 操作種類
symbol | 取引ペア
orderid | 注文ID
tradePrice | 約定価格
tradeVolume | 約定数量
orderSide | 取引方向[buy/sell]
orderType | 注文タイプ
tradeid | 取引ID
tradeTime | 取引時間
tradeFee | 取引手数料
feeCurrency | 手数料の支払い通貨
accountid | アカウントID
sauce | 注文元
orderPrice | 注文価格
orderSize | 注文数量
orderValue | 注文金額
clientOrderid | 注文番号
orderCreateTime | 注文時間
orderStatus | 注文状況[filled/partial-filled]

**キャンセル時のデータ**

Parameter | Description
------------ | ------------ 
remainAmt | 未約定数量


## 資産変動
----------------------------------------------------------

**トピック**
```txt
accounts.update#{mode}
```

**mode**

mode | Description
------------ | ------------ 
0 | 残高が変動する時のみ通知されます
1 | 利用可能残高に変動があった際、別々のデータが受信できます
2 | 利用可能残高に変更があった際、同じデータが受信できます

**購読**

```sh
{
    "action": "sub",
    "ch": "accounts.update"
}
```

**購読成功時のレスポンス**

```json
{
    "action": "sub",
    "code": 200,
    "ch": "accounts.update#0",
    "data": {}
}
```

**受信データ(accounts.update#0)**

```json
{
    "action": "push",
    "ch": "accounts.update#0",
    "data": {
          "accountId": 5566913,
          "accountType": "trade",
          "available": "0.000051644983883289",
          "balance": "0.000051644983883289",
          "changeTime": null,
          "changeType": null,
          "currency": "btc",
          "seqNum": 566163
    }
}
```

**受信データ(accounts.update#1)**

```josn
{
    "action": "push",
    "ch": "accounts.update#1",
    "data": {
          "accountId": 5566913,
          "accountType": "trade",
          "available": "0.001998038941797817",
          "balance": "0.001998038941797817",
          "changeTime": null,
          "changeType": null,
          "currency": "btc",
          "seqNum": 83
    }
}
```

Field | Description
------------ | ------------ 
currency | 通貨
accountid | アカウントID
balance | 残高
available | 利用可能残高
changeType | 変更タイプ[order-place(注文作成)，order-match(注文成約)，order-refund(注文取引の払い戻し)，order-cancel(注文のキャンセル), deposit (入金）, withdraw (出金)，other(その他の資産の変更)]
accountType | アカウントタイプ[trade, loan, interest]
changeTime | 変更時間
seqNum | アカウント変更のシリアル番号



