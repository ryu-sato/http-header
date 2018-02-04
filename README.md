# http-header 概要

サブディレクトリ毎に HTTP サーバへリバースプロクシする環境における HTTP ヘッダとなるコンテナ。

# 使い方

## デフォルト環境

### 環境説明

- /wiki 配下へのアクセスは `ryu310/pukiwiki-plus:7-apache-1.4.7-02` へ Reverse Proxy する
- その他アクセスは Index ページを表示する(コンテンツは空)

### インストールと起動
```sh
$ git clone ★
$ docker-compose up -d
```

インストール後、`http://localhost/` にアクセスすると `Index of /` が表示される。
また、`http://localhost/wiki/` にアクセスすると pukiwiki が表示される。

## カスタマイズ環境(Default コンテンツを追加)

## カスタマイズ環境(Reverse Proxy 先を追加)

例: /wp にアクセスすると Wordpress を表示する場合。

1. extra ディレクトリに apache のコンフィグファイルを保管する。
1. Dockerfile を編集してコンフィグファイルをコンテナにコピーする。
```text
  : <snip>
# ファイル最下部に下記の行を追加する
ProxyPass /wp http://wp/
ProxyPassReverse /wp http://wp/
```
1. docker-compose.yml を編集して wp からIPアドレスを名前解決できるようにする。
    - services 配下に wp の設定を追加する
    - links に wp を追加する
```
version: '2'
services:
  http_header:
    image: 'ryu310/http-header:2.4-01'
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
