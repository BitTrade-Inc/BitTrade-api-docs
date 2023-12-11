# アカウント関連　
------------------------------------------------------------
※署名認証が必要です。

## ユーザアカウント
--------------------------------------------------------------
このエンドポイントはユーザのアカウント情報を返します。

**HTTP Request**

```
GET /v1/account/accounts
```

```
curl -X GET "https://api-cloud.bittrade.co.jp/v1/account/accounts"
```

上記のコマンドは、次のような構造のJSONを返します。

```
{
  "status": "ok",
  "data": [
    {
      "id": 100009,
      "type": "spot",
      "state": "working",
      "user-id": 1000
    }
  ]
```

**Response Data**
Parameter | Description
------------ | ------------
id | アカウントID
type | アカウントタイプ
state | アカウントステータス
user-id | 	ユーザID


## 残高照会
-----------------------------------------------------------
このエンドポイントはユーザの残高情報を返します。

**HTTP Request**

```
GET /v1/account/accounts/{account-id}/balance
```

```
curl -X GET "https://api-cloud.bittrade.co.jp/v1/account/accounts/{account-id}/balance"
```

上記のコマンドは、次のような構造のJSONを返します。

```
{
  "status": "ok",
  "data": {
    "id": 100009,
    "type": "spot",
    "state": "working",
    "list": [
      {
        "currency": "jpy",
        "type": "trade",
        "balance": "500009195917.4362872650"
      },
```

**Response Data**
Parameter | Description
------------ | ------------
id | アカウントID
type | アカウントタイプ
state | アカウントステータス
list | 	残高情報

**List Data**
Field | Description
------------ | ------------
balance | 残高
currency | 暗号資産
type | trade：取引残高
frozen：凍結残高
