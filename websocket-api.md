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
**トピック:**
```
market.$symbol.kline.$period
```

key | Description
------------ | ------------
symbol | 取引ペア
period | チャートタイプ
