# Rest API共通情報
-----------------------------------------------------
本APIを使って、取引プログラムの実装ができます。

APIを使えば以下の機能が使用できます。

●下記のマーケット情報を取得可能

・ローソク足

・板情報

・BBO

・約定情報

・24時間相場情報

・etc,

●資産参照、出金申請

●取引所関連、注文、注文取消、注文履歴の参照

●販売所関連、価格監視、注文


## セキュリティ認証
--------------------------------------------------------
1.APIキーの申請

・APIキーの作成と変更について、「マイページ＞API＞秘密キーを作成」にて行って下さい。

 Key | Description
------------ | ------------
AccessKey | アクセスキー
SecretKey | シークレットキー

※プライベートキーは発行時のみ表示されますのでご注意ください。

2.APIキーの権限設定

authority | Description
------------ | ------------
読取 | アカウント情報、取引履歴、入出金履歴の参照ができます
出金 | 暗号資産の出金申請が可能です
取引 | 取引所、販売所の取引注文、キャンセルができます。

・デフォルトは読取権限になります。

・他の権限を追加したい場合、権限設定のチェックボックスをチェックしてから作成してください。

3.リクエストの共通仕様

セキュリティ上の理由で、公開API以外のリクエストには全て署名が必要となります。これから署名に関する手順を説明します

**HOST**

URL: https://api-cloud.bittrade.co.jp

**共通仕様**

Header | Description
------------ | ------------
UserAgent | User-Agent: Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/39.0.2171.71 Safari/537.36
Language | Accept-Language: ja-JP
POST Request | Content-Type: application/json
GET Request | Content-Type: application/x-www-form-urlencoded
Response Format | json

**認証用共通パラメータ**

Parameter | Description
------------ | ------------
AccessKeyId | アクセスキー
SignatureMethod | 署名の演算時に用いるハッシュベースプロトコル、ここではHmacSHA256を指定します
SignatureVersion | 署名プロトコルのバージョン、ここでは2を指定します
Timestamp | リクエスト時のUnix Timestamp(UTC 時間) 。
Signature | 署名に基づいて計算された値。署名が有効で改ざんされていないことを保証するために使用されます。

※POSTリクエストに対して、下記のパラメータのみ署名する必要があります。そのほかのパラメータはRequest Bodyに入れてください

・AccessKeyId

・SignatureMethod

・SignatureVersion

・Timestamp

