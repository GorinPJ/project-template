work-history-recorderを使って、作業履歴を記録してください。

## 対象
$ARGUMENTS

## 作業内容
1. 対象を特定（メインエージェント or サブエージェント）
2. 作業内容を会話履歴から抽出
3. MDファイルに記録
4. 全体サマリーを更新

## 出力先
- メイン: `work/作業履歴/エージェント/`
- サブ: `work/作業履歴/サブエージェント/{名前}/`

---

## 使用例

```
/agent-meta-history
/agent-meta-history code-developer
```
