# プロジェクトテンプレート ドキュメント

このテンプレートは、Claude Codeを活用したソフトウェア開発プロジェクトの標準構成を提供します。

## 目次

### カタログ
- [エージェントカタログ](./catalogs/agent-catalog.md) - 利用可能なサブエージェント一覧
- [MCPカタログ](./catalogs/mcp-catalog.md) - MCP設定テンプレート一覧
- [スキルカタログ](./catalogs/skill-catalog.md) - スキル一覧
- [プロジェクトレジストリ](./catalogs/project-registry.md) - テンプレート適用プロジェクト一覧

### ガイド
- [新プロジェクト構築ガイド](./guides/new-project-setup-guide.md) - **新規プロジェクト作成の全手順**
- [エージェント・コマンドガイド](./guides/agent-command-guide.md) - エージェントの使い方
- [プロジェクトガイドライン](./guides/project-guidelines.md) - プロジェクト運用ルール

### 変更履歴
- [CHANGELOG](./changelog/CHANGELOG.md) - テンプレートの更新履歴

### 設計ドキュメント
- [設計決定](./design/design-decisions.md) - 設計判断の記録
- [検討履歴](./design/discussion-log.md) - 検討過程の記録

## クイックスタート

### 1. テンプレートのコピー
```bash
# 新規プロジェクトにテンプレートをコピー
cp -r C:\noguchi\system\projects\project-template\.claude C:\noguchi\system\projects\{your-project}\
```

### 2. プロジェクト固有の設定
- `CLAUDE.md` をプロジェクトに合わせて編集
- 必要なMCPを `.claude/mcp.json` に設定

### 3. エージェントの利用
```
# カスタムコマンドでエージェントを呼び出し
/agent-dev-code 認証機能を実装してください

# または直接指示
@code-developer 認証機能を実装してください
```

## テンプレート構成

```
.claude/
├── agents/           # サブエージェント定義
│   ├── common/       # 共通ルール
│   ├── meta/         # メタエージェント
│   ├── document/     # ドキュメント管理
│   ├── development/  # 開発支援
│   ├── test/         # テスト管理
│   ├── analysis/     # 分析・調査
│   └── infrastructure/  # インフラ・運用
├── commands/         # カスタムコマンド
├── skills/           # スキル（自動適用ルール）
├── mcp/              # MCP設定テンプレート
│   └── templates/    # 各MCP設定
└── docs/             # ドキュメント
    ├── catalogs/     # カタログ類
    ├── guides/       # ガイド
    ├── changelog/    # 変更履歴
    └── design/       # 設計ドキュメント
```

## バージョン

- テンプレートバージョン: 1.0.0
- 作成日: 2026-01-10
- 対象: 汎用ソフトウェアプロジェクト
