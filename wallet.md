# ウォレット関連
---------------------------------------------------


## ステータス一覧
------------------------------------------------------------------

**暗号資産出金ステータスの定義**
 Status | Description
------------ | ------------ 
submitted | リクエスト済み
reexamine | 審査中
canceled | キャンセル
pass | 承認
reject | 拒否
pre-transfer | 処理中
wallet-transfer | 送金済み
wallet-reject | ウォレットの拒否
confirmed | ブロックチェーン上で承認済み
confirm-error | ブロックチェーン上で承認エラー
repealed | キャンセル

**暗号資産入金ステータスの定義**
 Status | Description
------------ | ------------ 
unknown | 不明
confirming | 確認中
confirmed | 確認済み
safe | 	完了
orphan | 独立


## 暗号資産の出金申請
------------------------------------------
このエンドポイントは暗号資産の出金申請を送信します。


**HTTP Request**

```
POST /v1/dw/withdraw/api/create
```

```
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

上記のコマンドは、次のような構造のJSONを返します。

```
{
  "status": "ok",
  "data": 700
}
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


## 暗号資産出金のキャンセル
---------------------------------------------------
このエンドポイントは暗号資産出金のキャンセルを送信します。


**HTTP Request**

```
POST /v1/dw/withdraw-virtual/{withdraw-id}/cancel
```

```
curl -X POST \
     -H "Content-Type: application/json" \
     "https://api-cloud.bittrade.co.jp/v1/dw/withdraw-virtual/{withdraw-id}/cancel"
```

上記のコマンドは、次のような構造のJSONを返します。

```
{
  "status": "ok",
  "data": 700
}
```

**Query Parameters**
Parameter | Description
------------ | ------------ 
withdraw-id | 出金ID


**Response Data**
Parameter | Description
------------ | ------------ 
data | 出金ID
