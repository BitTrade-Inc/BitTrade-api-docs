
# エラーコード一覧
-------------------------------------
## 共通情報
エラーコード | 説明
------------ | ------------
base-symbol-error | 取引ペアが存在しない
base-currency-error | 銘柄が存在しない
base-date-error | 日時フォーマットのエラー
account-transfer-balance-insufficient-error |  残高不足
bad-argument | パラメータの期限切れ
api-signature-not-valid | API署名エラー
gateway-internal-error | ゲートウェイ内部エラー
security-require-assets-password | 資産パスワードの入力が必要
audit-failed | 注文失敗
ad-ethereum-address | 有効な出金アドレスを入力
order-accountbalance-error | アカウント残高不足
order-limitorder-price-error | 指値の注文価格が制限を超えている
order-limitorder-amount-error	 | 指値の注文数量が制限を超えている
order-orderprice-precision-error | 注文価格が制限精度を超えている
order-orderamount-precision-error | 注文数量が制限精度を超えている
order-marketorder-amount-error | 注文数量が制限を超えている
order-queryorder-invalid | 注文が見つからない
order-orderstate-error | 注文ステータスエラー
order-datelimit-error | 注文照会タイムアウト
order-update-error | 注文更新失敗


## Webscket方式
エラーコード | エラー情報 | 説明
------------ | ------------ | ------------
200 |  | 正常に処理
100 | TimeOut Close | タイムアウト
400| Bad Request | パラメーターエラー
404 | Not Found | サービスが見つからない
429 | Too Many Requests | 接続上限超過
500 |  | システムエラー
2000 | invalid.ip | 無効なIP
2001 | nvalid.json | 無効なリクエストJSON
2001 | invalid.authType | 間違った署名方法
2001 | invalid.action | 無効なアクション
2001 | invalid.symbol | 無効な取引ペア
2001 | invalid.ch | 無効な購読
2001 | invalid.exchange | 無効な取引所コード
2001 | missing.param.auth | 署名パラメータ不足
2002 | invalid.auth.state | 認証失敗
2002 | auth.fail | 署名の検証に失敗
2003 | query.account.list.error | アカウントリストのクエリに失敗
4000 | too.many.request | クライアントアップリンク要求の頻度制限
4000 | too.many.connection | 接続上限超過

