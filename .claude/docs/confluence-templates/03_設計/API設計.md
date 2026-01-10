# API設計

## 概要

{API設計の概要を記載}

---

## 基本仕様

| 項目 | 内容 |
|------|------|
| ベースURL | `https://api.example.com/v1` |
| プロトコル | HTTPS |
| 認証方式 | Bearer Token (JWT) |
| コンテンツタイプ | application/json |
| 文字コード | UTF-8 |

---

## 共通ヘッダー

### リクエスト

| ヘッダー | 必須 | 説明 |
|---------|------|------|
| Authorization | ○ | `Bearer {token}` |
| Content-Type | ○ | `application/json` |
| Accept | ○ | `application/json` |
| X-Request-ID | × | リクエスト追跡用ID |

### レスポンス

| ヘッダー | 説明 |
|---------|------|
| X-Request-ID | リクエスト追跡用ID |
| X-RateLimit-Limit | レート制限上限 |
| X-RateLimit-Remaining | レート制限残り |

---

## 共通レスポンス形式

### 成功時

```json
{
  "success": true,
  "data": {
    // レスポンスデータ
  },
  "meta": {
    "timestamp": "2024-01-01T00:00:00Z"
  }
}
```

### 一覧取得時（ページネーション）

```json
{
  "success": true,
  "data": [
    // 配列データ
  ],
  "meta": {
    "current_page": 1,
    "per_page": 20,
    "total": 100,
    "last_page": 5
  }
}
```

### エラー時

```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "入力内容に誤りがあります",
    "details": [
      {
        "field": "email",
        "message": "メールアドレスの形式が正しくありません"
      }
    ]
  }
}
```

---

## HTTPステータスコード

| コード | 意味 | 使用場面 |
|-------|------|---------|
| 200 | OK | 取得・更新成功 |
| 201 | Created | 作成成功 |
| 204 | No Content | 削除成功 |
| 400 | Bad Request | リクエスト不正 |
| 401 | Unauthorized | 認証エラー |
| 403 | Forbidden | 権限エラー |
| 404 | Not Found | リソース不在 |
| 422 | Unprocessable Entity | バリデーションエラー |
| 429 | Too Many Requests | レート制限超過 |
| 500 | Internal Server Error | サーバーエラー |

---

## エラーコード一覧

| コード | 説明 | HTTPステータス |
|-------|------|--------------|
| VALIDATION_ERROR | バリデーションエラー | 422 |
| AUTHENTICATION_ERROR | 認証エラー | 401 |
| AUTHORIZATION_ERROR | 権限エラー | 403 |
| NOT_FOUND | リソースが見つからない | 404 |
| CONFLICT | 競合エラー | 409 |
| RATE_LIMIT_EXCEEDED | レート制限超過 | 429 |
| INTERNAL_ERROR | 内部エラー | 500 |

---

## API一覧

### 認証

| メソッド | パス | 概要 | 認証 |
|---------|-----|------|------|
| POST | /auth/login | ログイン | 不要 |
| POST | /auth/logout | ログアウト | 必要 |
| POST | /auth/refresh | トークン更新 | 必要 |
| POST | /auth/password/reset | パスワードリセット | 不要 |

### ユーザー

| メソッド | パス | 概要 | 認証 |
|---------|-----|------|------|
| GET | /users | ユーザー一覧 | 必要 |
| GET | /users/{id} | ユーザー詳細 | 必要 |
| POST | /users | ユーザー作成 | 必要 |
| PUT | /users/{id} | ユーザー更新 | 必要 |
| DELETE | /users/{id} | ユーザー削除 | 必要 |
| GET | /users/me | 自分の情報 | 必要 |

### 商品

| メソッド | パス | 概要 | 認証 |
|---------|-----|------|------|
| GET | /products | 商品一覧 | 不要 |
| GET | /products/{id} | 商品詳細 | 不要 |
| POST | /products | 商品作成 | 必要 |
| PUT | /products/{id} | 商品更新 | 必要 |
| DELETE | /products/{id} | 商品削除 | 必要 |

---

## API詳細

### POST /auth/login

ユーザー認証を行い、アクセストークンを取得する。

#### リクエスト

```json
{
  "email": "user@example.com",
  "password": "password123"
}
```

| パラメータ | 型 | 必須 | 説明 |
|-----------|---|------|------|
| email | string | ○ | メールアドレス |
| password | string | ○ | パスワード |

#### レスポンス（成功）

```json
{
  "success": true,
  "data": {
    "access_token": "eyJhbGciOiJIUzI1NiIs...",
    "refresh_token": "eyJhbGciOiJIUzI1NiIs...",
    "token_type": "Bearer",
    "expires_in": 3600
  }
}
```

#### レスポンス（エラー）

```json
{
  "success": false,
  "error": {
    "code": "AUTHENTICATION_ERROR",
    "message": "メールアドレスまたはパスワードが正しくありません"
  }
}
```

---

### GET /users

ユーザー一覧を取得する。

#### リクエストパラメータ

| パラメータ | 型 | 必須 | 説明 | デフォルト |
|-----------|---|------|------|-----------|
| page | int | × | ページ番号 | 1 |
| per_page | int | × | 1ページあたり件数 | 20 |
| sort | string | × | ソート項目 | created_at |
| order | string | × | ソート順 | desc |
| status | string | × | ステータス絞り込み | - |
| q | string | × | 検索キーワード | - |

#### レスポンス

```json
{
  "success": true,
  "data": [
    {
      "id": 1,
      "name": "山田太郎",
      "email": "yamada@example.com",
      "role": "user",
      "status": "active",
      "created_at": "2024-01-01T00:00:00Z"
    }
  ],
  "meta": {
    "current_page": 1,
    "per_page": 20,
    "total": 100,
    "last_page": 5
  }
}
```

---

### POST /users

新規ユーザーを作成する。

#### リクエスト

```json
{
  "name": "山田太郎",
  "email": "yamada@example.com",
  "password": "password123",
  "role": "user"
}
```

| パラメータ | 型 | 必須 | 説明 | バリデーション |
|-----------|---|------|------|--------------|
| name | string | ○ | 名前 | 1-100文字 |
| email | string | ○ | メールアドレス | メール形式、重複不可 |
| password | string | ○ | パスワード | 8文字以上 |
| role | string | × | 権限 | admin, user |

#### レスポンス

```json
{
  "success": true,
  "data": {
    "id": 1,
    "name": "山田太郎",
    "email": "yamada@example.com",
    "role": "user",
    "status": "active",
    "created_at": "2024-01-01T00:00:00Z"
  }
}
```

---

**最終更新**: YYYY-MM-DD
**更新者**: {名前}
