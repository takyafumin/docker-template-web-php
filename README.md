Docker環境テンプレート：HTML+Javascript+PHP
====================

<!-- @import "[TOC]" {cmd="toc" depthFrom=1 depthTo=6 orderedList=false} -->

<!-- code_chunk_output -->

- [環境](#環境)
- [フォルダ構成](#フォルダ構成)
- [使い方](#使い方)

<!-- /code_chunk_output -->

## 環境

| container | ミドルウェア | バージョン |  ポート  |
| --------- | ------------ | ---------- | -------- |
| app       | apache       | 2.4.51     | 80 -> 80 |
|           | php          | 7.4.20     | なし     |
| node      |              | slim       | なし     |

## フォルダ構成

```
+ .docker/              ・・・docker関連ファイル格納ディレクトリ
|   + apache/           ・・・apache設定
|      + files/         ・・・apache設定用のファイル
|      + resources/     ・・・apache構築用のリソースファイル
|      - Dockerfile     ・・・apache構築用Dockerfile
|
+ app/
|   + public/           ・・・Web公開ディレクトリ(ドキュメントルート)
|   + src/              ・・・プログラム格納ディレクトリ
|   - composer.json     ・・・composer設定ファイル
|   - package.json      ・・・npm設定ファイル
|
- docker-compose.yml
- README.md
```


## 使い方

リポジトリをクローン後, 以下のコマンドを実行してdockerコンテナを起動してください。

```bash
# docker-compose.ymlファイルと同一フォルダ階層で実行
# コンテナが起動する
docker-compose up -d
```

* コンテナの中で作業する(PHP)

```bash
# docker-compose.ymlファイルと同一フォルダ階層で実行
# dockerコンテナが起動している状態で実行
docker-compose exec app bash

# コンテナ内でcomposerやlinuxコマンドが実行できる
ls
php -v

# コンテナから抜ける
exit
```

* コンテナの中で作業する(node)

```bash
# docker-compose.ymlファイルと同一フォルダ階層で実行
# dockerコンテナが起動している状態で実行
docker-compose exec node bash

# コンテナ内でcomposerやlinuxコマンドが実行できる
ls
node -v

# コンテナから抜ける
exit
```

* コンテナの停止方法

```bash
# docker-compose.ymlファイルと同一フォルダ階層で実行
docker-compose down -v
```

## その他情報

* OSバージョン

```bash
bash-4.2# cat /etc/os-release
NAME="Amazon Linux"
VERSION="2"
ID="amzn"
ID_LIKE="centos rhel fedora"
VERSION_ID="2"
PRETTY_NAME="Amazon Linux 2"
ANSI_COLOR="0;33"
CPE_NAME="cpe:2.3:o:amazon:amazon_linux:2"
HOME_URL="https://amazonlinux.com/"
```

* phpバージョン

```bash
bash-4.2# php -v
PHP 7.4.20 (cli) (built: Sep 15 2022 09:40:47) ( ZTS )
Copyright (c) The PHP Group
Zend Engine v3.4.0, Copyright (c) Zend Technologies
```


* phpモジュール

```bash
bash-4.2# php -m
[PHP Modules]
bz2
calendar
Core
ctype
curl
date
dom
exif
fileinfo
filter
ftp
gettext
hash
iconv
json
libxml
mbstring
openssl
pcntl
pcre
PDO
pdo_pgsql
pdo_sqlite
pgsql
Phar
posix
readline
Reflection
session
SimpleXML
sockets
sodium
SPL
sqlite3
standard
tokenizer
xml
xmlreader
xmlwriter
xsl
zlib
```

* composerバージョン

```bash
bash-4.2# composer -V
Composer version 2.4.1 2022-08-20 11:44:50
```

* apacheバージョン

```bash
bash-4.2# httpd -v
Server version: Apache/2.4.51 (Unix)
Server built:   Sep 15 2022 08:38:26
```
