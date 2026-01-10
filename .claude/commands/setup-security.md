セキュリティ設定を対話形式で構築します。

## 概要

プロジェクトのセキュリティ設定（認証情報管理、シークレット管理）を構築します。

## 質問フロー

### 質問1: 設定レベル
1. 基本設定（.env.example, .gitignore）
2. 標準設定（+ セキュリティチェックリスト）
3. 詳細設定（+ Dependabot, セキュリティドキュメント）

### 質問2: 環境変数の種類（複数選択可）
1. アプリケーション設定（APP_NAME, APP_ENV等）
2. データベース接続（DB_HOST, DB_PASSWORD等）
3. 外部API連携（API_KEY, SECRET_KEY等）
4. 認証設定（JWT_SECRET, SESSION_KEY等）
5. クラウドサービス（AWS_ACCESS_KEY等）

### 質問3: シークレット管理方法
1. GitHub Secrets
2. AWS Secrets Manager
3. Azure Key Vault
4. 環境変数のみ（.env）

### 質問4: Dependabot設定（詳細設定選択時）
1. 有効（セキュリティアップデートのみ）
2. 有効（すべての依存関係）
3. 無効

### 質問5: 確認
生成予定のファイル一覧を表示し、確認を求める。

## 生成ファイル

### 基本設定

```
{project}/
├── .env.example
└── .gitignore
```

### 標準設定（基本 + 以下）

```
{project}/
└── docs/
    └── security/
        └── security-checklist.md
```

### 詳細設定（標準 + 以下）

```
{project}/
├── .github/
│   └── dependabot.yml
└── docs/
    └── security/
        ├── security-checklist.md
        └── secrets-management.md
```

## 生成例

### .env.example

```bash
# ===========================================
# アプリケーション設定
# ===========================================
APP_NAME=
APP_ENV=local
APP_DEBUG=true
APP_URL=http://localhost

# ===========================================
# データベース設定
# ===========================================
DB_CONNECTION=mysql
DB_HOST=localhost
DB_PORT=3306
DB_DATABASE=
DB_USERNAME=
DB_PASSWORD=

# ===========================================
# 外部サービス
# 本番値は GitHub Secrets 等で管理すること
# ===========================================
API_KEY=
SECRET_KEY=

# ===========================================
# 認証設定
# ===========================================
JWT_SECRET=
SESSION_LIFETIME=120
```

### .gitignore（追加項目）

```gitignore
# 環境変数
.env
.env.local
.env.*.local

# 認証情報
*.pem
*.key
credentials.json
service-account.json

# IDE設定（機密情報が含まれる可能性）
.idea/
.vscode/settings.json

# ログ（機密情報が含まれる可能性）
*.log
logs/
```

### dependabot.yml

```yaml
version: 2
updates:
  - package-ecosystem: "npm"
    directory: "/"
    schedule:
      interval: "weekly"
    open-pull-requests-limit: 10
    labels:
      - "dependencies"
      - "security"
```

### security-checklist.md

```markdown
# セキュリティチェックリスト

## 環境変数・認証情報

- [ ] .env が .gitignore に含まれている
- [ ] 本番の認証情報がコードにハードコードされていない
- [ ] APIキーは環境変数経由で取得している
- [ ] GitHub Secrets に機密情報を登録済み

## 依存関係

- [ ] Dependabot が有効になっている
- [ ] npm audit / composer audit で脆弱性がない
- [ ] 不要な依存関係を削除済み

## 通信

- [ ] HTTPS が有効（本番環境）
- [ ] CORS設定が適切
- [ ] CSRFトークンを使用

## 入力検証

- [ ] ユーザー入力をサニタイズしている
- [ ] SQLインジェクション対策済み
- [ ] XSS対策済み

## 認証・認可

- [ ] パスワードはハッシュ化して保存
- [ ] セッション管理が適切
- [ ] 権限チェックを実装
```

## 設定の記録

生成完了後、`.claude/project-config.yml` の `security` セクションを `completed` に更新する。

---

## 開始

それでは、セキュリティ設定を開始します。
まず、設定レベルを選択してください。
