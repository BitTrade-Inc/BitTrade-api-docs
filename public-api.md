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

```
{
    "ch": "market.btcjpy.detail.merged",
    "status": "ok",
    "ts": 1701829427499,
    "tick": {
        "id": 400751827463,
        "version": 400751827463,
        "open": 6153770,
        "close": 6461462,
        "low": 6103231,
        "high": 6556474,
        "amount": 122.17410241972473,
        "vol": 768756402.9727342,
        "count": 25178,
        "bid": [
            6461461,
            0.00002
        ],
        "ask": [
            6461647,
            0.00002
        ]
    }
}
```

**Response:**

Parameter | Description
------------ | ------------
status |リクエスト処理結果
ts | 現在の時刻
open | 24時間前の始値
close | 最新取引価格
low | 過去24時間の最安値取引価格
high | 過去24時間の最高値取引価格
amount | 過去24時間の取引量
vol | 過去24時間の取引金額
bid | 現在の最高売値
ask | 現在の最低買値
