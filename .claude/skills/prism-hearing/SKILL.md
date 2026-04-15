---
name: prism-hearing
description: |
  第N回ヒアリングの準備フェーズ。charter.md を読み込み、ヒアリングアジェンダと議事録テンプレートを生成する。
  当日持参するアジェンダと、ヒアリング後に埋める議事録のセットを出力する。
  hearing-preparer の機能を統合したエントリーポイント。
user-invocable: true
---

# prism-hearing

## 責務

「このプロジェクトで絶対に聞くべきこと」を charter.md から整理し、
ヒアリングアジェンダ + 議事録テンプレートをセットで生成する。

人間がヒアリングに持参し、終わったら議事録を埋めて `/prism-analyze` に渡す。

---

## 実行手順

### Step 1: 入力確認

ユーザーから以下を受け取る：

```
/prism-hearing [YY-MM-DD]

[YY-MM-DD] = ヒアリング予定日（例: 26-04-15）
```

ファイルパスは自動的に `docs/hearings/{YY-MM-DD}/` に統一する。

### Step 2: 前提ファイルの確認

```
docs/charter.md が存在するか確認
  ✅ 存在 → Step 3 へ
  ❌ 存在しない → エラーで停止
```

charter.md が存在しない場合：

```
「docs/charter.md が見つかりません。
 まず /prism-init を実行してください。」
```

### Step 3: charter.md 読み込み

charter.md から以下を抽出：

```
- Goals・Non-goals
- Constraints（予算・スケジュール・技術）
- Stakeholders（意思決定者・開発体制）
- Open Questions ⚠️（未解決事項）
- 各項目のステータス（🔵 Draft / ✅ Confirmed）
```

前回のヒアリング分析がある場合（`docs/analysis/analysis_*.md`）を確認：

```
最新の analysis_*.md から「次回ヒアリング 確認事項リスト」を読み込む
  → 🔴 必須確認を今回アジェンダに含める
```

### Step 4: 確認事項の生成

`references/question-guide.md` の判断基準に従い、確認事項を生成する。

#### 確認事項の抽出ロジック

**① charter.md の未定・🔵 Draft 項目を確認事項に変換**

```
「予算が 🔵 Draft」
  → 確認事項: 「予算の概算金額を確定させる」

「リリース目標が未定」
  → 確認事項: 「リリース目標日を確定させる」
```

**② Open Questions ⚠️ を優先度 🔴 として組み込む**

```
矛盾・認識ズレがある項目は必ず 🔴 必須確認に上げる
```

**③ ドメイン特有の質問を追加（charter.md の文脈から）**

```
charter.md の業界・システム種別・関係者情報を参考に、
「聞かないと後で詰まる」質問を追加する
```

**④ 前回積み残し確認事項を引き継ぐ**

```
docs/analysis/analysis_*.md が存在する場合：
  「次回ヒアリング 確認事項リスト」を読む
    ↓
  🔴 必須確認 → 今回のアジェンダの最優先に追加
  🟡 重要確認 → 今回のアジェンダに追加
  🟢 できれば → 時間あれば確認リストに追加
```

### Step 5: 出力ファイルの生成

`references/` のテンプレートに従い、2ファイルを生成：

```
docs/hearings/{YY-MM-DD}/agenda.md
docs/hearings/{YY-MM-DD}/hearing.md
```

（フォルダが存在しない場合は作成）

### Step 6: 完了サマリーを出力して停止

```
## prism-hearing 完了

docs/hearings/{YY-MM-DD}/agenda.md を生成しました。
docs/hearings/{YY-MM-DD}/hearing.md を生成しました。

🔴 必須確認: {N}件
🟡 重要確認: {N}件
🟢 できれば: {N}件

---
## 次のステップ

1. agenda.md をヒアリングに持参してください
2. ヒアリング実施後、hearing.md に回答を記入してください
3. 記入完了したら以下を実行:
   → /prism-analyze [YY-MM-DD]
```

完了後は必ず**停止**する。自動で prism-analyze を実行しない。

---

## 注意事項

- アジェンダは「聞く順番」まで考えて並べる（重要なものを前半に）
- 1回のヒアリングで消化できる量に絞る（目安：60分で🔴3件・🟡5件）
- charter.md の情報がない項目を勝手に仮定して質問を作らない
- 議事録テンプレート（hearing.md）は「埋めるだけ」の状態にする。構造化は prism-analyze に任せる
