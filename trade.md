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
submitted  発注
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
sauce | 注文方法[default/api]
symbol | 通貨ペア
type | 注文タイプ

**※buy-limit-maker**

“注文価格”>=“市場最低売り価格”である場合、注文は約定されません。

“注文価格”<“市場最低売り価格”である場合，送信成功後，この注文はシステムによって受け入れられます。
