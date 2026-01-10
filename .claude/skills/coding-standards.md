---
name: coding-standards
description: "コード作成、実装、開発、変数命名、関数作成、クラス設計時に適用するコーディング規約"
---

# コーディング規約

コード作成時は以下の規約に従うこと。

## 命名規則

### 変数名
- **camelCase** を使用
- 意味のある名前をつける
- 略語は避ける（一般的なものを除く）

```javascript
// Good
const userName = "John";
const itemCount = 10;

// Bad
const un = "John";
const cnt = 10;
```

### 関数名
- **camelCase** を使用
- 動詞で開始
- 何をするか明確に

```javascript
// Good
function getUserById(id) {}
function calculateTotal(items) {}

// Bad
function user(id) {}
function total(items) {}
```

### クラス名
- **PascalCase** を使用
- 名詞を使用

```javascript
// Good
class UserController {}
class OrderService {}

// Bad
class userController {}
class handleOrder {}
```

### 定数
- **UPPER_SNAKE_CASE** を使用

```javascript
// Good
const MAX_RETRY_COUNT = 3;
const API_BASE_URL = "https://api.example.com";

// Bad
const maxRetryCount = 3;
```

### ファイル名
- **kebab-case** を使用

```
// Good
user-controller.js
order-service.ts

// Bad
userController.js
OrderService.ts
```

## コードスタイル

### インデント
- スペース2つ（または4つ、プロジェクトに従う）

### 文字列
- シングルクォート優先（プロジェクトに従う）

### セミコロン
- プロジェクトの規約に従う

### 行の長さ
- 80〜120文字を目安

## 関数設計

### 原則
- 1つの関数は1つのことを行う
- 引数は3つ以下を推奨
- 早期リターンを活用

```javascript
// Good: 早期リターン
function processUser(user) {
  if (!user) return null;
  if (!user.isActive) return null;

  return doProcess(user);
}

// Bad: ネストが深い
function processUser(user) {
  if (user) {
    if (user.isActive) {
      return doProcess(user);
    }
  }
  return null;
}
```

## コメント

### 書くべきコメント
- なぜその実装にしたか（Why）
- 複雑なロジックの説明
- TODO、FIXME

### 避けるべきコメント
- 何をしているか（コードを読めばわかる）
- 変更履歴（Gitで管理）

```javascript
// Good: なぜかを説明
// ブラウザの互換性のため、古いAPIを使用
const result = legacyApi.call();

// Bad: 何をしているかを説明
// ユーザーを取得する
const user = getUser();
```

## エラーハンドリング

- 例外は適切にキャッチ
- エラーメッセージは具体的に
- ログを残す

```javascript
try {
  await saveUser(user);
} catch (error) {
  logger.error('ユーザー保存失敗', { userId: user.id, error });
  throw new UserSaveError('ユーザーの保存に失敗しました');
}
```

## セキュリティ

- ユーザー入力は必ずバリデーション
- SQLはプリペアドステートメントを使用
- 機密情報をハードコーディングしない
- XSS対策（出力時のエスケープ）
