# 非機能要件

## アベイラビリティ

- 24時間365日稼働を目標とする。
  - 完全冗長化により、ダウンタイムなくフェールオーバーする。
  - ディザスタリカバリとして、AWS別リージョンにも展開可能なように準備をおこなう。ただし、マルチA/Z対応は現時点ではおこなわない。

## パフォーマンス

- 同時アクセス数　1日のピークタイムで10PV/分（月間30万PV相当）
- 要求速度
  - 検索や更新のSQL発行全体で1秒以内
  - フロント表示まで含めて3秒以内

## スケーラビリティ

- データベースについては、AWS RDSを使うことで自動スケールとする
  - 現行システムのユーザー数の100倍が加入可能・利用可能であること
- Webサーバーについては、EC2をオートスケーリングするように設定する

## セキュリティ

- サービスへのアクセスはすべて SSL 接続をおこなう
- Webの認証は、標準のAuth（session）を使用する  
- APIの認証は、JWTを使用する [参考](https://hackthestuff.com/article/laravel-8-jwt-authentication-tutorial)  
- お客様の個人情報（名前・生年月日・メールアドレス・電話番号・郵便番号・住所）をデータベースに保存する際には、可逆暗号化して保存する

## メンテナンス性

- メンテナンスの際に停止する場合は、お客様と相談のうえ停止時間を設定する。
  - ただし、冗長化によって極力必要ないものとする
- [ブルーグリーンデプロイメント](https://www.redhat.com/ja/topics/devops/what-is-blue-green-deployment)を実施して、機能変更や追加の際にシステムを停止しない

## 運用性

- 正常稼働を自動監視する。
  - 異常時にアラートメールを送信する（AWS CloudWatch）
- データベースのバックアップを自動的におこなう（AWS RDSスナップショット→日次）

## 技術・環境用件

- AWSに本番環境を構築する（EC2, RDS, S3, VPCなど）
- PHP8以上、Laravel8以上、MySQL5.7以上を使用する