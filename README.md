# Movable Type 開発環境
## 動作環境
* Docker / Docker Compose

## 使い方
### git clone 〜 docker-compose build
```bash
git clone git@github.com:apricoton/docker-movabletype-psgi.git
cd docker-movabletype-psgi
cp .env.example .env
docker-compose build
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
docker exec -it dockermovabletypepsgi_mt_1 tools/run-periodic-tasks
```

#### プラグインを更新したりインストールした場合に反映する
```bash
docker-compose restart mt
```