# MCPカタログ

利用可能なMCP（Model Context Protocol）サーバーのテンプレート一覧です。

## カテゴリ別一覧

### ドキュメント・ファイル操作

| MCP | 説明 | 主要ツール |
|-----|------|-----------|
| excel-mcp-server | Excelファイルの読み書き | read_excel, write_excel |
| mcp-google-sheets | Googleスプレッドシート操作 | get_spreadsheet, update_cells, append_rows |
| mcp-google-drive | Googleドライブ操作 | search_files, read_file, list_folder |
| mcp-filesystem | ファイルシステム拡張 | read_file, write_file, search_files |

### 開発・バージョン管理

| MCP | 説明 | 主要ツール |
|-----|------|-----------|
| mcp-github | GitHub連携 | create_issue, create_pull_request, push_files |
| mcp-atlassian | Jira/Confluence連携 | jira_create_issue, confluence_create_page |
| drawio | 図表作成 | add_nodes, link_nodes, edit_nodes |

### データベース

| MCP | 説明 | 主要ツール |
|-----|------|-----------|
| mcp-postgres | PostgreSQL操作 | query, list_tables, describe_table |
| mcp-mysql | MySQL操作 | mysql_query, list_tables, describe_table |

### Web・ブラウザ操作

| MCP | 説明 | 主要ツール |
|-----|------|-----------|
| mcp-fetch | Webページ取得 | fetch |
| mcp-puppeteer | ブラウザ自動操作 | puppeteer_navigate, puppeteer_screenshot, puppeteer_click |

### コミュニケーション

| MCP | 説明 | 主要ツール |
|-----|------|-----------|
| mcp-slack | Slack連携 | post_message, get_channel_history, search_messages |

### ドキュメント参照

| MCP | 説明 | 主要ツール |
|-----|------|-----------|
| context7 | 最新ライブラリドキュメント取得 | resolve-library-id, get-library-docs |

## セットアップ手順

### 1. テンプレートの確認
```bash
# テンプレートファイルを確認
cat .claude/mcp/templates/{mcp-name}.json
```

### 2. mcp.json への追加
`.claude/mcp.json` にテンプレートの内容をコピーし、必要な値を設定します。

```json
{
  "mcpServers": {
    "excel": {
      "command": "npx",
      "args": ["--yes", "@negokaz/excel-mcp-server"],
      "env": {
        "EXCEL_MCP_PAGING_CELLS_LIMIT": "4000"
      }
    }
  }
}
```

### 3. 認証情報の設定
APIキーやトークンが必要なMCPは、環境変数で設定します。

```bash
# 例：GitHubトークン
set GITHUB_PERSONAL_ACCESS_TOKEN=your-token
```

## テンプレートファイル一覧

```
.claude/mcp/
├── mcp.json.example      # 設定例
└── templates/
    ├── atlassian.json    # Jira/Confluence
    ├── context7.json     # ライブラリドキュメント
    ├── drawio.json       # 図表作成
    ├── excel.json        # Excel操作
    ├── fetch.json        # Web取得
    ├── filesystem.json   # ファイル操作
    ├── github.json       # GitHub
    ├── google-drive.json # Googleドライブ
    ├── google-sheets.json # スプレッドシート
    ├── mysql.json        # MySQL
    ├── postgres.json     # PostgreSQL
    ├── puppeteer.json    # ブラウザ自動操作
    └── slack.json        # Slack
```

## 推奨MCP組み合わせ

### Web開発プロジェクト
- mcp-github（バージョン管理）
- mcp-mysql または mcp-postgres（DB操作）
- context7（ライブラリドキュメント）

### ドキュメント管理プロジェクト
- mcp-atlassian（Confluence）
- excel-mcp-server（Excel）
- drawio（図表）

### 自動化・テストプロジェクト
- mcp-puppeteer（ブラウザ操作）
- mcp-slack（通知）
- excel-mcp-server（結果出力）

## 注意事項

1. **セキュリティ**: 認証情報は環境変数で管理し、コードにハードコードしない
2. **権限**: 最小限の権限を持つトークン・ユーザーを使用
3. **本番環境**: データベースMCPは本番環境への接続に注意
