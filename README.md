# Movable Type 開発環境
## 動作環境
* Docker 1.13.0+ / Docker Compose

## 立ち上がる環境
* Apache 2.4 + PSGI + MySQL

## 使い方
### git clone 〜 docker-compose build
```bash
git clone git@github.com:apricoton/docker-movabletype-psgi.git
cd docker-movabletype-psgi
cp .env.example .env
docker-compose build --no-cache
```

### Movable Type を下記のように設置
```
docker-movabletype-psgi/
└ src/
    ├ mt/ Movable Type 本体ディレクトリ
    └ html/ webroot
         └ mt-static/ シンボリックリンク or コピー
```

### mt-config.cgi
```perl
CGIPath        /mt/
StaticWebPath  /mt-static/
StaticFilePath /var/www/html/mt-static

ObjectDriver DBI::mysql
Database movabletype
DBUser movabletype
DBPassword movabletype
DBHost db

DefaultLanguage ja

PIDFilePath /var/run/mt.pid
```

### 実行
```bash
docker-compose up -d
```

### 管理画面にアクセス
* http://localhost:8000/mt/mt.cgi
    * 「最初のウェブサイトを作成」画面
        * ウェブサイトURL : http://localhost:8000/
        * ウェブサイトパス : /var/www/html

### Tips
#### toolsのスクリプトを実行する
```bash
docker-compose exec --user docker mt /var/www/mt/tools/upgrade --name Melody
docker-compose exec --user docker mt /var/www/mt/tools/run-periodic-tasks
```

#### MTの再起動
```bash
docker-compose restart mt
```

#### .env
```bash
MOVABLETYPE_UID_MIN=1000           # アプリケーションを動かす User ID
MOVABLETYPE_GID_MIN=1000           # アプリケーションを動かす Group ID
MOVABLETYPE_WEB_PORT=8000          # ウェブサーバの外向けのポート（ http://localhost:8000/ ）
MOVABLETYPE_APP_PORT=5000          # アプリケーションサーバのポート
MOVABLETYPE_DOC_ROOT=/var/www/html # ドキュメントルートのコンテナ内部パス（デフォルトでは ./src が /var/www になっている）
MOVABLETYPE_WORK_DIR=/var/www/mt   # MTインストールディレクトリのコンテナ内部パス
```