# project_status.md テンプレート

`docs/project_status.md` を生成するときのフォーマット。
各コマンド実行後に更新される。

---

```yaml
project: { { PROJECT_NAME } }
phase: フェーズ0（初期化完了）
last_updated: { { DATE } }

charter:
  draft_count: { { N } }
  confirmed_count: 0
  open_questions: { { N } }

hearings: []

unresolved: []

log:
  - datetime: { { DATE } }
    action: prism-init 実行
    result: charter.md・project_status.md を生成

next_action: /prism-hearing [YY-MM-DD]
```

---

## 各フィールドの説明

| フィールド                | 説明                | 更新タイミング                  |
| ------------------------- | ------------------- | ------------------------------- |
| `phase`                   | 現在のフェーズ      | 各コマンド実行後                |
| `charter.draft_count`     | 🔵 Draft 項目数     | prism-init・prism-update 後     |
| `charter.confirmed_count` | ✅ Confirmed 項目数 | prism-update 後                 |
| `charter.open_questions`  | ⚠️ 未解決件数       | prism-init・prism-update 後     |
| `hearings`                | ヒアリング履歴      | prism-hearing・prism-analyze 後 |
| `unresolved`              | 未解決⚠️一覧        | prism-analyze 後                |
| `log`                     | 実行ログ            | 各コマンド実行後                |
| `next_action`             | 次に叩くコマンド    | 各コマンド実行後                |

---

## フェーズ定義

| フェーズ             | 値                              |
| -------------------- | ------------------------------- |
| 初期化完了           | `フェーズ0（初期化完了）`       |
| ヒアリング準備中     | `フェーズ1（ヒアリング準備中）` |
| ヒアリング分析中     | `フェーズ2（分析中）`           |
| 要件定義中           | `フェーズ3（要件定義中）`       |
| アーキテクチャ設計中 | `フェーズ4（設計中）`           |
