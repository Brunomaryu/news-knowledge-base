# 変更履歴

このファイルは [Keep a Changelog](https://keepachangelog.com/ja/1.0.0/) 形式に準拠する。  
バージョン採番は [Semantic Versioning](https://semver.org/lang/ja/) に従う。

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
- Notion→Excel同期スクリプト
