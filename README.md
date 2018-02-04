# http-header-docker-compose 概要

サブディレクトリ毎に HTTP サーバへリバースプロクシする環境における HTTP ヘッダとなるコンテナ。

# 使い方

## デフォルト環境

### 環境説明

- /wiki 配下へのアクセスは `ryu310/pukiwiki-plus` へ Reverse Proxy される
- Index ページは /wiki へのリンクが表示される
- php は利用不可

### 起動方法

```sh
$ curl -o docker-compose.yml https://raw.githubusercontent.com/ryu-sato/http-header-docker-compose/master/docker-compose.yml
$ docker-compose up -d
```

インストール後、`http://localhost/` にアクセスすると Index (静的ページ) が表示される。
リンクをクリックするか、または `http://localhost/wiki/` にアクセスすると pukiwiki が表示される。

## PHP 付環境

- /wiki 配下へのアクセスは `ryu310/pukiwiki-plus` へ Reverse Proxy される
- Index ページは /wiki へのリンクが表示される
- php が利用不可

### 起動方法

```sh
$ curl -o docker-compose.yml https://raw.githubusercontent.com/ryu-sato/http-header-docker-compose/master/stretch/apache/docker-compose.yml
$ docker-compose up -d
```

## カスタマイズ環境(Default コンテンツを追加)

`htdocs/` 配下のファイルを編集して再ビルドする。

```sh
$ git clone https://github.com/ryu-sato/http-header-docker-compose.git && cd http-header-docker-compose
$ vim htdocs/index.html
$ docker-compose up -d --build
```

## カスタマイズ環境(Reverse Proxy 先を追加)

例: /wp にアクセスすると Wordpress を表示する場合。

1. extra ディレクトリに apache のコンフィグファイルを保管する。
1. Dockerfile を編集してコンフィグファイルをコンテナにコピーする。
```text
  : <snip>
# ファイル最下部に下記の行を追加する
ProxyPass /wp http://wp/wp
ProxyPassReverse /wp http://wp/wp/
```
1. docker-compose.yml を編集して wp からIPアドレスを名前解決できるようにする。
    - services 配下に wp の設定を追加する
    - links に wp を追加する
```
version: '2'
services:
  http_header:
    image: 'ryu310/http-header-docker-compose:2.4-01'
    build:
      context: .
      dockerfile: Dockerfile
    volumes:
      - 'http_header_config:/usr/local/apache2/conf'
    ports:
      - 80:80
    links:
      - wiki:wiki
      - wp:wp
    # Enable to restart container (It is useful for Docker for Windows)
    #restart: always
  wp:
    image: 'wordpress'
```

# TODO

- [ ] stretch/apache/Dockerfile を Docker Hub で Autobuild する
