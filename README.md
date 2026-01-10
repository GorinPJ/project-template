# Project Template

Claude Codeを活用したソフトウェア開発プロジェクトの標準テンプレートです。

## 概要

このテンプレートは以下を提供します：

- **サブエージェント定義**: 16個の専門エージェント
- **カスタムコマンド**: エージェント呼び出し用コマンド
- **スキル**: 自動適用されるルール・ガイドライン
- **MCPテンプレート**: 外部ツール連携設定
- **ドキュメント**: カタログ、ガイド、変更履歴

## クイックスタート

### 1. テンプレートのコピー

```bash
# 新規プロジェクトに.claudeディレクトリをコピー
cp -r C:\noguchi\system\projects\project-template\.claude C:\noguchi\system\projects\{your-project}\
```

### 2. CLAUDE.mdの作成

`CLAUDE.md.template`を参考に、プロジェクト固有の`CLAUDE.md`を作成します。

### 3. MCPの設定（必要に応じて）

`.claude/mcp/templates/`から必要なMCPを選び、`.claude/mcp.json`を作成します。

### 4. エージェントの利用

```
/agent-dev-code ログイン機能を実装してください
/agent-test-manager テストを作成してください
```

## テンプレート構成

```
project-template/
├── .claude/
│   ├── agents/           # サブエージェント定義
│   ├── commands/         # カスタムコマンド
│   ├── skills/           # スキル
│   ├── mcp/              # MCP設定
│   └── docs/             # ドキュメント
├── work/
│   └── 作業履歴/         # 作業履歴ディレクトリ
├── CLAUDE.md             # テンプレート説明
├── CLAUDE.md.template    # プロジェクト用テンプレート
├── README.md             # このファイル
└── .gitignore
```

## エージェント一覧

### Meta（3）
- agent-creator: エージェント作成・更新
- work-history-recorder: 作業履歴記録
- template-manager: テンプレート同期

### Document（2）
- doc-manager: ドキュメント管理
- data-manager: データ定義管理

### Development（3）
- code-developer: コード実装
- refactoring-advisor: リファクタリング
- code-reviewer: コードレビュー

### Test（1）
- test-manager: テスト管理

### Analysis（3）
- debug-analyzer: バグ調査
- dependency-analyzer: 依存関係分析
- performance-analyzer: パフォーマンス分析

### Infrastructure（4）
- project-initializer: プロジェクト初期化
- git-workflow-manager: Git操作
- jira-manager: Jira連携
- mcp-developer: MCP開発

## スキル一覧

- coding-standards: コーディング規約
- commit-message: コミットメッセージ規約
- code-review-criteria: レビュー基準
- test-conventions: テスト規約
- documentation-style: ドキュメントスタイル
- doc-management: ドキュメント管理ルール

## MCPテンプレート

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

## ドキュメント

詳細は `.claude/docs/index.md` を参照してください。

## バージョン

- 現在のバージョン: 1.0.0
- 作成日: 2026-01-10

## ライセンス

Private
