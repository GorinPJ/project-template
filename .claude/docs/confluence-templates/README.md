# Confluenceテンプレート

## 概要

新規プロジェクト用のConfluenceスペースを作成するためのテンプレート集です。

---

## フォルダ構成

```
confluence-templates/
├── 01_プロジェクト概要/
│   ├── プロジェクト概要.md
│   ├── 体制図.md
│   └── スケジュール.md
├── 02_要件定義/
│   ├── 機能要件.md
│   ├── 非機能要件.md
│   └── 画面一覧.md
├── 03_設計/
│   ├── アーキテクチャ設計.md
│   ├── DB設計.md
│   ├── API設計.md
│   └── 画面設計.md
├── 04_開発/
│   ├── 環境構築手順.md
│   ├── コーディング規約.md
│   └── 開発ガイド.md
├── 05_テスト/
│   ├── テスト計画.md
│   ├── テストケース.md
│   └── テスト結果.md
├── 06_運用/
│   ├── 運用手順書.md
│   ├── 障害対応手順.md
│   └── 監視設定.md
├── 07_議事録/
│   └── 議事録テンプレート.md
└── 08_その他/
    └── 参考資料.md
```

---

## Confluenceへのアップロード手順

### 方法1: 手動アップロード

#### 1. スペース作成

1. Confluenceにログイン
2. 「スペースを作成」をクリック
3. 「空白のスペース」を選択
4. スペース情報を入力
   - スペース名: {プロジェクト名}
   - スペースキー: {略称（大文字英数字）}
5. 「作成」をクリック

#### 2. フォルダ（親ページ）作成

1. スペースのホームページを開く
2. 「+」→「ページを作成」
3. タイトル: 「01_プロジェクト概要」
4. 内容は空でOK → 「公開」
5. 同様に02〜08のフォルダを作成

#### 3. 各ページ作成

1. 親ページ（例: 01_プロジェクト概要）を開く
2. 「...」→「子ページを作成」
3. タイトル: 「プロジェクト概要」
4. このリポジトリの対応するMarkdownファイルの内容をコピー&ペースト
5. 「公開」
6. 同様に他のページも作成

### 方法2: Confluence API経由（自動化）

#### 前提条件

- Confluence APIトークンが必要
- Python環境が必要

#### スクリプト実行

```bash
# 環境変数設定
export CONFLUENCE_URL="https://xxx.atlassian.net"
export CONFLUENCE_USER="your-email@example.com"
export CONFLUENCE_TOKEN="your-api-token"
export CONFLUENCE_SPACE_KEY="NEWPROJ"

# スクリプト実行
python scripts/upload-to-confluence.py
```

#### スクリプト例（参考）

```python
import os
import requests
from pathlib import Path

CONFLUENCE_URL = os.environ.get("CONFLUENCE_URL")
CONFLUENCE_USER = os.environ.get("CONFLUENCE_USER")
CONFLUENCE_TOKEN = os.environ.get("CONFLUENCE_TOKEN")
SPACE_KEY = os.environ.get("CONFLUENCE_SPACE_KEY")

def create_page(title, content, parent_id=None):
    """Confluenceにページを作成"""
    url = f"{CONFLUENCE_URL}/wiki/rest/api/content"

    data = {
        "type": "page",
        "title": title,
        "space": {"key": SPACE_KEY},
        "body": {
            "storage": {
                "value": content,
                "representation": "storage"
            }
        }
    }

    if parent_id:
        data["ancestors"] = [{"id": parent_id}]

    response = requests.post(
        url,
        json=data,
        auth=(CONFLUENCE_USER, CONFLUENCE_TOKEN),
        headers={"Content-Type": "application/json"}
    )

    return response.json()

# 使用例
# create_page("プロジェクト概要", "<p>内容</p>", parent_id)
```

### 方法3: MCP経由（推奨）

Confluence MCPが設定されている場合：

```
/setup-confluence

AI: Confluenceへのアップロードを開始します。
    スペースキーを入力してください。

ユーザー: NEWPROJ

AI: 以下のページを作成します。
    - 01_プロジェクト概要/プロジェクト概要
    - 01_プロジェクト概要/体制図
    - ...

    続行しますか？
```

---

## テンプレートのカスタマイズ

### プロジェクト固有の情報を反映

各テンプレートの `{xxx}` 部分をプロジェクトの情報に置き換えてください。

| プレースホルダー | 置き換え内容 |
|----------------|-------------|
| `{プロジェクト名}` | 実際のプロジェクト名 |
| `{名前}` | 担当者名 |
| `YYYY-MM-DD` | 実際の日付 |
| `example.com` | 実際のドメイン |

### 不要なセクションの削除

プロジェクトに不要なセクションは削除してください。

### 追加ページの作成

必要に応じてページを追加してください。
テンプレートの形式を参考に作成すると統一感が出ます。

---

## 更新履歴

| 日付 | バージョン | 内容 |
|------|-----------|------|
| 2026-01-10 | 1.0.0 | 初版作成 |
