CI/CD設定を対話形式で構築します。

## 概要

プロジェクトに合わせたCI/CDパイプラインを構築し、必要なファイルを生成します。

## 質問フロー

### 質問1: CIツール
1. GitHub Actions
2. GitLab CI
3. Jenkins
4. その他

### 質問2: テスト実行タイミング
1. push時のみ
2. PR時のみ
3. push + PR両方（推奨）

### 質問3: 対象ブランチ
1. main のみ
2. main + develop
3. すべてのブランチ
4. カスタム指定

### 質問4: 実行内容（複数選択可）
1. テスト実行
2. Linter（コード品質チェック）
3. ビルド
4. デプロイ
5. セキュリティスキャン

### 質問5: デプロイ設定（デプロイを選択した場合）
1. dev環境のみ
2. dev + staging
3. dev + staging + production
4. カスタム

### 質問6: 通知設定
1. Slack
2. Microsoft Teams
3. メール
4. なし

### 質問7: 確認
生成予定のファイル一覧を表示し、確認を求める。

## 生成ファイル（GitHub Actionsの場合）

```
.github/
└── workflows/
    ├── ci.yml           # テスト・Lint
    ├── build.yml        # ビルド（必要な場合）
    └── deploy.yml       # デプロイ（必要な場合）
```

## 生成例

### ci.yml

```yaml
name: CI

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'

      - name: Install dependencies
        run: npm ci

      - name: Run linter
        run: npm run lint

      - name: Run tests
        run: npm test

      - name: Upload coverage
        uses: codecov/codecov-action@v4
        if: always()

  notify:
    needs: test
    runs-on: ubuntu-latest
    if: failure()
    steps:
      - name: Notify Slack
        uses: 8398a7/action-slack@v3
        with:
          status: ${{ job.status }}
          fields: repo,message,commit,author
        env:
          SLACK_WEBHOOK_URL: ${{ secrets.SLACK_WEBHOOK_URL }}
```

### deploy.yml

```yaml
name: Deploy

on:
  push:
    branches:
      - main
      - develop

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Deploy to dev
        if: github.ref == 'refs/heads/develop'
        run: |
          echo "Deploying to dev environment"
          # デプロイコマンドをここに記載

      - name: Deploy to production
        if: github.ref == 'refs/heads/main'
        run: |
          echo "Deploying to production environment"
          # デプロイコマンドをここに記載
```

## 設定の記録

生成完了後、`.claude/project-config.yml` の `ci_cd` セクションを `completed` に更新する。

## 必要なシークレット

GitHub Secretsに以下を設定するよう案内：

| シークレット名 | 用途 |
|---------------|------|
| SLACK_WEBHOOK_URL | Slack通知（通知設定時） |
| DEPLOY_KEY | デプロイ用キー（デプロイ設定時） |

---

## 開始

それでは、CI/CD設定を開始します。
まず、使用するCIツールを選択してください。
