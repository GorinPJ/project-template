template-managerを使って、テンプレート・カタログ・ガイドラインを管理してください。

## 要件
$ARGUMENTS

## 作業内容
- 共通テンプレートへの反映
- 特定プロジェクトへの適用
- カタログの更新
- ガイドラインの更新
- 変更履歴の記録

## 参照ファイル
- 変更履歴: `.claude/docs/changelog/CHANGELOG.md`
- プロジェクト一覧: `.claude/docs/catalogs/project-registry.md`
- カタログ: `.claude/docs/catalogs/`

---

## 使用例

```
/agent-meta-template これを共通テンプレートに反映して
/agent-meta-template 共通テンプレートの更新をプロジェクトXに適用して
/agent-meta-template エージェントカタログを更新して
```
