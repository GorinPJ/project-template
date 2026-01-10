# CHANGELOG

このテンプレートの変更履歴です。

## [1.0.0] - 2026-01-10

### 初期リリース

テンプレートの初期バージョンをリリース。

#### エージェント（16個）

**Meta（3）**
- agent-creator: エージェント作成・更新
- work-history-recorder: 作業履歴記録
- template-manager: テンプレート同期管理

**Document（2）**
- doc-manager: ドキュメント管理
- data-manager: データ定義管理

**Development（3）**
- code-developer: コード実装
- refactoring-advisor: リファクタリング支援
- code-reviewer: コードレビュー

**Test（1）**
- test-manager: テスト管理

**Analysis（3）**
- debug-analyzer: バグ調査
- dependency-analyzer: 依存関係分析
- performance-analyzer: パフォーマンス分析

**Infrastructure（4）**
- project-initializer: プロジェクト初期化
- git-workflow-manager: Git操作
- jira-manager: Jira連携
- mcp-developer: MCP開発

#### スキル（6個）
- coding-standards: コーディング規約
- commit-message: コミットメッセージ規約
- code-review-criteria: レビュー基準
- test-conventions: テスト規約
- documentation-style: ドキュメントスタイル
- doc-management: ドキュメント管理ルール

#### MCPテンプレート（13個）
- atlassian: Jira/Confluence
- context7: ライブラリドキュメント
- drawio: 図表作成
- excel: Excel操作
- fetch: Web取得
- filesystem: ファイル操作
- github: GitHub連携
- google-drive: Googleドライブ
- google-sheets: スプレッドシート
- mysql: MySQL
- postgres: PostgreSQL
- puppeteer: ブラウザ自動操作
- slack: Slack連携

#### ドキュメント
- index.md: 目次
- agent-catalog.md: エージェント一覧
- mcp-catalog.md: MCP一覧
- skill-catalog.md: スキル一覧
- project-registry.md: プロジェクト登録
- agent-command-guide.md: 使い方ガイド
- project-guidelines.md: 運用ガイドライン

---

## 変更履歴の書き方

### バージョニング

セマンティックバージョニングを使用：
- **メジャー (X.0.0)**: 破壊的変更
- **マイナー (0.X.0)**: 機能追加（後方互換性あり）
- **パッチ (0.0.X)**: バグ修正

### エントリ形式

```markdown
## [X.Y.Z] - YYYY-MM-DD

### Added（追加）
- 新機能の説明

### Changed（変更）
- 変更内容の説明

### Deprecated（非推奨）
- 非推奨になった機能

### Removed（削除）
- 削除された機能

### Fixed（修正）
- バグ修正の説明

### Security（セキュリティ）
- セキュリティ関連の修正
```

### 変更詳細ファイル

大きな変更は `changes/` ディレクトリに詳細ファイルを作成：

```
.claude/docs/changelog/
├── CHANGELOG.md           # メインの変更履歴
└── changes/
    ├── 1.0.0-initial.md   # 各バージョンの詳細
    └── ...
```
