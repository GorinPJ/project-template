---
name: doc-management
description: "MDファイル作成、ドキュメント作成、ファイル配置、ガイド作成時に適用するドキュメント管理ルール"
---

# ドキュメント管理ルール

MDファイル作成時は以下のルールに従うこと。

## 配置ルール

### フォルダ構造

```
.claude/docs/
├── index.md              # 目次（エントリーポイント）
├── rules/                # ルール・規約系
│   ├── coding-standards.md
│   └── naming-conventions.md
├── guides/               # ガイド・手順書系
│   ├── setup-guide.md
│   └── development-guide.md
├── catalogs/             # カタログ・一覧系
│   ├── agent-catalog.md
│   ├── mcp-catalog.md
│   └── project-registry.md
├── design/               # 設計ドキュメント
│   ├── design-decisions.md
│   └── discussion-log.md
└── changelog/            # 変更履歴
    ├── CHANGELOG.md
    └── changes/
```

### カテゴリ判断基準

| カテゴリ | 配置フォルダ | 例 |
|---------|-------------|-----|
| 規約・ルール | `rules/` | コーディング規約、命名規則 |
| ガイド・手順 | `guides/` | セットアップ、開発手順 |
| 一覧・カタログ | `catalogs/` | エージェント一覧、MCP一覧 |
| 設計・決定 | `design/` | 設計思想、検討履歴 |
| 変更履歴 | `changelog/` | 変更記録 |

## 命名規則

### ファイル名
- **ケバブケース**: `setup-guide.md`, `coding-standards.md`
- **日本語禁止**: ファイル名に日本語を使用しない
- **明確な名前**: 内容が推測できる名前をつける

### 良い例
```
setup-guide.md
api-specification.md
coding-standards.md
```

### 悪い例
```
セットアップガイド.md    # 日本語
setupGuide.md           # camelCase
setup_guide.md          # snake_case
guide.md                # 曖昧
```

## 単一ソース原則

### 重複禁止
- 同じ内容を複数のファイルに書かない
- 他の場所で必要な場合は**リンクで参照**

```markdown
# CLAUDE.md

## コーディング規約
詳細は [コーディング規約](.claude/docs/rules/coding-standards.md) を参照
```

### 更新時の注意
- 定義元のファイルのみを更新
- 参照元は自動的に最新を参照

## index.md の管理

### フォーマット

```markdown
# ドキュメント目次

## ルール
- [コーディング規約](rules/coding-standards.md)
- [命名規則](rules/naming-conventions.md)

## ガイド
- [セットアップガイド](guides/setup-guide.md)

## カタログ
- [エージェント一覧](catalogs/agent-catalog.md)
- [MCP一覧](catalogs/mcp-catalog.md)
```

### 更新タイミング
- 新しいMDファイルを作成したら必ずindex.mdに追加
- ファイルを削除したらindex.mdから削除

## ファイル作成チェックリスト

- [ ] 適切なフォルダに配置したか
- [ ] ファイル名がケバブケースか
- [ ] 既存ファイルと重複していないか
- [ ] index.mdにリンクを追加したか
- [ ] 他ファイルからリンクする場合、参照が正しいか
