# マイクロサービスにおける認証方法と実装

## 2025/12/27
gitのテスト

## 2025/6/5

### Cognitoの検証
以下内容を実機（CognitoとAWS CLI）で検証した。  
[今から始める Amazon Cognito 入門 #1：ユーザープールと ID プールの違い](https://dev.classmethod.jp/articles/get-started-with-amazon-cognito-now-1/) 

![image](./_image/001-01.png)


## 今後やりたいこと
以下イメージ図と案件でやろうとしていること（川畑さんに相談）して、案件のアーキ図を整理して実装（フィージビリティ担保）までやりたい。  
[私が考えるマイクロサービスアーキテクチャ](https://zenn.dev/rescuenow/articles/8396c8528a50e2)

![image](./_image/001-02.png)

## 2025/6/20
ECSでタスクを上げておき、ALBのパスベースルーティングで各タスクにアクセスできることを確認する。
1. 以下ファイルをapp1のコンテナのドキュメントルート/app1に格納する（app2も同様）
    ```html:index.html
    <!DOCTYPE html>
    <html>
        <head>
            <title>
                App1
            </title>
            <body>
                <h1>
                    Hello from /app1!!!
                </h1>
            </body>
        </head>    
    </html>
    ```

2. aaa
