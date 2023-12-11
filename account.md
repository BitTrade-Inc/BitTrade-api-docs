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
