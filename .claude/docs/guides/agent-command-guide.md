# エージェント・コマンドガイド

サブエージェントの効果的な使い方を説明します。

## 基本的な使い方

### カスタムコマンドで呼び出し

```
/agent-{カテゴリ}-{名前} {指示内容}
```

**例**:
```
/agent-dev-code ユーザー認証機能を実装してください
/agent-dev-review 直前のコミットをレビューしてください
/agent-test-manager 認証機能のユニットテストを作成してください
```

### 直接指示

```
@{エージェント名} {指示内容}
```

**例**:
```
@code-developer ログイン画面を作成してください
@test-manager E2Eテストを実行してください
```

## カテゴリ別コマンド一覧

### Meta（メタ）
| コマンド | 用途 |
|---------|------|
| `/agent-meta-create` | エージェント作成・更新 |
| `/agent-meta-history` | 作業履歴記録 |
| `/agent-meta-template` | テンプレート同期 |

### Document（ドキュメント）
| コマンド | 用途 |
|---------|------|
| `/agent-doc-manager` | ドキュメント管理 |
| `/agent-doc-data` | データ定義管理 |

### Development（開発）
| コマンド | 用途 |
|---------|------|
| `/agent-dev-code` | コード実装 |
| `/agent-dev-refactor` | リファクタリング |
| `/agent-dev-review` | コードレビュー |

### Test（テスト）
| コマンド | 用途 |
|---------|------|
| `/agent-test-manager` | テスト管理全般 |

### Analysis（分析）
| コマンド | 用途 |
|---------|------|
| `/agent-analyze-debug` | バグ調査 |
| `/agent-analyze-deps` | 依存関係分析 |
| `/agent-analyze-perf` | パフォーマンス分析 |

### Infrastructure（インフラ）
| コマンド | 用途 |
|---------|------|
| `/agent-infra-init` | プロジェクト初期化 |
| `/agent-infra-git` | Git操作 |
| `/agent-infra-jira` | Jira管理 |
| `/agent-infra-mcp` | MCP開発 |

## 実践的な使用例

### 新機能開発フロー

```
# 1. 機能実装
/agent-dev-code ユーザープロフィール編集機能を実装してください

# 2. テスト作成
/agent-test-manager プロフィール編集のテストを作成してください

# 3. コードレビュー
/agent-dev-review プロフィール機能のコードをレビューしてください

# 4. 作業履歴記録
/agent-meta-history 本日のプロフィール機能開発作業を記録してください
```

### バグ修正フロー

```
# 1. 原因調査
/agent-analyze-debug ログイン時にエラーが発生する問題を調査してください

# 2. 修正実装
/agent-dev-code 調査結果に基づいてバグを修正してください

# 3. テスト確認
/agent-test-manager 修正後のリグレッションテストを実行してください
```

### リファクタリングフロー

```
# 1. 分析・提案
/agent-dev-refactor AuthServiceクラスのリファクタリングを提案してください

# 2. 依存関係確認
/agent-analyze-deps AuthServiceの依存関係を分析してください

# 3. 実装
/agent-dev-code 提案されたリファクタリングを実施してください

# 4. レビュー
/agent-dev-review リファクタリング結果をレビューしてください
```

### プロジェクト初期化フロー

```
# 1. プロジェクト作成
/agent-infra-init 新しいNode.jsプロジェクトを初期化してください

# 2. Git設定
/agent-infra-git GitHubリポジトリを作成し接続してください

# 3. ドキュメント作成
/agent-doc-manager README.mdとCONTRIBUTING.mdを作成してください
```

## エージェント連携のコツ

### 1. 明確な指示を与える

**良い例**:
```
/agent-dev-code src/auth/login.tsにパスワードリセット機能を追加してください。
メールでリセットリンクを送信し、24時間有効とします。
```

**悪い例**:
```
/agent-dev-code パスワード関連の機能を作って
```

### 2. コンテキストを共有する

```
# 前の作業結果を参照させる
/agent-dev-review 先ほど実装したログイン機能をレビューしてください
```

### 3. 段階的に進める

複雑なタスクは分割して指示：

```
# Step 1: 設計
/agent-dev-code まずAPIの設計を提案してください

# Step 2: 確認後に実装
/agent-dev-code 設計に基づいてAPIを実装してください
```

## トラブルシューティング

### エージェントが意図と異なる動作をする場合

1. 指示を具体的に書き直す
2. 期待する出力形式を明示する
3. 制約条件を追加する

### エージェント定義を確認したい場合

```
# エージェント定義ファイルを読む
Read .claude/agents/development/code-developer.md
```

### 新しいエージェントが必要な場合

```
/agent-meta-create セキュリティ監査専用のエージェントを作成してください
```
