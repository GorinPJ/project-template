# Docker環境構築ガイド

新規プロジェクトでDocker環境を構築する際のフローと決定事項をまとめます。

## 構築フロー

```
1. ポート番号の確認・割り当て
       ↓
2. docker-compose.yml 作成
       ↓
3. 各サービスの設定ファイル作成
       ↓
4. 環境変数ファイル作成
       ↓
5. Makefile 作成
       ↓
6. ポート管理ファイル更新
       ↓
7. ドキュメント更新
```

---

## 1. ポート番号の確認・割り当て

### 確認手順

1. `C:\noguchi\system\docker\ポート.txt` を確認
2. 空いているポート番号を特定
3. プロジェクトに必要なサービスごとにポートを割り当て

### ポート番号の割り当てルール

| サービス | ポート範囲 | 説明 |
|---------|-----------|------|
| Webサーバー (Nginx/Apache) | 8081〜8099 | メインのHTTPアクセス |
| MySQL | 3306〜3319 | データベース |
| phpMyAdmin | 8880〜8899 | DB管理ツール |
| Node.js/Frontend | 5173〜5189 | フロントエンド開発サーバー |
| Redis | 6379〜6389 | キャッシュ（必要な場合） |
| その他 | 適宜 | プロジェクト固有 |

### 割り当て例

```
プロジェクト: shopify-hit-union
- Webサーバー: 8092
- MySQL: 3307
- phpMyAdmin: 8892
- Node.js: 5173
```

---

## 2. docker-compose.yml 作成

### 標準構成（SPA + API）

```yaml
version: '3.8'

services:
  # Nginx - Webサーバー
  nginx:
    image: nginx:alpine
    container_name: {project-name}-nginx
    ports:
      - "{WEB_PORT}:80"
    volumes:
      - ./backend:/var/www/html
      - ./docker/nginx/default.conf:/etc/nginx/conf.d/default.conf
    depends_on:
      - php
    networks:
      - app-network

  # PHP-FPM - バックエンド
  php:
    build:
      context: ./docker/php
      dockerfile: Dockerfile
    container_name: {project-name}-php
    volumes:
      - ./backend:/var/www/html
    networks:
      - app-network

  # MySQL - データベース
  mysql:
    image: mysql:8.0
    container_name: {project-name}-mysql
    ports:
      - "{MYSQL_PORT}:3306"
    environment:
      MYSQL_ROOT_PASSWORD: ${DB_ROOT_PASSWORD:-root}
      MYSQL_DATABASE: ${DB_DATABASE:-app}
      MYSQL_USER: ${DB_USERNAME:-app}
      MYSQL_PASSWORD: ${DB_PASSWORD:-secret}
    volumes:
      - mysql-data:/var/lib/mysql
      - ./docker/mysql/init:/docker-entrypoint-initdb.d
    networks:
      - app-network

  # Node.js - フロントエンド開発サーバー
  node:
    image: node:20-alpine
    container_name: {project-name}-node
    working_dir: /app
    ports:
      - "{NODE_PORT}:5173"
    volumes:
      - ./frontend:/app
    command: sh -c "npm install && npm run dev -- --host 0.0.0.0"
    networks:
      - app-network

  # phpMyAdmin - DB管理ツール（オプション）
  phpmyadmin:
    image: phpmyadmin:latest
    container_name: {project-name}-pma
    ports:
      - "{PMA_PORT}:80"
    environment:
      PMA_HOST: mysql
      PMA_USER: root
      PMA_PASSWORD: ${DB_ROOT_PASSWORD:-root}
    depends_on:
      - mysql
    networks:
      - app-network

networks:
  app-network:
    driver: bridge

volumes:
  mysql-data:
```

### 命名規則

- コンテナ名: `{project-name}-{service}`
  - 例: `shopify-hit-union-nginx`, `shopify-hit-union-mysql`
- ネットワーク名: `app-network`（プロジェクト内で共有）
- ボリューム名: `{service}-data`
  - 例: `mysql-data`

---

## 3. 各サービスの設定ファイル作成

### ディレクトリ構成

```
docker/
├── php/
│   └── Dockerfile
├── nginx/
│   └── default.conf
└── mysql/
    └── init/
        └── 01_init.sql
```

### PHP Dockerfile（Laravel用）

```dockerfile
FROM php:8.2-fpm

RUN apt-get update && apt-get install -y \
    git curl libpng-dev libonig-dev libxml2-dev libzip-dev zip unzip \
    && rm -rf /var/lib/apt/lists/*

RUN docker-php-ext-install pdo_mysql mbstring exif pcntl bcmath gd zip

COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

WORKDIR /var/www/html

RUN chown -R www-data:www-data /var/www/html

RUN echo "upload_max_filesize = 64M" >> /usr/local/etc/php/conf.d/custom.ini \
    && echo "post_max_size = 64M" >> /usr/local/etc/php/conf.d/custom.ini \
    && echo "memory_limit = 256M" >> /usr/local/etc/php/conf.d/custom.ini

EXPOSE 9000
CMD ["php-fpm"]
```

