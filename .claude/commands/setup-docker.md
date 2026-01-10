Docker環境を対話形式で構築します。

## 概要

プロジェクトに合わせたDocker環境を構築し、必要なファイルを生成します。

## 質問フロー

### 質問1: サービス構成
1. Webアプリ + DB
2. Webアプリ + DB + Redis
3. Webアプリ + DB + Redis + その他
4. カスタム（個別に指定）

### 質問2: データベース
1. PostgreSQL
2. MySQL
3. MongoDB
4. なし

### 質問3: Webアプリのベースイメージ
1. Node.js (20-alpine)
2. PHP (8.3-fpm)
3. Python (3.12-slim)
4. Java (21-eclipse-temurin)
5. Go (1.22-alpine)
6. Ruby (3.3-alpine)
7. その他

### 質問4: Webサーバー
1. Nginx
2. Apache
3. アプリ内蔵（不要）

### 質問5: ポート設定
- Webアプリのポート番号（デフォルト: 8080）
- DBのポート番号（デフォルト: DB種別に応じて設定）

### 質問6: 確認
生成予定のファイル一覧を表示し、確認を求める。

## 生成ファイル

```
{project}/
├── docker/
│   ├── app/
│   │   └── Dockerfile
│   ├── web/
│   │   ├── nginx.conf (または apache.conf)
│   │   └── default.conf
│   └── db/
│       └── my.cnf / postgresql.conf / mongod.conf
├── docker-compose.yml
└── .env.docker.example
```

## 生成例

### docker-compose.yml (PHP + MySQL + Nginx)

```yaml
version: '3.8'

services:
  app:
    build: ./docker/app
    volumes:
      - .:/var/www/html
    depends_on:
      - db

  web:
    image: nginx:alpine
    ports:
      - "${WEB_PORT:-8080}:80"
    volumes:
      - .:/var/www/html
      - ./docker/web/default.conf:/etc/nginx/conf.d/default.conf
    depends_on:
      - app

  db:
    image: mysql:8.0
    environment:
      MYSQL_ROOT_PASSWORD: ${DB_ROOT_PASSWORD}
      MYSQL_DATABASE: ${DB_DATABASE}
      MYSQL_USER: ${DB_USER}
      MYSQL_PASSWORD: ${DB_PASSWORD}
    volumes:
      - db-data:/var/lib/mysql
      - ./docker/db/my.cnf:/etc/mysql/conf.d/my.cnf
    ports:
      - "${DB_PORT:-3306}:3306"

volumes:
  db-data:
```

## 設定の記録

生成完了後、`.claude/project-config.yml` の `docker` セクションを `completed` に更新する。

## 起動確認

生成後、以下のコマンドで起動確認を促す：

```bash
cp .env.docker.example .env
docker compose up -d
docker compose ps
```

---

## 開始

それでは、Docker環境の構築を開始します。
まず、サービス構成を選択してください。
