# 検討履歴

このテンプレート作成時の検討過程を記録します。

## 2026-01-10: テンプレート設計セッション

### 1. 初期要件の確認

**ユーザー要望**:
- 現在のpressanceプロジェクトで作成したサブエージェントを他プロジェクトでも再利用したい
- 汎用的なソフトウェアプロジェクト全般で使用可能なテンプレートが欲しい

**決定事項**:
- テンプレートリポジトリとして `C:\noguchi\system\projects\project-template\` に配置
- Docker環境を使用（Vagrantからの移行）
- プロジェクトは `C:\noguchi\system\projects\{project}\`、Docker設定は `C:\noguchi\system\docker\{project}\`

---

### 2. エージェント構成の検討

**初期案（24エージェント）**:
- Meta: agent-creator, work-history-recorder, template-manager
- Document: doc-manager, agent-catalog-manager, spec-document-generator, data-manager
- Development: code-developer, pattern-developer, refactoring-advisor, code-reviewer
- Test: test-manager, test-spec-generator, test-result-generator, e2e-test-generator
- Analysis: debug-analyzer, defect-analyzer, dependency-analyzer, performance-analyzer
- Infrastructure: project-initializer, git-workflow-manager, jira-manager, mcp-developer

**統合提案**:
ユーザーからの「整理したい」という要望を受け、役割の重複を分析。

**最終決定（16エージェント）**:
- test-spec-generator, test-result-generator, e2e-test-generator → test-manager に統合
- pattern-developer → code-developer に統合（機能実装は同じ責務）
- defect-analyzer → debug-analyzer に統合
- spec-document-generator, agent-catalog-manager → doc-manager に統合

---

### 3. スキル機能の導入

**ユーザー質問**:
「Claude Codeのスキル機能とは何か？サブエージェントとの違いは？」

**説明と議論**:
- スキル: 自動適用されるルール・ガイドライン
- エージェント: 明示的に呼び出すタスク実行者

**決定したスキル（6個）**:
1. coding-standards - コーディング規約
2. commit-message - コミットメッセージ規約
3. code-review-criteria - レビュー基準
4. test-conventions - テスト規約
5. documentation-style - ドキュメントスタイル
6. doc-management - ドキュメント管理ルール

---

### 4. doc-managerの責務

**議論点**:
doc-managerはチェックのみか、作成も行うか？

**選択肢**:
A. チェックのみ（作成は各エージェントが実施）
B. チェック＋作成（ドキュメント管理を一元化）

**決定**: B案を採用
- ドキュメント関連の責務を一元化
- 整合性チェックと作成・更新を統合
- Skill（自動適用ルール）とAgent（タスク実行）の組み合わせ

---

### 5. テンプレート同期機能

**ユーザー要望**:
「テンプレート更新時に、適用済みプロジェクトにも反映したい」

**設計**:
- template-managerエージェントが同期を管理
- project-registry.mdで適用プロジェクトを追跡
- CHANGELOGでバージョン管理
- 差分マージによる更新

**同期対象**:
- 同期する: agents/, commands/, skills/, mcp/templates/, docs/
- 同期しない: mcp.json, CLAUDE.md（プロジェクト固有）

---

### 6. MCP管理方式

**選択肢**:
A. カタログのみ（ドキュメント）
B. テンプレートのみ（JSONファイル）
C. カタログ + テンプレート

**決定**: C案を採用
- mcp-catalog.md: 一覧・概要・使い方
- templates/: 各MCP設定のJSONテンプレート
- mcp.json.example: 設定例

**追加決定**:
- context7 MCPを追加（最新ライブラリドキュメント取得）
- Jira連携はMCP（mcp-atlassian）とエージェント（jira-manager）の両方で対応

---

### 7. 最終確認

**作成物確認**:
- フォルダ構造 ✓
- エージェント定義（16個）✓
- カスタムコマンド（16個）✓
- スキル（6個）✓
- MCPテンプレート（13個）✓
- ドキュメント ✓
- 設計ドキュメント ✓

**ユーザー承認**: 構成案承認後、ファイル作成を開始

---

## 今後の検討事項

1. エージェント機能の拡張
2. 追加MCPテンプレート
3. CI/CD連携
4. チーム開発対応
