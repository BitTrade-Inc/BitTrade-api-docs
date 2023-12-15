# 取引関連
---------------------------------------------------------------

## 共通定義
--------------------------------------------------------

**Order Type**
 Order | Description
------------ | ------------ 
buy-market | 成行買い 
sell-market | 成行売り
buy-limit | 指値買い
sell-limit | 指値売り
buy-ioc | IOC買い
sell-ioc | IOC売り
buy-limit-maker | Post only(買い)
sell-limit-maker | Post only(売り)

**Status**
Status | Description
------------ | ------------ 
created | 注文作成 
submitted | 発注
partial-filled | 部分約定
partial-canceled | 部分キャンセル
filled | 全部約定
canceled | キャンセル
canceling | キャンセル中

## 注文実行
------------------------------------------------------------
このエンドポイントは注文を出します。

**HTTP Request**

```
POST /v1/order/orders/place
```

```
curl -X POST \
     -H "Content-Type: application/json" \
     "https://api-cloud.bittrade.co.jp/v1/order/orders/place" \
     -d \
{
   "account-id": "100009",
   "amount": "10.1",
   "price": "100.1",
   "source": "api",
   "symbol": "btcjpy",
   "type": "buy-limit"
}
```

上記のコマンドは次のような構造のJSONを返します。

```
{
  "status": "ok",
  "data": "59378"
}
```


**Query Parameters**
Parameter | Description
------------ | ------------ 
account-id | アカウントID
amount | 取引数量
price | 指値の注文価格
sauce | 注文方法
symbol | 通貨ペア
type | 注文タイプ

**※buy-limit-maker**

“注文価格”>=“市場最低売り価格”である場合、注文は約定されません。

“注文価格”<“市場最低売り価格”である場合のみ注文が成立します。

**※sell-limit-maker**

“注文価格”<=“市場最高買い入れ価格” である場合、注文は約定しません。

“注文価格”>“市場最高買い入れ価格” である場合のみ注文が成立します。


## 未約定注文一覧
--------------------------------------------------------------------------------
このエンドポイントは未約定注文一覧を返します。

**HTTP Request**

```
GET /v1/order/openOrders
```

```
curl -X GET \
    "https://api-cloud.bittrade.co.jp/v1/order/openOrders" 
```

上記のコマンドは、次のような構造のJSONを返します。

```
{
  "data": [
    {
      "account-id": 12698099,
      "amount": "2.000000000000000000",
      "client-order-id": "",
      "created-at": 1633016858283,
      "filled-amount": "0.0",
      "filled-cash-amount": "0.0",
      "filled-fees": "0.0",
      "id": 375977156321165,
      "price": "6000000",
      "source": "api",
      "state": "submitted",
      "symbol": "btcjpy",
      "type": "buy-limit"
    }
  ],
  "status": "ok"
}
```

**Query Parameters**
Parameter | Description
------------ | ------------ 
account-id | アカウントID
取引ペア | 通貨ペア
side | 取引方向[buy/sell]
size | 必要な記録数

**Response Data**
Parameter | Description
------------ | ------------ 
id | 注文ID
symbol | 通貨ペア
price | 注文価格
created-at | 注文時間
type | 注文タイプ
filled-amount | 約定数量
filled-cash-amount | 約定された部分の約定価格
filled-fees | 約定された部分の手数料
source | 注文方法[sys/web/api/app]
state | 注文ステータス

## 注文キャンセル
--------------------------------------------------------------------
このエンドポイントはキャンセルリクエストを出します。

**HTTP Request**

```
POST /v1/order/orders/{order-id}/submitcancel
```

```
curl -X POST \
     -H "Content-Type: application/json" \
     "https://api-cloud.bittrade.co.jp/v1/order/orders/{order-id}/submitcancel"
```

上記のコマンドは、次のような構造のJSONを返します。

```
{
  "status": "ok",
  "data": 59378
}
```

**Query Parameters**
Parameter | Description
------------ | ------------ 
order-id | 注文ID


**Response Data**
Parameter | Description
------------ | ------------ 
status | リクエストの状態
data | 注文ID


## 注文の一括キャンセル
-----------------------------------------------------------------
このエンドポイントは注文の一括キャンセルを実行します。

**HTTP Request**

```
POST /v1/order/orders/batchcancel
```

