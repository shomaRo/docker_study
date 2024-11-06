# Dockerの構築フロー

### コンテナを作ることが最終目的

アプリケーション用のコンテナ、データベースのコンテナとか、その他諸々のコンテナを作る

----


### Dockerfile
- 自分で作る
- Imageを自作するためのファイル

**`↓build`**

### Image(イメージ)
- Containerを起動する土台
- DockerHubで公開されている


**`↓run`**


### Container(コンテナ)
- アプリケーションの実行環境
- 使い捨てできる(実行、停止、削除)


## なんでDockerHubで公開されているのに自分でDockerfile作るの？？？

わからないよね？そこでまず疑問持つよね？？その気持ちがわかります

実際の現場ではそのままのイメージを使うことはない。
なぜなら、アプリケーションを作って運用する場合は自分たちでカスタマイズをしたいことが多い！
そんなときに使われるのがDockerfile

イメージをカスタマイズ、自作することができる。

---

### さらに便利になります

dockerfileを一つずつ書いてコンテナを一つずつ立ち上げなきゃいけない、、、のか、、、

いいえ、違います！！今回ご紹介するのがこちら！！

**Docker Compose**となります。

- 複数のコンテナを定義して実行できるDockerのためのツール
- yamlファイル(~.yml)を使って定義を記載
- コマンド1つで定義するすべてのコンテナを立ち上げることができる。

もうすごい！！

---

### Docker Composeの使い方3ステップ

1. 目的に合わせた「Dockerfile」を作る
2. docker-compose.yml(compose.yml)を作る
3. 「docker compose up」コマンドを実行する

えええええええ、こんな簡単に出来ちゃうんですかアアア！？

### でも実際にDocker Composeがやってくれることってなんなの？

答えは簡単！！

buildとrun

ええええええ、たったそれだけなんですかああああああ！？


# ここからは細かく見ていきまっしょ！！！

### Dockerfileの中身を覗いてみよう
Dockerfile
```Dockerfile
FROM golang:1.20-alpine AS builder

RUN apk update && apk add --no--cache git

WORKDIR /app

RUN go install github.com/google/wire/cmd/wire@latest

COPY go.mod go.sum ./
RUN go mod tidy && go mod download

COPY . .

RUN wire


```
