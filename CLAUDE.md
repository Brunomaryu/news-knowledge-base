# News Knowledge Base — CLAUDE.md

## システム概要
コンサルタント個人向けニュース知識資産管理システム。業界ニュースを構造化してNotionに蓄積し、業界別・テーマ別・キーワード別に検索・分析できる状態を維持する。

## ユーザープロフィール
- 職種: コンサルタント
- 専門領域: 製造業、AI・データ基盤、経営管理
- 利用目的: 長期的な業界知識の蓄積・構造化・活用

## Notion DB 接続情報
- Data Source ID: `347a6219-f619-80de-a8f9-000b3917fd22`
- Database ID: `347a6219-f619-80ff-bd5c-c42e18612223`
- DB URL: `collection://347a6219-f619-80de-a8f9-000b3917fd22`

---

## スキーマ定義（正式定義は `schema/` を参照）

### フィールド一覧

| フィールド名 | 型 | 役割 |
|---|---|---|
| Fact | title | 記事の核心ファクト（1行） |
| 背景 | text | ニュースの背景・文脈 |
| 示唆 | text | コンサルとして読む示唆・論点 |
| 業界 | multi-select | 10カテゴリから選択（複数可） |
| タグ | multi-select | 39タグから選択（複数可） |
| 日付 | date | 記事日付（YYYY-MM-DD） |
| Summary | text | 要約（レガシー、任意） |
| 業界／国 | text | レガシーフィールド（参照用） |

### 業界カテゴリ（10分類）
自動車・モビリティ / テクノロジー・AI / 半導体・電子部品 / 素材・資源 / 水処理・環境 / エネルギー / 造船・重工業 / ヘルスケア・流通 / メディア・エンタメ / 政策・マクロ経済

詳細は `schema/taxonomy.md` を参照。

---

## AIの行動ルール

### セッション開始時
1. `schema/taxonomy.md` を参照し、業界・タグの許容値を把握する
2. ユーザーの指示を待つ

### 記事を追加する時
1. `templates/article-input-template.md` のフォーマットで内容を整理する
2. `schema/taxonomy.md` で業界・タグを確認・選択する
3. `notion-create-pages` でNotionに登録する（parent: data_source_id）
4. 追加後、セッションログ用のサマリを手元に控える

### 複数記事を一括追加する時（手書きノート等）
1. 全記事の内容を先に整理・分類する
2. 既存記事との重複確認（`notion-search` で類似タイトルを検索）
3. バッチ単位（5〜8件）で `notion-create-pages` を実行する
4. 完了後、①新規追加件数 ②補完件数 ③主要テーマ をレポートする

### スキーマを変更する時（業界・タグの追加・変更等）
```
① 変更内容・影響範囲をユーザーに提示
        ↓
② ユーザー承認を得る（必須）
        ↓
③ schema/taxonomy.md を更新
④ Notion MCPで ALTER COLUMN を実行
⑤ schema/changelog-schema.md に変更を記録
⑥ CHANGELOG.md に追記
⑦ バージョン更新（必要に応じて）
```

### 既存記事を補完する時
1. `notion-search` で対象記事を検索・IDを特定する
2. `notion-fetch` で現在の内容を確認する
3. 不足フィールド（背景・示唆・日付・タグ等）を特定する
4. `notion-update-page` (command: update_properties) で更新する

### 過去記事を検索・分析する時
1. `notion-search` でキーワード検索する
2. 必要に応じて `notion-fetch` で詳細を参照する
3. 複数記事の比較・分析結果を構造化して提示する

### 週次サマリを生成する時
1. `notion-search` で直近1週間の記事を収集する
2. `templates/weekly-digest-template.md` に従いサマリを生成する
3. `logs/week-YYYY-WNN.md` として保存する

### タグ整合性チェック時
1. `tests/validation_rules.md` のルールを確認する
2. Notionの全記事タグが `schema/taxonomy.md` の許容値内かチェックする
3. 逸脱があれば一覧化してユーザーに報告・修正する

---

## ファイル更新ルール

| イベント | 更新ファイル |
|---|---|
| 記事追加（10件以上） | logs/{date}_{topic}.md |
| スキーマ変更 | schema/taxonomy.md + schema/changelog-schema.md + CHANGELOG.md |
| 機能追加・変更 | CHANGELOG.md + REQUIREMENTS.md + VERSION.md |
| 週次サマリ | logs/week-YYYY-WNN.md |

---

## 不変ルール
- タグ・業界の値は `schema/taxonomy.md` の定義外を使わない
- スキーマ変更は必ずユーザー承認後に実行する
- Notion MCPツール呼び出しは `notion-create-pages` の parent に `data_source_id` を使う（`database_id` ではない）
- multi-selectの値はJSON配列文字列として渡す（例: `'["生成AI", "OpenAI"]'`）

---

## commit message形式（GitHub連携時）

```
{type}({scope}): {内容（日本語可）}

type: feat / fix / docs / refactor / chore
scope:
  claude    - CLAUDE.md
  schema    - schema/配下
  ops       - OPERATIONS.md
  req       - REQUIREMENTS.md
  template  - templates/配下
  log       - logs/配下
  design    - docs/DESIGN.md
```
