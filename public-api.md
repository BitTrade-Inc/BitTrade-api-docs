Public API
-----------------------------------------------
公開されている取引所の注文状況や取引履歴、注文板を確認することができます。


#ティッカー

各取引ペアのティッカー情報を取得することができます。

##HTTP REQUEST

```txt
GET /market/detail/merged
```

**Parameters:**

Parameter | Required | Description
------------ | ------------ | ------------
symbol | true | 取引ペア: [ペア一覧](pairs.md)
