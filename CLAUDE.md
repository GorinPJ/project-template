# CLAUDE.md

このファイルは、Claude Code (claude.ai/code) がこのリポジトリで作業する際のガイダンスを提供します。

## プロジェクト概要

これはプロジェクトテンプレートリポジトリです。
新規プロジェクト作成時に `.claude/` ディレクトリをコピーして使用します。

## テンプレート構成

```
.claude/
├── agents/           # サブエージェント定義（16個）
│   ├── common/       # 共通ルール
│   ├── meta/         # メタエージェント
│   ├── document/     # ドキュメント管理
│   ├── development/  # 開発支援
│   ├── test/         # テスト管理
│   ├── analysis/     # 分析・調査
│   └── infrastructure/  # インフラ・運用
├── commands/         # カスタムコマンド（16個）
├── skills/           # スキル（6個）
├── mcp/              # MCP設定テンプレート
│   └── templates/    # 各MCP設定（13個）
└── docs/             # ドキュメント
    ├── catalogs/     # カタログ類
    ├── guides/       # ガイド
    ├── changelog/    # 変更履歴
    └── design/       # 設計ドキュメント
```

## 使い方

### 新規プロジェクトへの適用

```bash
# .claude ディレクトリをコピー
cp -r C:\noguchi\system\projects\project-template\.claude C:\noguchi\system\projects\{new-project}\

# プロジェクト固有のCLAUDE.mdを作成
# CLAUDE.md.templateを参考にカスタマイズ
```

### エージェントの利用

```
# カスタムコマンドで呼び出し
/agent-dev-code 機能を実装してください
/agent-test-manager テストを作成してください

# 直接指示
@code-developer ログイン機能を実装してください
```

### テンプレート更新の同期

```
# 登録プロジェクトに更新を反映
/agent-meta-template テンプレートの更新を全プロジェクトに同期してください
```

## エージェント一覧

| カテゴリ | エージェント | コマンド |
|---------|------------|---------|
| Meta | agent-creator | /agent-meta-create |
| Meta | work-history-recorder | /agent-meta-history |
| Meta | template-manager | /agent-meta-template |
| Document | doc-manager | /agent-doc-manager |
| Document | data-manager | /agent-doc-data |
| Development | code-developer | /agent-dev-code |
| Development | refactoring-advisor | /agent-dev-refactor |
| Development | code-reviewer | /agent-dev-review |
| Test | test-manager | /agent-test-manager |
| Analysis | debug-analyzer | /agent-analyze-debug |
| Analysis | dependency-analyzer | /agent-analyze-deps |
| Analysis | performance-analyzer | /agent-analyze-perf |
| Infrastructure | project-initializer | /agent-infra-init |
| Infrastructure | git-workflow-manager | /agent-infra-git |
| Infrastructure | jira-manager | /agent-infra-jira |
| Infrastructure | mcp-developer | /agent-infra-mcp |

## スキル一覧

- coding-standards: コーディング規約
- commit-message: コミットメッセージ規約
- code-review-criteria: レビュー基準
- test-conventions: テスト規約
- documentation-style: ドキュメントスタイル
- doc-management: ドキュメント管理ルール

## ドキュメント

詳細は `.claude/docs/index.md` を参照してください。
