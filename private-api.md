# Private API
<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

  - [署名処理](#%E8%AA%8D%E8%A8%BC)
  - [エンドポイント一覧](#%E3%82%A8%E3%83%B3%E3%83%89%E3%83%9D%E3%82%A4%E3%83%B3%E3%83%88%E4%B8%80%E8%A6%A7)
    - [アセット](#%E3%82%A2%E3%82%BB%E3%83%83%E3%83%88)
      - [アセット一覧を返す](#%E3%82%A2%E3%82%BB%E3%83%83%E3%83%88%E4%B8%80%E8%A6%A7%E3%82%92%E8%BF%94%E3%81%99)
    - [注文情報](#%E6%B3%A8%E6%96%87%E6%83%85%E5%A0%B1)
      - [注文実行](#%E6%B3%A8%E6%96%87%E6%83%85%E5%A0%B1%E3%82%92%E5%8F%96%E5%BE%97%E3%81%99%E3%82%8B)
      - [未約定注文一覧](#%E6%96%B0%E8%A6%8F%E6%B3%A8%E6%96%87%E3%82%92%E8%A1%8C%E3%81%86)
      - [注文キャンセル](#%E6%B3%A8%E6%96%87%E3%82%92%E3%82%AD%E3%83%A3%E3%83%B3%E3%82%BB%E3%83%AB%E3%81%99%E3%82%8B)
      - [注文の一括キャンセル](#%E6%B3%A8%E6%96%87%E3%82%92%E3%82%AD%E3%83%A3%E3%83%B3%E3%82%BB%E3%83%AB%E3%81%99%E3%82%8B%E8%A4%87%E6%95%B0)
      - [条件付き注文の一括キャンセル](#%E6%B3%A8%E6%96%87%E6%83%85%E5%A0%B1%E3%82%92%E5%8F%96%E5%BE%97%E3%81%99%E3%82%8B%E8%A4%87%E6%95%B0)
      - [注文の照会](#%E3%82%A2%E3%82%AF%E3%83%86%E3%82%A3%E3%83%96%E3%81%AA%E6%B3%A8%E6%96%87%E3%82%92%E5%8F%96%E5%BE%97%E3%81%99%E3%82%8B)
    - [約定履歴](#%E7%B4%84%E5%AE%9A%E5%B1%A5%E6%AD%B4)
      - [約定履歴の検索](#%E7%B4%84%E5%AE%9A%E5%B1%A5%E6%AD%B4%E3%82%92%E5%8F%96%E5%BE%97%E3%81%99%E3%82%8B)
    - [入出金](#%E5%85%A5%E9%87%91)
      - [入出金履歴を取得する](#%E5%85%A5%E9%87%91%E5%B1%A5%E6%AD%B4%E3%82%92%E5%8F%96%E5%BE%97%E3%81%99%E3%82%8B)
      - [出金申請](#%E5%87%BA%E9%87%91%E3%82%A2%E3%82%AB%E3%82%A6%E3%83%B3%E3%83%88%E3%82%92%E5%8F%96%E5%BE%97%E3%81%99%E3%82%8B)
      - [出金のキャンセル](#%E5%87%BA%E9%87%91%E3%83%AA%E3%82%AF%E3%82%A8%E3%82%B9%E3%83%88%E3%82%92%E8%A1%8C%E3%81%86)
  
<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Private API


## 署名処理


- HMACを使うため、標準化しないと署名の結果が変わります
  - ACCESS-KEY : APIキーページで取得したAPIキー。
  - ACCESS-NONCE : 整数値。リクエスト毎に数を増加させる必要があります。通常はUNIXタイムスタンプを使用してください。
  - ACCESS-SIGNATURE : 以下に記述する署名を指定します。

-  HMACを使うため、標準化しないと署名の結果が変わります
  - リクエスト方法（GET 或いは POST）、続けて改行を追加する
  - 小文字のアクセスアドレスに続けて改行を追加する
  - アクセスメソッドへのパス、続けて改行を追加する
  - パラメータ名は、ASCIIコードの順にソートし、URLエンコーディングしてください。
  - 上記の順序で、各パラメーターは文字 '＆'を使用して連結します。
  - 署名元の文字列は右の通りに示されます。
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

## エンドポイント一覧

### アセット

#### アセット一覧を返す

```txt
GET /user/assets
```

**Parameters:**
None

**Response:**

Name | Type | Description
------------ | ------------ | ------------
asset | string | アセット名: [アセット一覧](assets.md)
free_amount | string | 利用可能な量
amount_precision | number | 精度
onhand_amount | string | 保有量
locked_amount | string | ロックされている量
withdrawal_fee | { min: string, max: string } or { under: string, over: string, threshold:string } for `jpy` | 出金手数料
stop_deposit | boolean | 入金ステータス（全ネットワークが入金停止 = `true`）
stop_withdrawal | boolean | 出金ステータス（全ネットワークが出金停止 = `true`）
network_list | { asset: string, network: string, stop_deposit: boolean, stop_withdrawal: boolean, withdrawal_fee: string } or undefined for `jpy` | ネットワーク一覧

**サンプルコード:**

<details>
<summary>Curl</summary>
<p>

```sh
export API_KEY=___your api key___
export API_SECRET=___your api secret___
export ACCESS_NONCE="$(date +%s)"
export ACCESS_SIGNATURE="$(echo -n "$ACCESS_NONCE/v1/user/assets" | openssl dgst -sha256 -hmac "$API_SECRET")"

curl -H 'ACCESS-KEY:'"$API_KEY"'' -H 'ACCESS-NONCE:'"$ACCESS_NONCE"'' -H 'ACCESS-SIGNATURE:'"$ACCESS_SIGNATURE"'' https://api.bitbank.cc/v1/user/assets
```

</p>
</details>


**レスポンスのフォーマット:**

```json
{
  "success": 1,
  "data": {
    "assets": [
      {
        "asset": "string",
        "free_amount": "string",
        "amount_precision": 0,
        "onhand_amount": "string",
        "locked_amount": "string",
        "withdrawal_fee": {
            "min": "string",
            "max": "string"
        },
        "stop_deposit": false,
        "stop_withdrawal": false,
        "network_list": [
            {
                "asset": "string",
                "network": "string",
                "stop_deposit": false,
                "stop_withdrawal": false,
                "withdrawal_fee": "string"
            }
        ]
      },
      {
        "asset": "jpy",
        "free_amount": "string",
        "amount_precision": 0,
        "onhand_amount": "string",
        "locked_amount": "string",
        "withdrawal_fee": {
            "under": "string",
            "over": "string",
            "threshold": "string"
        },
        "stop_deposit": false,
        "stop_withdrawal": false,
    },
    ]
  }
}
```

### 注文情報

#### 注文実行

```txt
GET /user/spot/order
```

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
pair | string | YES | 通貨ペア: [ペア一覧](pairs.md)
order_id | number | YES | 取引ID

**Response:**

Name | Type | Description
------------ | ------------ | ------------
order_id | number | 取引ID
pair | string | 通貨ペア: [ペア一覧](pairs.md)
side | string | `buy` または `sell`
type | string | `limit` または `market` または `stop` または `stop_limit`
start_amount | string | 注文時の数量
remaining_amount | string | 未約定の数量
executed_amount| string | 約定済み数量
price | string \| undefined | 注文価格（type = `limit` または `stop_limit` 時のみ）
post_only | boolean \| undefined | Post Onlyかどうか（type = `limit`時のみ）
average_price | string | 平均約定価格
ordered_at | number | 注文日時(UnixTimeのミリ秒)
expire_at | number \| null | 有効期限(UnixTimeのミリ秒)
triggered_at | number \| undefined | トリガー日時(UnixTimeのミリ秒)（type = `stop` または `stop_limit` 時のみ）
trigger_price | string \| undefined | トリガー価格（type = `stop` または `stop_limit` 時のみ）
status | string | 注文ステータス: `INACTIVE` 非アクティブ, `UNFILLED` 注文中, `PARTIALLY_FILLED` 注文中(一部約定), `FULLY_FILLED` 約定済み, `CANCELED_UNFILLED` 取消済, `CANCELED_PARTIALLY_FILLED` 取消済(一部約定)

**注意事項:**

* このAPIでは3ヶ月以上前の約定済またはキャンセル済注文を取得できません。（50009が返ります。）
  3ヶ月以上前の注文情報の取得にはお手数ですが[注文履歴の抽出ページ](https://app.bitbank.cc/account/data/orders/download)をご利用ください。

**サンプルコード:**

<details>
<summary>Curl</summary>
<p>

```sh
export API_KEY=___your api key___
export API_SECRET=___your api secret___
export ACCESS_NONCE="$(date +%s)"
export ACCESS_SIGNATURE="$(echo -n "$ACCESS_NONCE/v1/user/spot/order?pair=btc_jpy&order_id=1" | openssl dgst -sha256 -hmac "$API_SECRET")"

curl -H 'ACCESS-KEY:'"$API_KEY"'' -H 'ACCESS-NONCE:'"$ACCESS_NONCE"'' -H 'ACCESS-SIGNATURE:'"$ACCESS_SIGNATURE"'' https://api.bitbank.cc/v1/user/spot/order?pair=btc_jpy\&order_id=1
```

</p>
</details>


**レスポンスのフォーマット:**

```json
{
  "success": 1,
  "data": {
    "order_id": 0,
    "pair": "string",
    "side": "string",
    "type": "string",
    "start_amount": "string",
    "remaining_amount": "string",
    "executed_amount": "string",
    "price": "string",
    "post_only": false,
    "average_price": "string",
    "ordered_at": 0,
    "expire_at": 0,
    "triggered_at": 0,
    "trigger_price": "string",
    "status": "string"
  }
}
```

#### 未約定注文一覧

```txt
POST /user/spot/order
```

**Parameters(requestBody):**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
pair | string | YES | 通貨ペア: [ペア一覧](pairs.md)
amount | string | YES | 注文量
price | string | NO | 価格
side | string | YES  | `buy` または `sell`
type | string | YES | `limit` または `market` または `stop` または `stop_limit`
post_only | boolean | NO | Post Onlyかどうか（type = `limit` 時のみ `true` を指定可能。デフォルト `false`）
trigger_price | string | NO | トリガー価格

**Response:**

Name | Type | Description
------------ | ------------ | ------------
order_id | number | 取引ID
pair | string | 通貨ペア: [ペア一覧](pairs.md)
side | string | `buy` または `sell`
type | string | `limit` または `market` または `stop` または `stop_limit`
start_amount | string | 注文時の数量
remaining_amount | string | 未約定の数量
executed_amount| string | 約定済み数量
price | string \| undefined | 注文価格（type = `limit` または `stop_limit` 時のみ）
post_only | boolean \| undefined | Post Onlyかどうか（type = `limit`時のみ）
average_price | string | 平均約定価格
ordered_at | number | 注文日時(UnixTimeのミリ秒)
expire_at | number | 有効期限(UnixTimeのミリ秒)
trigger_price | string \| undefined | トリガー価格（type = `stop` または `stop_limit` 時のみ）
status | string | 注文ステータス: `INACTIVE` 非アクティブ, `UNFILLED` 注文中, `PARTIALLY_FILLED` 注文中(一部約定), `FULLY_FILLED` 約定済み, `CANCELED_UNFILLED` 取消済, `CANCELED_PARTIALLY_FILLED` 取消済(一部約定)

**注意事項:**
- circuit_break_info.mode が `NONE` 以外の場合は成行注文を行うことができず、`70020`エラーが返ります。
- circuit_break_info.mode が `NONE` 以外の場合は `post_only` オプションは `false` として扱われます。

**サンプルコード:**

<details>
<summary>Curl</summary>
<p>

```sh
export API_KEY=___your api key___
export API_SECRET=___your api secret___
export ACCESS_NONCE="$(date +%s)"
export REQUEST_BODY='{"pair": "xrp_jpy", "price": "20", "amount": "1","side": "buy", "type": "limit"}'
export ACCESS_SIGNATURE="$(echo -n "$ACCESS_NONCE$REQUEST_BODY" | openssl dgst -sha256 -hmac "$API_SECRET")"

curl -H 'ACCESS-KEY:'"$API_KEY"'' -H 'ACCESS-NONCE:'"$ACCESS_NONCE"'' -H 'ACCESS-SIGNATURE:'"$ACCESS_SIGNATURE"'' -H "Content-Type: application/json" -d ''"$REQUEST_BODY"'' https://api.bitbank.cc/v1/user/spot/order
```

</p>
</details>


**レスポンスのフォーマット:**

```json
{
  "success": 1,
  "data": {
    "order_id": 0,
    "pair": "string",
    "side": "string",
    "type": "string",
    "start_amount": "string",
    "remaining_amount": "string",
    "executed_amount": "string",
    "price": "string",
    "post_only": false,
    "average_price": "string",
    "ordered_at": 0,
    "expire_at": 0,
    "trigger_price": "string",
    "status": "string"
  }
}
```

#### 注文キャンセル

```txt
POST /user/spot/cancel_order
```

**Parameters(requestBody):**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
pair | string | YES | 通貨ペア: [ペア一覧](pairs.md)
order_id | number | YES | 注文ID

**Response:**

Name | Type | Description
------------ | ------------ | ------------
order_id | number | 注文ID
pair | string | 通貨ペア: [ペア一覧](pairs.md)
side | string | `buy` または `sell`
type | string | `limit` または `market` または `stop` または `stop_limit`
start_amount | string | 注文時の数量
remaining_amount | string | 未約定の数量
executed_amount| string | 約定済み数量
price | string \| undefined | 注文価格（type = `limit` または `stop_limit` 時のみ）
post_only | boolean \| undefined | Post Onlyかどうか（type = `limit`時のみ）
average_price | string | 平均約定価格
ordered_at | number | 注文日時(UnixTimeのミリ秒)
expire_at | number | 有効期限(UnixTimeのミリ秒)
canceled_at | number | キャンセル日時(UnixTimeのミリ秒)
triggered_at | number \| undefined | トリガー日時(UnixTimeのミリ秒)（type = `stop` または `stop_limit` 時のみ）
trigger_price | string \| undefined | トリガー価格（type = `stop` または `stop_limit` 時のみ）
status | string | 注文ステータス: `INACTIVE` 非アクティブ, `UNFILLED` 注文中, `PARTIALLY_FILLED` 注文中(一部約定), `FULLY_FILLED` 約定済み, `CANCELED_UNFILLED` 取消済, `CANCELED_PARTIALLY_FILLED` 取消済(一部約定)

**サンプルコード:**

<details>
<summary>Curl</summary>
<p>

```sh
export API_KEY=___your api key___
export API_SECRET=___your api secret___
export ACCESS_NONCE="$(date +%s)"
export REQUEST_BODY='{"pair": "xrp_jpy", "order_id": 1}'
export ACCESS_SIGNATURE="$(echo -n "$ACCESS_NONCE$REQUEST_BODY" | openssl dgst -sha256 -hmac "$API_SECRET")"

curl -H 'ACCESS-KEY:'"$API_KEY"'' -H 'ACCESS-NONCE:'"$ACCESS_NONCE"'' -H 'ACCESS-SIGNATURE:'"$ACCESS_SIGNATURE"'' -H "Content-Type: application/json" -d ''"$REQUEST_BODY"'' https://api.bitbank.cc/v1/user/spot/cancel_order
```

</p>
</details>


**レスポンスのフォーマット:**

```json
{
  "success": 1,
  "data": {
    "order_id": 0,
    "pair": "string",
    "side": "string",
    "type": "string",
    "start_amount": "string",
    "remaining_amount": "string",
    "executed_amount": "string",
    "price": "string",
    "post_only": false,
    "average_price": "string",
    "ordered_at": 0,
    "expire_at": 0,
    "canceled_at": 0,
    "triggered_at": 0,
    "trigger_price": "string",
    "status": "string"
  }
}
```

#### 注文の一括キャンセル

```txt
POST /user/spot/cancel_orders
```

**Parameters(requestBody):**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
pair | string | YES | 通貨ペア: [ペア一覧](pairs.md)
order_ids | number[] | YES | 注文ID。最大30個まで指定可能

**Response:**

```json
{
  "success": 1,
  "data": {
    "orders": [
      {
        "order_id": 0,
        "pair": "string",
        "side": "string",
        "type": "string",
        "start_amount": "string",
        "remaining_amount": "string",
        "executed_amount": "string",
        "price": "string",
        "post_only": false,
        "average_price": "string",
        "ordered_at": 0,
        "expire_at": 0,
        "canceled_at": 0,
        "triggered_at": 0,
        "trigger_price": "string",
        "status": "string"
      }
    ]
  }
}
```

#### 条件付き注文の一括キャンセル

```txt
POST /user/spot/orders_info
```

*POSTメソッド注意*

**Parameters (requestBody):**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
pair | string | YES | 通貨ペア: [ペア一覧](pairs.md)
order_ids | number[] | YES | 注文ID

**注意事項:**

* このAPIでは3ヶ月以上前の約定済またはキャンセル済注文を取得できません。（エラーとならず、結果に含まれません。）
  3ヶ月以上前の注文情報の取得にはお手数ですが[注文履歴の抽出ページ](https://app.bitbank.cc/account/data/orders/download)をご利用ください。

**サンプルコード:**

<details>
<summary>Curl</summary>
<p>

```sh
export API_KEY=___your api key___
export API_SECRET=___your api secret___
export ACCESS_NONCE="$(date +%s)"
export REQUEST_BODY='{"pair": "xrp_jpy", "order_ids": [1]}'
export ACCESS_SIGNATURE="$(echo -n "$ACCESS_NONCE$REQUEST_BODY" | openssl dgst -sha256 -hmac "$API_SECRET")"

curl -H 'ACCESS-KEY:'"$API_KEY"'' -H 'ACCESS-NONCE:'"$ACCESS_NONCE"'' -H 'ACCESS-SIGNATURE:'"$ACCESS_SIGNATURE"'' -H "Content-Type: application/json" -d ''"$REQUEST_BODY"'' https://api.bitbank.cc/v1/user/spot/orders_info
```

</p>
</details>


**レスポンスのフォーマット:**

```json
{
  "success": 1,
  "data": {
    "orders": [
      {
        "order_id": 0,
        "pair": "string",
        "side": "string",
        "type": "string",
        "start_amount": "string",
        "remaining_amount": "string",
        "executed_amount": "string",
        "price": "string",
        "post_only": false,
        "average_price": "string",
        "ordered_at": 0,
        "expire_at": 0,
        "triggered_at": 0,
        "trigger_price": "string",
        "status": "string"
      }
    ]
  }
}
```

#### 注文の照会

```txt
GET /user/spot/active_orders
```

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
pair | string | YES | 通貨ペア: [ペア一覧](pairs.md)
count | number | NO | 取得する注文数
from_id | number | NO | 取得開始注文ID
end_id | number | NO | 取得終了注文ID
since | number | NO | 開始UNIXタイムスタンプ
end | number | NO | 終了UNIXタイムスタンプ

**Response:**

Name | Type | Description
------------ | ------------ | ------------
order_id | number | 注文ID
pair | string | 通貨ペア: [ペア一覧](pairs.md)
side | string | `buy` または `sell`
type | string | `limit` または `market` または `stop` または `stop_limit`
start_amount | string | 注文時の数量
remaining_amount | string | 未約定の数量
executed_amount| string | 約定済み数量
price | string \| undefined | 注文価格（type = `limit` または `stop_limit` 時のみ）
post_only | boolean \| undefined | Post Onlyかどうか（type = `limit`時のみ）
average_price | string | 平均約定価格
ordered_at | number | 注文日時(UnixTimeのミリ秒)
expire_at | number | 有効期限(UnixTimeのミリ秒)
executed_at | number \| undefined | 約定日時(UnixTimeのミリ秒)
canceled_at | number \| undefined | キャンセル日時(UnixTimeのミリ秒)
triggered_at | number \| undefined | トリガー日時(UnixTimeのミリ秒)（type = `stop` または `stop_limit` 時のみ）
trigger_price | string \| undefined | トリガー価格（type = `sopt` または `stop_limit` 時のみ）
status | string | 注文ステータス: `INACTIVE` 非アクティブ, `UNFILLED` 注文中, `PARTIALLY_FILLED` 注文中(一部約定), `FULLY_FILLED` 約定済み, `CANCELED_UNFILLED` 取消済, `CANCELED_PARTIALLY_FILLED` 取消済(一部約定)

**サンプルコード:**

<details>
<summary>Curl</summary>
<p>

```sh
export API_KEY=___your api key___
export API_SECRET=___your api secret___
export ACCESS_NONCE="$(date +%s)"
export ACCESS_SIGNATURE="$(echo -n "$ACCESS_NONCE/v1/user/spot/active_orders?pair=btc_jpy" | openssl dgst -sha256 -hmac "$API_SECRET")"

curl -H 'ACCESS-KEY:'"$API_KEY"'' -H 'ACCESS-NONCE:'"$ACCESS_NONCE"'' -H 'ACCESS-SIGNATURE:'"$ACCESS_SIGNATURE"'' https://api.bitbank.cc/v1/user/spot/active_orders?pair=btc_jpy
```

</p>
</details>


**レスポンスのフォーマット:**

```json
{
  "success": 1,
  "data": {
    "orders": [
      {
        "order_id": 0,
        "pair": "string",
        "side": "string",
        "type": "string",
        "start_amount": "string",
        "remaining_amount": "string",
        "executed_amount": "string",
        "price": "string",
        "post_only": false,
        "average_price": "string",
        "ordered_at": 0,
        "expire_at": 0,
        "triggered_at": 0,
        "trigger_price": "string",
        "status": "string"
      }
    ]
  }
}
```

### 約定履歴

#### 約定履歴の検索

```txt
GET /user/spot/trade_history
```

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
pair | string | YES | 通貨ペア: [ペア一覧](pairs.md)
count | number | NO | 取得する約定数(最大1000)
order_id | number | NO | 注文ID
since | number | NO | 開始UNIXタイムスタンプ
end | number | NO | 終了UNIXタイムスタンプ
order | string | NO | 約定時刻順序(`asc`: 昇順、`desc`: 降順、デフォルト`desc`)

**Response:**

Name | Type | Description
------------ | ------------ | ------------
trade_id | number | trade id
pair | string | 通貨ペア: [ペア一覧](pairs.md)
order_id | number | 注文ID
side | string | `buy` または `sell`
type | string | `limit` または `market` または `stop` または `stop_limit`
amount | string | 注文量
price | string | 価格
maker_taker | string | `maker` または `taker`
fee_amount_base | string | base手数料
fee_amount_quote | string | quote手数料
executed_at | number | 約定日時（UnixTimeのミリ秒）

**サンプルコード:**

<details>
<summary>Curl</summary>
<p>

```sh
export API_KEY=___your api key___
export API_SECRET=___your api secret___
export ACCESS_NONCE="$(date +%s)"
export ACCESS_SIGNATURE="$(echo -n "$ACCESS_NONCE/v1/user/spot/trade_history?pair=btc_jpy" | openssl dgst -sha256 -hmac "$API_SECRET")"

curl -H 'ACCESS-KEY:'"$API_KEY"'' -H 'ACCESS-NONCE:'"$ACCESS_NONCE"'' -H 'ACCESS-SIGNATURE:'"$ACCESS_SIGNATURE"'' https://api.bitbank.cc/v1/user/spot/trade_history?pair=btc_jpy
```

</p>
</details>


**レスポンスのフォーマット:**

```json
{
  "success": 1,
  "data": {
    "trades": [
      {
        "trade_id": 0,
        "pair": "string",
        "order_id": 0,
        "side": "string",
        "type": "string",
        "amount": "string",
        "price": "string",
        "maker_taker": "string",
        "fee_amount_base": "string",
        "fee_amount_quote": "string",
        "executed_at": 0
      }
    ]
  }
}
```

### 入出金

#### 入出金履歴を取得する

```txt
GET /user/deposit_history
```

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
asset | string | YES | アセット名: [アセット一覧](assets.md)
count | number | NO | 取得する履歴数(最大100)
since | number | NO | 開始UNIXタイムスタンプ(ミリ秒)
end | number | NO | 終了UNIXタイムスタンプ(ミリ秒)

**Response:**

Name | Type | Description
------------ | ------------ | ------------
uuid | string | 入金識別uuid
address | string | 入金address
asset | string | アセット名: [アセット一覧](assets.md)
network | string | ネットワーク名: [ネットワーク一覧](networks.md)
amount | number | 入金数量
txid | string \| null | 入金トランザクションID(暗号資産の時のみ)
status | string | 入金状態: `FOUND`, `CONFIRMED`, `DONE`
found_at | number| 検知UNIXタイムスタンプ(ミリ秒)
confirmed_at | number | 承認(残高追加確定時)UNIXタイムスタンプ(ミリ秒、承認後のみ存在)


**注意事項:**

* 現時点では入金履歴レスポンスには宛先タグ、メモおよび銀行口座情報が含まれていません。他システムの送金・送信との突合にはtxidをお使いください。

**サンプルコード:**

<details>
<summary>Curl</summary>
<p>

```sh
export API_KEY=___your api key___
export API_SECRET=___your api secret___
export ACCESS_NONCE="$(date +%s)"
export ACCESS_SIGNATURE="$(echo -n "$ACCESS_NONCE/v1/user/deposit_history?asset=btc" | openssl dgst -sha256 -hmac "$API_SECRET")"

curl -H 'ACCESS-KEY:'"$API_KEY"'' -H 'ACCESS-NONCE:'"$ACCESS_NONCE"'' -H 'ACCESS-SIGNATURE:'"$ACCESS_SIGNATURE"'' https://api.bitbank.cc/v1/user/deposit_history?asset=btc
```

</p>
</details>


**レスポンスのフォーマット:**

```json
{
  "success": 1,
  "data": {
    "deposits": [
      {
        "uuid": "string",
        "asset": "string",
        "network": "string",
        "amount": "string",
        "txid": "string",
        "status": "string",
        "found_at": 0,
        "confirmed_at": 0
      }
    ]
  }
}
```

#### 出金申請

```txt
GET /user/withdrawal_account
```

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
asset | string | YES | アセット名: [アセット一覧](assets.md)

**Response:**

Name | Type | Description
------------ | ------------ | ------------
uuid | string | 出金アカウントのID
label | string | ラベル
network | string | ネットワーク名: [ネットワーク一覧](networks.md)
address | string | 出金先アドレス

**サンプルコード:**

<details>
<summary>Curl</summary>
<p>

```sh
export API_KEY=___your api key___
export API_SECRET=___your api secret___
export ACCESS_NONCE="$(date +%s)"
export ACCESS_SIGNATURE="$(echo -n "$ACCESS_NONCE/v1/user/withdrawal_account?asset=btc" | openssl dgst -sha256 -hmac "$API_SECRET")"

curl -H 'ACCESS-KEY:'"$API_KEY"'' -H 'ACCESS-NONCE:'"$ACCESS_NONCE"'' -H 'ACCESS-SIGNATURE:'"$ACCESS_SIGNATURE"'' https://api.bitbank.cc/v1/user/withdrawal_account?asset=btc
```

</p>
</details>


**レスポンスのフォーマット:**

```json
{
  "success": 1,
  "data": {
    "accounts": [
      {
        "uuid": "string",
        "label": "string",
        "network": "string",
        "address": "string"
      }
    ]
  }
}
```

#### 出金のキャンセル

```txt
POST /user/request_withdrawal
```

**Parameters (requestBody):**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
asset | string | YES | アセット名: [アセット一覧](assets.md)
uuid | string | YES | 出金アカウントのuuid
amount | string | YES | 出金数量
otp_token | string | NO | 二段階認証トークン(設定している場合、otp_tokenかsms_tokenのどちらか一方を指定)
sms_token | string | NO | SMS認証トークン

**Response:**

Name | Type | Description
------------ | ------------ | ------------
uuid | string | 出金識別ID
asset | string | アセット名: [アセット一覧](assets.md)
account_uuid | string | 出金アカウントのID
amount | string | 出金数量
fee | string | 出金手数料
label | string | 出金先アドレスにつけたラベル(暗号資産の時のみ)
address | string | 出金先アドレス(暗号資産の時のみ)
network | string | ネットワーク名(暗号資産の時のみ): [ネットワーク一覧](networks.md)
destination_tag | number or string | 出金先宛先タグまたはメモ(タグまたはメモを指定した暗号資産の出金時のみ)
txid | string \| null | 出金トランザクションID(暗号資産の時のみ)
bank_name | string | 出金先銀行(法定通貨の時のみ)
branch_name | string | 出金先銀行支店(法定通貨の時のみ)
account_type | string | 出金先口座種別(法定通貨の時のみ)
account_number | string | 出金先口座番号(法定通貨の時のみ)
account_owner | string | 出金先口座名義(法定通貨の時のみ)
status | string | ステータス: `CONFIRMING`, `EXAMINING`, `SENDING`,  `DONE`, `REJECTED`, `CANCELED`, `CONFIRM_TIMEOUT`
requested_at | number | リクエスト日時UNIXタイムスタンプ(ミリ秒)


**サンプルコード:**

<details>
<summary>Curl</summary>
<p>

```sh
export API_KEY=___your api key___
export API_SECRET=___your api secret___
export ACCESS_NONCE="$(date +%s)"
export REQUEST_BODY='{"asset": "xrp", "uuid": "___your uuid___", "amount": "1"}'
export ACCESS_SIGNATURE="$(echo -n "$ACCESS_NONCE$REQUEST_BODY" | openssl dgst -sha256 -hmac "$API_SECRET")"

curl -H 'ACCESS-KEY:'"$API_KEY"'' -H 'ACCESS-NONCE:'"$ACCESS_NONCE"'' -H 'ACCESS-SIGNATURE:'"$ACCESS_SIGNATURE"'' -H "Content-Type: application/json" -d ''"$REQUEST_BODY"'' https://api.bitbank.cc/v1/user/request_withdrawal
```

</p>
</details>



**レスポンスのフォーマット:**

```json
{
  "success": 1,
  "data": {
    "uuid": "string",
    "asset": "string",
    "account_uuid": "string",
    "amount": "string",
    "fee": "string",

    "label": "string",
    "address": "string",
    "network": "string",
    "txid": "string",
    "destination_tag": 0,

    "bank_name": "string",
    "branch_name": "string",
    "account_type": "string",
    "account_number": "string",
    "account_owner": "string",

    "status": "string",
    "requested_at": 0
  }
}
```

