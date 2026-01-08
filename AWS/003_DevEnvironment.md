## 背景・思い

- Cloud9でよく基盤開発していたので、なくなって困っている。
    - terraformやCloudFormation、コンテナアプリ（Node.jsやpython）等の開発・検証用途。
- AWSオンリーの案件が多いため、AWSに閉じるソリューションがベター。

## 代替案の検討

- 代替案としては下記がある。もっといい方法があれば知りたい。
    1. Cloudshell
        - 素早く手軽だが、揮発性に対する工夫がいるしスペック低いのでビルドで詰む
    2. SageMakerStudio
        - AI流行りでこちらを推したいのだろうが、案件でSageMaker自体を使わないので説明や設定などがめんどくさそう。
    3. GitHub Codespaces
        - 便利だがGitHub使ってないとNG。AWS内で閉じれない。
    4. amazon workspaces
        - VDI払い出し。コードが書ければいいためこれは少し過剰。
    5. code-server on EC2
        - 公式も推してるのでこれを試す。いろんな人が試してて導入しやすそう。
        https://repost.aws/ja/questions/QUiHHHrRawTC6pF5gWOTeNyA/aws-cloud-9-%E3%81%AE%E4%BB%A3%E6%9B%BF%E6%A1%88%E3%81%AB%E3%81%A4%E3%81%84%E3%81%A6

## 検証

以下記事を実施し、パブリックサブネット上のEC2でCloud9ぽく作ってみた。

結論、ポイントや課題は下記の通り。

- ソースIP制限あり
- HTTPSの証明書がない
- マルチセッションには向かない
    
    ![image.png](attachment:578884ac-1bba-472d-be58-db609ada9c8d:image.png)
    

　→Cloud9同様、ユーザごとにEC2立てるのが無難か。

　　★無理やりやろうと思えば、同一EC2上でポート番号を変えて複数のcode-serverを立てて、ALBを置いてクライアントごとにターゲット分ける方法（同一EC2でポート分け）で可能か。

https://dev.classmethod.jp/articles/code-server-multi-user-ec2-review/

https://dev.classmethod.jp/articles/use-code-server-as-temporary-cloud9-alternative/
