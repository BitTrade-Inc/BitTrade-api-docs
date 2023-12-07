# システム情報関連
----------------------------------------------------------------
## 取引ペア情報

このエンドポイントは各取引ペアの精度を返します。

**HTTP Request**

GET /v1/common/symbols

```
curl -X GET \
      "https://api-cloud.bittrade.co.jp/v1/common/symbols"
```

上記のコマンドは次のような構造のJSONを返します。
```
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

**Reception Data**

