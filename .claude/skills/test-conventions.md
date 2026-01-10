---
name: test-conventions
description: "テスト作成、テストコード、E2Eテスト、単体テスト時に適用するテスト規約"
---

# テスト規約

テスト作成時は以下の規約に従うこと。

## テストファイル配置

```
tests/
├── unit/                    # 単体テスト
│   └── {module}.test.ts
├── integration/             # 統合テスト
│   └── {feature}.test.ts
└── e2e/                     # E2Eテスト
    ├── specs/
    │   └── {feature}.spec.ts
    └── evidence/            # スクリーンショット
```

## 命名規則

### ファイル名
- `{対象}.test.ts`（単体テスト）
- `{対象}.spec.ts`（E2Eテスト）

### テスト名
- 日本語OK（読みやすさ優先）
- 「〜の場合、〜となること」形式

```typescript
describe('UserService', () => {
  describe('getUser', () => {
    it('存在するユーザーIDの場合、ユーザー情報を返すこと', () => {});
    it('存在しないユーザーIDの場合、nullを返すこと', () => {});
  });
});
```

## テスト構成（AAA パターン）

```typescript
it('テスト名', () => {
  // Arrange: 準備
  const user = createTestUser();

  // Act: 実行
  const result = userService.getUser(user.id);

  // Assert: 検証
  expect(result).toEqual(user);
});
```

## カバレッジ目標

| 種別 | 目標 |
|------|------|
| 単体テスト | 80%以上 |
| 統合テスト | 主要フロー |
| E2Eテスト | クリティカルパス |

## E2Eテスト（Playwright）

### 基本構成

```typescript
import { test, expect } from '@playwright/test';

test.describe('ログイン機能', () => {
  test.beforeEach(async ({ page }) => {
    await page.goto('/login');
  });

  test('正しい認証情報でログインできること', async ({ page }) => {
    // 入力
    await page.fill('[name="email"]', 'test@example.com');
    await page.fill('[name="password"]', 'password123');

    // スクリーンショット（入力後）
    await page.screenshot({ path: 'evidence/login-input.png' });

    // 送信
    await page.click('button[type="submit"]');

    // 検証
    await expect(page).toHaveURL('/dashboard');

    // スクリーンショット（完了後）
    await page.screenshot({ path: 'evidence/login-success.png' });
  });
});
```

### スクリーンショット取得タイミング

- テスト開始時（初期状態）
- 主要操作後
- エラー発生時
- テスト完了時

### ページオブジェクトパターン

```typescript
// pages/login.page.ts
export class LoginPage {
  constructor(private page: Page) {}

  async login(email: string, password: string) {
    await this.page.fill('[name="email"]', email);
    await this.page.fill('[name="password"]', password);
    await this.page.click('button[type="submit"]');
  }
}
```

## テストデータ

### 原則
- テストごとに独立したデータを使用
- 本番データを使用しない
- 固定値より動的生成を優先

```typescript
// ファクトリ関数
function createTestUser(overrides = {}) {
  return {
    id: `user-${Date.now()}`,
    name: 'テストユーザー',
    email: `test-${Date.now()}@example.com`,
    ...overrides,
  };
}
```

## 避けるべきこと

- テスト間の依存
- 固定のsleep/wait（明示的な待機を使用）
- 本番環境への接続
- 機密情報のハードコーディング