```
curl -X POST \
    -H "Content-Type: application/json" \
    "https://api-cloud.bittrade.co.jp/v1/order/orders/batchcancel" \
    -d \
{
  "order-ids": [
    "1",
    "2",
    "3"
  ]
}
```

上記のコマンドは、次のような構造のJSONを返します。

```
{
  "status": "ok",
  "data": {
    "success": [
      "1",
      "2",
      "3"
    ]
  }
}
```

失敗する場合、下記のようなレスポンスが返します。

```
{
    "status": "error",
    "err-code": "order-orderstate-error",
    "err-msg": "Incorrect order state",
    "data": null,
    "order-state": 7 
}
```

**Query Parameters**
Parameter | Description
------------ | ------------ 
order-ids | 注文IDのリスト

**Response Data**
Parameter | Description
------------ | ------------ 
status | リクエストの状態
data | キャンセルの結果

**order-state**
order-state | Description
------------ | ------------ 
-1 | キャンセル済み
1 | 
3 | 
4 | 一部約定済み
5 | 一部キャンセル
6 | 約定済み
7 | キャンセル済み
10 | キャンセル中


## 条件付き注文の一括キャンセル
--------------------------------------------------------------
このエンドポイントは条件付き注文を一括でキャンセルします。

**HTTP Request**

```
POST /v1/order/orders/batchCancelOpenOrders
```

```
curl -X POST \
     -H "Content-Type: application/json" \
     "https://api-cloud.bittrade.co.jp/v1/order/orders/batchCancelOpenOrders"
```

上記のコマンドは、次のような構造のJSONを返します。

```
{
  "status": "ok",
  "data": {
    "success-count": 2,
    "failed-count": 0,
    "next-id": 5454600
  }
}
```

**Query Parameters**
Parameter | Description
------------ | ------------ 
account-id | 	アカウントID
symbol | 通貨ペア
side | 取引方向[buy/sell]
size | 必要な記録数
types | 注文タイプの組み合わせ

**Response Data**
Parameter | Description
------------ | ------------ 
success-count | 	キャンセル注文成功数
failed-count | キャンセル注文失敗数
next-id | キャンセル基準を満たす取引ID

## 注文の紹介
----------------------------------------------------
このエンドポイントは注文の詳細情報を返します。

**HTTP Request**

```
GET /v1/order/orders/{order-id}
```

```
curl -X GET \
    "https://api-cloud.bittrade.co.jp/v1/order/orders/{order-id}" 
```

上記のコマンドは、次のような構造のJSONを返します。

```
{
  "data": {
    "account-id": 12698099,
    "amount": "2.000000000000000000",
    "canceled-at": 0,
    "client-order-id": "",
    "created-at": 1633016858283,
    "field-amount": "0.0",
    "field-cash-amount": "0.0",
    "field-fees": "0.0",
    "finished-at": 0,
    "id": 375977156321165,
    "price": "6000000",
    "source": "api",
    "state": "submitted",
    "symbol": "btcjpy",
    "type": "buy-limit"
  },
  "status": "ok"
}

```

**Query Parameters**
Parameter | Description
------------ | ------------ 
order-id | 	注文ID

**Response Data**
Parameter | Description
------------ | ------------ 
account-id | アカウント ID
amount | 注文数量
canceled-at | キャンセル時間
created-at | 注文作成時間
field-amount | 約定数量
field-cash-amount | 約定総額
field-fees | 約定手数料
finished-at | 約定完了時間
id | 	注文ID
client-order-id | ユーザ設定の注文ID
price | 注文価格
source | 注文方法
state | 注文ステータス
symbol | 通貨ペア
type | 注文タイプ


## 注文の約定詳細
--------------------------------------------------------------
このエンドポイントは注文の約定詳細を返します。

**HTTP Request**

```
GET /v1/order/orders/{order-id}/matchresults
```

```
curl -X GET \
  "https://api-cloud.bittrade.co.jp/v1/order/orders/{order-id}/matchresults" 
```

上記のコマンドは、次のような構造のJSONを返します。

```
{
  "data": [
    {
      "created-at": 1633018402536,
      "fee-currency": "btc",
      "fee-deduct-currency": "",
      "fee-deduct-state": "done",
      "filled-amount": "0.1",
      "filled-fees": "0",
      "filled-points": "0.0",
      "id": 372969482257005,
      "match-id": 100554712526,
      "order-id": 375977348044411,
      "price": "6000000",
      "role": "taker",
      "source": "spot-api",
      "symbol": "btcjpy",
      "trade-id": 100026336962,
      "type": "buy-limit"
    }
  ],
  "status": "ok"
}

```

