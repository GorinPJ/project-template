---
name: commit-message
description: "コミット、git commit、変更をコミット時に適用するコミットメッセージ規約"
---

# コミットメッセージ規約

Git コミット時は以下のフォーマットに従うこと。

## フォーマット

```
<type>(<scope>): <subject>

<body>

<footer>
```

## タイプ一覧

| タイプ | 用途 | 例 |
|--------|------|-----|
| feat | 新機能追加 | feat: ログイン機能を追加 |
| fix | バグ修正 | fix: ログイン時のエラーを修正 |
| docs | ドキュメント | docs: READMEを更新 |
| style | フォーマット（機能変更なし） | style: インデントを修正 |
| refactor | リファクタリング | refactor: ユーザークラスを分割 |
| test | テスト | test: ログインテストを追加 |
| chore | その他（ビルド、CI等） | chore: パッケージを更新 |

## スコープ（任意）

変更の影響範囲を示す：
- `auth`: 認証関連
- `api`: API関連
- `ui`: UI関連
- `db`: データベース関連

## サブジェクト（件名）

- 50文字以内
- 命令形で記述（「〜を追加」ではなく「〜を追加する」）
- 末尾にピリオドをつけない
- 日本語OK

## ボディ（本文）- 任意

- 何を、なぜ変更したかを説明
- 72文字で改行
- 箇条書きOK

## フッター（任意）

- 関連Issue: `Closes #123`, `Fixes #456`
- 破壊的変更: `BREAKING CHANGE: 〜`

## 例

### シンプルなコミット

```
feat: ログイン機能を追加
```

### 詳細なコミット

```
feat(auth): ログイン機能を追加

- メールアドレスとパスワードによる認証
- セッション管理の実装
- ログイン画面のUI作成

Closes #123
```

### バグ修正

```
fix(api): ユーザー取得APIのnullエラーを修正

ユーザーが存在しない場合にnullが返されず
エラーになっていた問題を修正

Fixes #456
```

### 破壊的変更

```
refactor(api): ユーザーAPIのレスポンス形式を変更

BREAKING CHANGE: レスポンスの形式が変更されました
旧: { user: { ... } }
新: { data: { ... } }
```

## Claude Code使用時

Claude Codeでコミットする場合、末尾に以下を追加：

```
🤖 Generated with [Claude Code](https://claude.ai/claude-code)

Co-Authored-By: Claude <noreply@anthropic.com>
```
