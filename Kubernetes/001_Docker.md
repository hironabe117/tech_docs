## Docker
参考
- [Amazon Linux 2023にDockerとDocker Composeのインストール](https://zenn.dev/rock_penguin/articles/28875c7b0a5e30)

1. ECRログイン（アカウントIDのみ要変更）
    ```bash
    aws ecr get-login-password --region ap-northeast-1 | docker login --username AWS --password-stdin [8517XXXXXXXX.dkr.ecr.ap-northeast-1.amazonaws.com](http://8517XXXXXXXX.dkr.ecr.ap-northeast-1.amazonaws.com/)
    ```

1. Dockerビルド（タグを要変更）
    ```bash
    docker build -t django:1019-01 .
    ```

1. Dockerタグ付け（タグ・アカウントIDを要変更）

    ```bash
    docker tag django:1019-01 [8517XXXXXXXX.dkr.ecr.ap-northeast-1.amazonaws.com/django:](http://8517XXXXXXXX.dkr.ecr.ap-northeast-1.amazonaws.com/django:latest)1019-01
    ```

1. ECRへのプッシュ（タグ・アカウントIDを要変更）

    ```bash
    docker push [8517XXXXXXXX.dkr.ecr.ap-northeast-1.amazonaws.com/django:](http://8517XXXXXXXX.dkr.ecr.ap-northeast-1.amazonaws.com/django:latest)1019-01 
    ```

    オプションの順序が違うとエラーになる
    ```bash
    docker run -d -p 8000:8000 django:1019-01
    ```