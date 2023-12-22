# Private API
<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

  - [署名処理](#署名処理)
    - [アセット](#アセット)
      - [アセット一覧を返す](#アセット一覧を返す)
　- [注文情報](#注文情報)
      - [注文実行](#注文実行)
      - [未約定注文一覧](#未約定注文一覧)
      - [注文キャンセル](#注文キャンセル)
      - [注文の一括キャンセル](#注文の一括キャンセル)
      - [条件付き注文の一括キャンセル](#条件付き注文の一括キャンセル)
      - [注文の照会](#注文の照会)
    - [約定履歴](#約定履歴)
      - [約定履歴の検索](#約定履歴の検索)
    - [入出金](#入出金)
      - [入出金履歴を取得する](#入出金履歴を取得する)
      - [出金申請](#出金申請)
      - [出金のキャンセル](#出金のキャンセル)
  
<!-- END doctoc generated TOC please keep comment here to allow auto update -->


## 署名処理


- HMACを使うため、標準化しないと署名の結果が変わります
  - クエスト方法（GET 或いは POST）、続けて改行を追加する
  - 小文字のアクセスアドレスに続けて改行を追加する
  - アクセスメソッドへのパス、続けて改行を追加する
  - パラメータ名は、ASCIIコードの順にソートし、URLエンコーディングしてください
  - 上記の順序で、各パラメーターは文字 '＆'を使用して連結します
  - 署名元の文字列は右の通りに示されます
  - 秘密キー(SecretKey)と変換後のリクエスト文字列を使って、署名処理を行い、その結果はBase64エンコードします。上記の値をパラメータSignatureの値としてAPIリクエストに追加します。 URIエンコードされている必要があります
  - 最終的に、サーバーに送信するAPIリクエストは次のようになります

*※ Postのリクエストにおいて、上記の署名用のパラメータ以外のデータを渡したい場合、Json形式でRequestBodyに入れて送ってください。*

```
# 元のリクエスト
https://api-cloud.bittrade.co.jp/v1/order/orders?
AccessKeyId=e2xxxxxx-99xxxxxx-84xxxxxx-7xxxx
&order-id=1234567890
&SignatureMethod=HmacSHA256
&SignatureVersion=2
&Timestamp=2017-05-11T15:19:30

# 1. 改行入れる
GET\n

# 2. 改行入れる
api-cloud.bittrade.co.jp\n

# 3. 改行入れる
/v1/order/orders\n

# 4. パラメータソートする
AccessKeyId=e2xxxxxx-99xxxxxx-84xxxxxx-7xxxx
SignatureMethod=HmacSHA256
SignatureVersion=2
Timestamp=2017-05-11T15%3A19%3A30
order-id=1234567890

# 5. '&'で連結
AccessKeyId=e2xxxxxx-99xxxxxx-84xxxxxx-7xxxx&SignatureMethod=HmacSHA256&SignatureVersion=2&Timestamp=2017-05-11T15%3A19%3A30&order-id=1234567890

# 6. 署名用文字列
GET\n
api-cloud.bittrade.co.jp\n
/v1/order/orders\n
AccessKeyId=e2xxxxxx-99xxxxxx-84xxxxxx-7xxxx&SignatureMethod=HmacSHA256&SignatureVersion=2&Timestamp=2017-05-11T15%3A19%3A30&order-id=1234567890

# 7. 署名処理 - サンプルコードに参照
SecretKey: b0xxxxxx-c6xxxxxx-94xxxxxx-dxxxx
Signature: 4F65x5A2bLyMWVQj3Aqp+B4w+ivaA7n5Oi2SuYtCJ9o=

# 8. 署名後の文字列をURLに付加する
https://api-cloud.bittrade.co.jp/v1/order/orders?AccessKeyId=e2xxxxxx-99xxxxxx-84xxxxxx-7xxxx&order-id=1234567890&SignatureMethod=HmacSHA256&SignatureVersion=2&Timestamp=2017-05-11T15%3A19%3A30&Signature=4F65x5A2bLyMWVQj3Aqp%2BB4w%2BivaA7n5Oi2SuYtCJ9o%3D
```



### アセット
---------------------------------------------------------------------------------------------------------------
#### アセット一覧を返す

```txt
GET /v1/common/symbols
```


**Response:**

Parameter | Description
------------ | ------------
status | リクエスト処理結果[ok/error]
data | 取引ペアの情報


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

**レスポンスのフォーマット:**

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

### 注文情報
------------------------------------------------------------------------------------------------------
#### 注文実行

```txt
POST /v1/order/orders/place
```

### 注文情報
#### 注文実行


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



**サンプルコード:**

```sh
curl -X POST \
     -H "Content-Type: application/json" \
     "https://api-cloud.bittrade.co.jp/v1/order/orders/place" \
     -d \
{
   "account-id": "100009",
   "amount": "10.1",
   "price": "100.1",
   "source": "api",
   "symbol": "ethjpy",
   "type": "buy-limit"
}
```


**レスポンスのフォーマット:**

```json
{
  "status": "ok",
  "data": "59378"
}

```
-----------------------------------------------------------------------------------
#### 未約定注文一覧

```txt
GET /v1/order/openOrders
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


**サンプルコード:**


```sh
curl -X GET \
    "https://api-cloud.bittrade.co.jp/v1/order/openOrders" 
```


**レスポンスのフォーマット:**

```json

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
----------------------------------------------------------------
#### 注文キャンセル

```txt
POST /v1/order/orders/{order-id}/submitcancel
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

**サンプルコード:**

```sh
curl -X POST \
     -H "Content-Type: application/json" \
     "https://api-cloud.bittrade.co.jp/v1/order/orders/{order-id}/submitcancel"
```


**レスポンスのフォーマット:**

```json
{
  "status": "ok",
  "data": 59378
}
```

----------------------------------------------------------------
#### 注文の一括キャンセル

```txt
POST /v1/order/orders/batchcancel
```

**Parameters(requestBody):**

**Query Parameters**
Parameter | Description
------------ | ------------ 
order-ids | 注文IDのリスト

**Response Data**
Parameter | Description
------------ | ------------ 
status | リクエストの状態
data | キャンセルの結果

**サンプルコード:**

```sh
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

**レスポンスのフォーマット:**

```json
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

----------------------------------------------------------------
#### 条件付き注文の一括キャンセル

```txt
POST /v1/order/orders/batchCancelOpenOrders
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


**サンプルコード:**


```sh
curl -X POST \
     -H "Content-Type: application/json" \
     "https://api-cloud.bittrade.co.jp/v1/order/orders/batchCancelOpenOrders"
```


**レスポンスのフォーマット:**

```json
{
  "status": "ok",
  "data": {
    "success-count": 2,
    "failed-count": 0,
    "next-id": 5454600
  }
}
```

----------------------------------------------------------------
#### 注文の照会

```txt
GET /v1/order/orders/{order-id}
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



**サンプルコード:**


```sh
curl -X GET \
    "https://api-cloud.bittrade.co.jp/v1/order/orders/{order-id}" 
```


**レスポンスのフォーマット:**

```json
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

### 約定履歴

#### 約定履歴の検索

**※制限 10回/2秒間**

```txt
GET /v1/order/matchresults
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


**サンプルコード:**


```sh
curl -X GET \
  "https://api-cloud.bittrade.co.jp/v1/order/matchresults" 
```


**レスポンスのフォーマット:**

```json
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

### 入出金

#### 入出金履歴を取得する

```txt
GET /v1/query/deposit-withdraw?currency=xrp&type=deposit&from=5&size=12
```


**Query Parameters**
Parameter | Description
------------ | ------------ 
currency | 銘柄
types | 入出金種別[deposit/withdraw]
from | 照会ID
size | 記録数


**Response Data**
Parameter | Description
------------ | ------------ 
id | 入出金ID
type | 入出金種別[deposit/withdraw]
currency | 銘柄
tx-hash | トレードハッシュ
amount | 数量
address | ウォレットアドレス
address-tag | アドレスラベル
fee | 手数料
state | ステータス
created-at | 開始時間
updated-at | 最後に更新した時間





**サンプルコード:**



```sh
curl -X GET \
     "https://api-cloud.bittrade.co.jp/v1/query/deposit-withdraw" 
```


**レスポンスのフォーマット:**

```json
{
  "data":
    [
      {
        "id": 1171,
        "type": "deposit",
        "currency": "btc",
        "tx-hash": "ed03094b84eafbe4bc16e7ef766ee959885ee5bcb265872baaa9c64e1cf86c2b",
        "amount": 7.457467,
        "address": "rae93V8d2mdoUQHwBDBdM4NHCMehRJAsbm",
        "address-tag": "100040",
        "fee": 0,
        "state": "safe",
        "created-at": 1510912472199,
        "updated-at": 1511145876575
      }
    ]
}
```


#### 出金申請

```txt
POST /v1/dw/withdraw/api/create
```

**Query Parameters**
 Parameter | Description
------------ | ------------ 
address | 出金アドレス
amount | 出金数量
currency | 通貨種別
fee | 送金手数料
addr-tag | タグ

**Response Data**
Parameter | Description
------------ | ------------ 
data | 出金ID


**サンプルコード:**


```sh
curl -X POST \
     -H "Content-Type: application/json" \
     "https://api-cloud.bittrade.co.jp/v1/dw/withdraw/api/create" \
     -d \
{
  "address": "0xde709f2102306220921060314715629080e2fb77",
  "amount": "0.05",
  "currency": "btc",
  "fee": "0.0001"
}
```

**レスポンスのフォーマット:**

```json
{
  "status": "ok",
  "data": 700
}
```

#### 出金のキャンセル

```txt
POST /v1/dw/withdraw-virtual/{withdraw-id}/cancel
```

**Query Parameters**
Parameter | Description
------------ | ------------ 
withdraw-id | 出金記録ID


**Response Data**
Parameter | Description
------------ | ------------ 
data | 出金記録ID


**サンプルコード:**

```sh
curl -X POST \
     -H "Content-Type: application/json" \
     "https://api-cloud.bittrade.co.jp/v1/dw/withdraw-virtual/{withdraw-id}/cancel"
```


**レスポンスのフォーマット:**

```json
{
  "status": "ok",
  "data": 700
}
```