### Nginx設定（Laravel用）

```nginx
server {
    listen 80;
    server_name localhost;
    root /var/www/html/public;
    index index.php index.html;

    client_max_body_size 64M;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php$ {
        fastcgi_pass php:9000;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        include fastcgi_params;
    }

    location ~ /\. {
        deny all;
    }
}
```

---

## 4. 環境変数ファイル作成

### .env.example

```bash
# Application
APP_NAME={project-name}
APP_ENV=local
APP_DEBUG=true
APP_URL=http://localhost:{WEB_PORT}

# Database (Docker)
DB_CONNECTION=mysql
DB_HOST=mysql
DB_PORT=3306
DB_DATABASE={db_name}
DB_USERNAME=app
DB_PASSWORD=secret
DB_ROOT_PASSWORD=root

# Frontend
VITE_API_URL=http://localhost:{WEB_PORT}/api
```

---

## 5. Makefile 作成

### 標準Makefile

```makefile
.PHONY: help up down restart build logs ps shell mysql composer artisan npm init clean

help:
	@echo "使用可能なコマンド:"
	@echo "  make up        - コンテナ起動"
	@echo "  make down      - コンテナ停止"
	@echo "  make restart   - コンテナ再起動"
	@echo "  make build     - イメージをビルド"
	@echo "  make logs      - ログ表示"
	@echo "  make shell     - PHPコンテナにシェル接続"
	@echo "  make mysql     - MySQLに接続"
	@echo "  make init      - 初期セットアップ"
	@echo "  make clean     - コンテナ・ボリュームを削除"

up:
	docker-compose up -d

down:
	docker-compose down

restart:
	docker-compose restart

build:
	docker-compose build --no-cache

logs:
	docker-compose logs -f

ps:
	docker-compose ps

shell:
	docker-compose exec php bash

mysql:
	docker-compose exec mysql mysql -u root -proot {db_name}

composer:
	docker-compose exec php composer $(CMD)

artisan:
	docker-compose exec php php artisan $(CMD)

npm:
	docker-compose exec node npm $(CMD)

init:
	docker-compose build
	docker-compose up -d
	sleep 30
	docker-compose exec php composer install
	docker-compose exec php cp .env.example .env
	docker-compose exec php php artisan key:generate
	docker-compose exec php php artisan migrate
	docker-compose exec node npm install

clean:
	docker-compose down -v --rmi all
```

---

## 6. ポート管理ファイル更新

### 更新手順

1. `C:\noguchi\system\docker\ポート.txt` を開く
2. 割り当てたポートを追記
3. サービス別に整理

### フォーマット

```
=== Webサーバー (8081〜) ===
8081：pentoys
8092：shopify-hit-union

=== MySQL (3306〜) ===
3306：（デフォルト/共有）
3307：shopify-hit-union

=== phpMyAdmin (8880〜) ===
8892：shopify-hit-union

=== Node.js/Frontend (5173〜) ===
5173：shopify-hit-union
```

---

## 7. ドキュメント更新

### 更新対象

1. **README.md** - Docker起動方法、アクセスURL
2. **Confluence** - 環境構築手順ページ
3. **CLAUDE.md** - 開発コマンド

### README.md に記載する内容

```markdown
## Docker環境

### 起動

docker-compose up -d

### アクセスURL

| サービス | URL |
|---------|-----|
| API | http://localhost:{WEB_PORT} |
| Frontend | http://localhost:{NODE_PORT} |
| phpMyAdmin | http://localhost:{PMA_PORT} |
```

---

## チェックリスト

- [ ] ポート番号を確認・割り当て
- [ ] docker-compose.yml 作成
- [ ] docker/php/Dockerfile 作成
- [ ] docker/nginx/default.conf 作成
- [ ] docker/mysql/init/01_init.sql 作成
- [ ] .env.example 更新
- [ ] Makefile 作成
- [ ] ポート.txt 更新
- [ ] README.md 更新
- [ ] Confluence更新（任意）

---

## 注意事項

1. **コンテナ名の重複**: 他プロジェクトと被らないようプロジェクト名をプレフィックスに使用
2. **ポートの競合**: 必ず `ポート.txt` を確認してから割り当て
3. **ボリュームの永続化**: MySQLデータは名前付きボリュームで永続化
4. **本番環境との差異**: 本番がWindows Server (IIS) の場合、cronはタスクスケジューラに置き換え

---

**最終更新**: 2026-01-11
