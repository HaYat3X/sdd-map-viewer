---
name: prism-init
description: |
  新規プロジェクト開始時に使用するコマンド。
  対話形式でプロジェクト情報を収集し、docs/charter.md と docs/project_status.md を生成する。
---

# prism-init

## 責務

新規プロジェクトの起点。対話でプロジェクト情報を収集し、
`docs/charter.md` と `docs/project_status.md` を生成する。

---

## 実行手順

### Step 1: 既存チェック

`docs/charter.md` が既に存在する場合は以下を伝えて**停止**する：

```
docs/charter.md が既に存在します。
内容を更新する場合は /prism-update を実行してください。
```

存在しない場合は Step 2 へ。

### Step 2: 情報収集（対話）

`references/questions.md` を読み込み、質問グループに従って対話形式で情報収集する。

### Step 3: docs/charter.md の生成

`references/charter-template.md` を読み込み、テンプレートに従って生成する。
ステータス付与のルールは `prism-status-rules` スキルに従う。

### Step 4: docs/project_status.md の生成

`references/status-template.md` を読み込み、テンプレートに従って生成する。
charter.md と**必ずセットで**生成する。

### Step 5: 完了サマリーを出力して停止

```
## prism-init 完了

docs/charter.md を作成しました。
docs/project_status.md を作成しました。

🔵 Draft 項目: {N}件
⚠️ Open Questions: {N}件

---
## 次のアクション

内容を確認・修正したら、ヒアリング準備へ:
→ /prism-hearing [YY-MM-DD]
```

完了後は必ず**停止**する。自動で次のコマンドを実行しない。
