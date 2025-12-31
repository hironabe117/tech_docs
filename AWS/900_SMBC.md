## CodeCommit
- Gitの権限はIAMで管理することになる。あまり柔軟ではない。

## EventBridge
- AWSイベントやスケジュールイベントはDefaultイベントバスにしか通知されない。
- カスタムイベントバスにルールは作成できるが、通知されない罠がある。

## SES承認ポリシー
- Lambda→SESの構成では、LambdaのIAMロールとSES承認ポリシーの条件を両方見て強い方が働く。
- 権限の強さは①明示Deny②明示Allow③暗黙Deny
- IAMロールでSESFullAccessしていると②が働いている形のため、SES承認ポリシーの方で③が働かなくなる。

## マルチリージョンact/act
CloudFront配下でRoute53を活用してマルチリージョンのAct/Actを実現する。
- [レイテンシベースルーティングを使用して Amazon CloudFront で マルチリージョンのアクティブ – アクティブなアーキテクチャを実現する](https://aws.amazon.com/jp/blogs/news/latency-based-routing-leveraging-amazon-cloudfront-for-a-multi-region-active-active-architecture/)