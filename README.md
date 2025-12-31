# マイクロサービスにおける認証方法と実装

## 2025/12/27
gitのテスト

## 2025/6/5

### Cognitoの検証
以下内容を実機（CognitoとAWS CLI）で検証した。  
https://dev.classmethod.jp/articles/get-started-with-amazon-cognito-now-1/  

![image](https://github.com/user-attachments/assets/babe42a7-8303-4373-9173-c3c3af176612)


## 今後やりたいこと
以下イメージ図と案件でやろうとしていること（川畑さんに相談）して、案件のアーキ図を整理して実装（フィージビリティ担保）までやりたい。  
https://zenn.dev/rescuenow/articles/8396c8528a50e2  

![image](https://github.com/user-attachments/assets/ad158e49-30c4-4244-8ff4-7e0582c18a5d)

## 2025/6/20
ECSでタスクを上げておき、ALBのパスベースルーティングで各タスクにアクセスできることを確認する。
１）以下ファイルをapp1のコンテナのドキュメントルート/app1に格納する（app2も同様）
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

２）
