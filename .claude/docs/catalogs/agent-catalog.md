# エージェントカタログ

利用可能なサブエージェント一覧です。

## カテゴリ別一覧

### Meta（メタ）- 3エージェント

| エージェント | コマンド | 説明 |
|------------|---------|------|
| agent-creator | `/agent-meta-create` | 新規エージェント定義の作成・既存エージェントの更新 |
| work-history-recorder | `/agent-meta-history` | 作業履歴の記録・整理 |
| template-manager | `/agent-meta-template` | テンプレートのバージョン管理・プロジェクト間同期 |

### Document（ドキュメント）- 2エージェント

| エージェント | コマンド | 説明 |
|------------|---------|------|
| doc-manager | `/agent-doc-manager` | ドキュメント作成・更新・整合性チェック |
| data-manager | `/agent-doc-data` | データ定義・スキーマ管理・マスタデータ管理 |

### Development（開発）- 3エージェント

| エージェント | コマンド | 説明 |
|------------|---------|------|
| code-developer | `/agent-dev-code` | 機能実装・コード作成 |
| refactoring-advisor | `/agent-dev-refactor` | リファクタリング提案・実行 |
| code-reviewer | `/agent-dev-review` | コードレビュー・品質チェック |

### Test（テスト）- 1エージェント

| エージェント | コマンド | 説明 |
|------------|---------|------|
| test-manager | `/agent-test-manager` | テスト戦略策定・テストコード作成・実行管理 |

### Analysis（分析）- 3エージェント

| エージェント | コマンド | 説明 |
|------------|---------|------|
| debug-analyzer | `/agent-analyze-debug` | バグ調査・原因分析・修正提案 |
| dependency-analyzer | `/agent-analyze-deps` | 依存関係分析・更新影響調査 |
| performance-analyzer | `/agent-analyze-perf` | パフォーマンス分析・ボトルネック特定 |

### Infrastructure（インフラ）- 4エージェント

| エージェント | コマンド | 説明 |
|------------|---------|------|
| project-initializer | `/agent-infra-init` | プロジェクト初期設定・環境構築 |
| git-workflow-manager | `/agent-infra-git` | Git操作・ブランチ管理・コンフリクト解決 |
| jira-manager | `/agent-infra-jira` | Jiraチケット管理・ステータス更新 |
| mcp-developer | `/agent-infra-mcp` | MCPサーバー開発・設定 |

## 使用方法

### カスタムコマンドで呼び出し
```
/agent-dev-code ログイン機能を実装してください
```

### 直接指示
```
@code-developer ログイン機能を実装してください
```

### 複数エージェントの連携例
```
1. /agent-dev-code 機能実装
2. /agent-dev-review コードレビュー
3. /agent-test-manager テスト作成
4. /agent-meta-history 作業履歴記録
```

## エージェント定義ファイル

各エージェントの詳細な定義は以下のパスにあります：

```
.claude/agents/
├── common/shared-rules.md      # 全エージェント共通ルール
├── meta/
│   ├── agent-creator.md
│   ├── work-history-recorder.md
│   └── template-manager.md
├── document/
│   ├── doc-manager.md
│   └── data-manager.md
├── development/
│   ├── code-developer.md
│   ├── refactoring-advisor.md
│   └── code-reviewer.md
├── test/
│   └── test-manager.md
├── analysis/
│   ├── debug-analyzer.md
│   ├── dependency-analyzer.md
│   └── performance-analyzer.md
└── infrastructure/
    ├── project-initializer.md
    ├── git-workflow-manager.md
    ├── jira-manager.md
    └── mcp-developer.md
```

## カスタムエージェントの追加

新しいエージェントを追加する場合は、`/agent-meta-create` を使用してください。

```
/agent-meta-create セキュリティ監査用のエージェントを作成してください
```

作成後は、このカタログも更新してください。