**Query Parameters**
Parameter | Description
------------ | ------------ 
order-id | 	注文ID


**Response Data**
Parameter | Description
------------ | ------------ 
account-id | アカウント ID
amount | 注文数量
canceled-at | キャンセル受付委時間
created-at | 注文作成時間
field-amount | 約定数量
field-cash-amount | 約定総額
field-fees | 約定手数料
finished-at | 約定時間
id | 	注文ID
price | 注文価格
source | 注文方法
state | 注文ステータス
symbol | 通貨ペア
type | 注文タイプ


## 注文履歴の検索
---------------------------------------------------------
このエンドポイントは条件に合った注文履歴を返します。

**HTTP Request**

```
GET /v1/order/orders
```

```
curl -X GET \
  "https://api-cloud.bittrade.co.jp/v1/order/orders" 
```

上記のコマンドは、次のような構造のJSONを返します。

```
{
    "status": "ok",
    "data": [
        {
            "id": 345487249132375,
            "symbol": "btcjpy",
            "account-id": 13496526,
            "client-order-id": "",
            "amount": "50.000000000000000000",
            "price": "0.0",
            "created-at": 1629443051822,
            "type": "buy-market",
            "field-amount": "147.928994082840236000",
            "field-cash-amount": "49.999999999999999768",
            "field-fees": "0.295857988165680472",
            "finished-at": 1629443051838,
            "source": "spot-web",
            "state": "filled",
            "canceled-at": 0
        }
    ]
}
```

**Query Parameters**
Parameter | Description
------------ | ------------ 
symbol | 通貨ペア
types | 注文タイプの照会
start-time | 照会開始時間(最大過去180日 キャンセル注文は2時間)
end-time | 照会終了時間(最大過去180日 キャンセル注文は2時間)
states | 注文状況の照会
from | 注文IDの照会
direct | 約定IDの照会
size | 記録数(最大100)

**Response Data**
Parameter | Description
------------ | ------------ 
created-at | 約定時間
filled-amount | 約定数量
filled-fees | 約定手数料
id | 取引ID
match-id | マッチングID
order-id | 注文ID
price | 約定価格
source | 注文方法
symbol | 通貨ペア
type | 注文タイプ

## 約定履歴の検索
-------------------------------------------------------------------
このエンドポイントは検索条件に合った約定履歴を返します。

※制限 10回/2秒間

**HTTP Request**

```
GET /v1/order/matchresults
```

```
curl -X GET \
  "https://api-cloud.bittrade.co.jp/v1/order/matchresults" 
```

上記のコマンドは、次のような構造のJSONを返します。

```
{
  "data": [
    {
      "created-at": 1633018402536,
      "fee-currency": "btc",
      "fee-deduct-currency": "",
      "fee-deduct-state": "done",
      "filled-amount": "0.1",
      "filled-fees": "0",
      "filled-points": "0.0",
      "id": 372969482257005,
      "match-id": 100554712526,
      "order-id": 375977348044411,
      "price": "6000000",
      "role": "taker",
      "source": "spot-api",
      "symbol": "btcjpy",
      "trade-id": 100026336962,
      "type": "buy-limit"
    }
  ],
  "status": "ok"
}
```

**Query Parameters**
Parameter | Description
------------ | ------------ 
symbol | 通貨ペア
types | 注文タイプの照会
start-time | 照会開始時間(最大過去180日 キャンセル注文は2時間)
end-time | 照会終了時間(最大過去180日 キャンセル注文は2時間)
states | 注文状況の照会
from | 注文IDの照会
direct | 約定IDの照会
size | 記録数(最大100)

**Response Data**
Parameter | Description
------------ | ------------ 
account-id | アカウント ID
amount | 注文数量
canceled-at | キャンセル時間
created-at | 注文作成時間
field-amount | 約定数量
field-cash-amount | 約定総額
field-fees | 約定手数料
finished-at | 約定時間
id | 	注文ID
price | 注文価格
source | 注文方法
state | 注文ステータス
symbol | 通貨ペア
type | 注文タイプ
