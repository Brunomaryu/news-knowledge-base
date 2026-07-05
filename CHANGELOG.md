# 変更履歴

このファイルは [Keep a Changelog](https://keepachangelog.com/ja/1.0.0/) 形式に準拠する。  
バージョン採番は [Semantic Versioning](https://semver.org/lang/ja/) に従う。

---

## [v1.2.0] - 2026-06-28

### 概要
外部AIアーキテクト（ChatGPT）からの12提案を技術レビューし、個人利用前提・保守性・後方互換性を重視した観点で採否を判断。採用・一部採用した内容を設計に反映。

### Added（新規）

**設計・アーキテクチャ**
- `docs/DESIGN.md`: AI役割定義（Ingest/Analyst/Curator/QA）を設計ドキュメントに追記
- `docs/DESIGN.md`: 定期レビューサイクル（週次・月次・四半期）を追加
- `docs/DESIGN.md`: 企業DB・業界DB更新時の擬似Draftフロー（変更前確認フロー）を追加
- `docs/DESIGN.md`: バリデーションルール一覧（V-1〜V-9）を追加
- `docs/PROJECT_OVERVIEW.md`: プロジェクト概要・全機能一覧ドキュメントを新規作成

**フィールド追加（設計）**
- ニュースDB: `出典URL`（url型）・`媒体名`（text型）フィールドを設計に追加
- ニュースDB: `関連記事`（relation型）フィールドを設計に追加（同テーマ記事の時系列連鎖）
- 企業DB・業界DB: `情報ステータス`（select型: 最新/要確認/obsolete）フィールドを設計に追加

**ルール・テンプレート**
- 示唆フィールドの推奨フォーマット（5要素＋確度フラグ）を `docs/DESIGN.md` に定義
- タグ5件上限を `schema/taxonomy.md` ガイドラインで明示化
- バリデーションルール V-3〜V-9 を `tests/validation_rules.md` に追加予定

**将来機能（設計のみ）**
- `scripts/export_notion.py`: Notion定期エクスポート（バックアップ）
- `scripts/validate_taxonomy.py`: バリデーション自動化スクリプト
- `prompts/`: プロンプト管理フォルダ

### Changed（変更）

- `docs/DESIGN.md`: v0.1.0 → v1.2.0 に全面更新
- `REQUIREMENTS.md`: 機能要件を全面拡充（37項目 → 64項目）

### Decided（設計判断・不採用）

| 提案 | 判断 | 理由 |
|---|---|---|
| P1: ローカルDB追加 | 不採用 | 個人利用には過剰。二重管理コストが価値を上回る |
| P7: 独自ID設計 | 不採用 | NotionのネイティブUUIDと二重管理になる |
| P3: Source DB追加 | 不採用（フィールド追加で代替） | 個人利用33件にはオーバーエンジニアリング |
| P10: 出典必須化 | 不採用（推奨のまま） | 手書きノート・口頭情報に出典がない場合への配慮 |
| P12: 日次レビュー | 不採用（週次以上のみ採用） | 個人利用に過剰 |
| P4: マルチエージェント実装 | 将来予定（文書化のみ採用） | 現行の単一AI運用で十分。将来Agent化時の設計準備として文書化 |

---

## [v1.1.0] - 2026-06-28

### Added（新規）

**企業DB・業界DB**
- 企業DB新規作成（10社: NVIDIA・安川電機・Netflix・日本製鉄・栗田工業・BYD・IHI・川崎重工業・トヨタ・ホンダ）
- 業界DB新規作成（10業界）
- 6業界のHOTニュースセクション更新（insert_contentコマンド使用）
- 全10社の関連ニュース記事リレーションリンクを確立

**CLAUDE.md 機能追加**
- 「既存ニュースから企業・業界DBを一括バッチ生成する時」フローを追加
- 企業情報フォーマット: 「営業利益率推移（1年1行形式）」仕様を追加
- 企業情報フォーマット: 「自分のメモ・学び（示唆の自動引用）」仕様を追加

**データ処理**
- 全10社の営業利益率推移を `/` 区切り → 1年1行の改行形式に一括変換
- 全15本の関連記事から「示唆」フィールドを取得し、企業DB「自分のメモ・学び」に追記（記事リンク付き）

### Fixed（修正）

- Batch 2（BYD・IHI・川崎重工業・トヨタ・ホンダ）の一括作成時の文字化けエラー（「上場区分」→「上場区剆」）を修正して再実行
- 業界DBのHOTニュース更新コマンドを `append`（無効）→ `insert_content`（有効）に修正

### Decided（設計判断）

- `notion-update-page` の有効コマンド: `update_properties` / `update_content` / `replace_content` / `insert_content` / `apply_template` / `update_verification`（`append` は無効）
- 関連ニュース記事のリレーションはフルURL形式: `["https://app.notion.com/p/{UUID_no_dashes}?pvs=1"]`
- notion-fetch のパラメータ名は `id`（`page_id` は無効）

---

## [v1.1.0-schema] - 2026-06-28

### Added
- タグ追加: 高市重点（orange）— 高市内閣17重点産業対応
- schema/taxonomy.md を v1.0 → v1.1 に更新

---

## [v0.1.0] - 2026-06-28

### Added（新規）
- Notion News DBの初期スキーマ設計（Fact / Summary / 業界 / タグ / 日付 / 背景 / 示唆）
- 業界 multi-select カラム追加（10カテゴリ）
- タグ multi-select カラム追加（39タグ）
- 既存21記事への業界・タグ一括付与
- 手書きノート8ページ分から30件の新規記事をNotionに登録
- 既存3記事（ツルハHD・ディズニー×OpenAI・トヨタ受注停止）の背景・示唆を補完
- 運用管理フォルダ構成の初期構築（CLAUDE.md・OPERATIONS.md・REQUIREMENTS.md等）
- schema/taxonomy.md（業界・タグ体系の正式定義）
- schema/notion-schema.md（DBフィールド定義）
- templates/article-input-template.md
- templates/weekly-digest-template.md
- tests/validation_rules.md

### Changed（変更）
- タグ列: Text型 → multi-select型に変更
- 業界列: 既存Textフィールドとは別に新規multi-select列を追加

### Decided（設計判断）
- マスターデータはNotion DB、本リポジトリはルール・設計定義のみ管理
- 記事は原則削除しない（知識資産として保持）
- スキーマ変更は必ずユーザー承認後に実行

---

## [Unreleased]
- GitHub初回コミット・リポジトリ公開
- scripts/validate_taxonomy.py（タグ整合性チェック自動化）
- scripts/export_notion.py（Notion定期エクスポート）
- Notion→Excel同期スクリプト
- templates/insight-template.md（示唆フォーマットテンプレート）
- ニュースDBへの出典URL・媒体名・関連記事フィールド追加（Notion MCP実行）
- 企業DB・業界DBへの情報ステータスフィールド追加（Notion MCP実行）
