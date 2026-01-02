# Amplifyとは
AWS Amplifyは、フロントエンドとバックエンドのアプリケーション開発を加速するためのフレームワークとサービスの集合体です。
特に、第二世代にあたるAmplify Gen2は、TypeScriptと緊密に統合された新世代のツールセットで、以下のような特徴を持っています。
- TypeScriptファースト：TypeScriptによる型の安全性を最大限に活用
- セキュリティとコンプライアンス：AWSのセキュリティ基準に準拠
- コード中心のアプローチ：UIからではなく、コードを使ってインフラを定義
 
# Amplify Gen2の主な機能
Amplify Gen2は2023年にリリースされたAmplifyの新しいバージョンである。それまでのGen1と比較して、TypeScriptのサポート強化、出力されるコードのサイズ最適化、コード中心のアプローチなど、多くの改善が施されている。
以下のようなクラウドサービスを簡単に組み込むことができる。
- 認証（Authentication）：Amazon Cognitoを使ったユーザ認証
- データストア：AWS AppSyncとAmazon DynamoDBを活用したGraphQLベースのデータ管理
- ストレージ：Amazon S3を使ったファイルストレージ
- API:AWS AppSyncを使ったGraphQL APIやAmazon API Gatewayを使ったREST APIの構築
- ホスティング：グローバルCDNを通じたWebアプリケーションのホスティング
- 継続的デプロイ（CI/CD）：GitHubなどと連携した自動デプロイ