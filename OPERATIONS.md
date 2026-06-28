# 日常運用マニュアル

**作成日:** 2026-06-28  
**最終更新:** 2026-06-28

---

## 目次

1. [記事の追加](#1-記事の追加)
2. [手書きノートの一括登録](#2-手書きノートの一括登録)
3. [既存記事の補完・修正](#3-既存記事の補完修正)
4. [記事の検索・分析](#4-記事の検索分析)
5. [スキーマ変更（業界・タグ追加等）](#5-スキーマ変更業界タグ追加等)
6. [週次サマリ生成](#6-週次サマリ生成)
7. [タグ整合性チェック](#7-タグ整合性チェック)
8. [GitHub運用手順](#8-github運用手順)
9. [Cowork利用ルール](#9-cowork利用ルール)

---

## 1. 記事の追加

### Cowork経由（推奨）
```
「以下のニュースをNews DBに追加してください：
  [ファクト・背景・示唆]
  業界: 〇〇
  タグ: 〇〇, 〇〇
  日付: YYYY-MM-DD」
```

Coworkが以下を自動実行：
- `schema/taxonomy.md` で業界・タグの妥当性を確認
- `notion-create-pages` でNotionに登録

### 記事フォーマット
`templates/article-input-template.md` を参照。

### 日付ルール
| 状況 | 設定値 |
|---|---|
| 日付が明確 | 記事掲載日（YYYY-MM-DD） |
| 手書きノート（日付なし） | 2025-08-01（デフォルト） |
| ページ上部に日付記載あり | 2025-{記載月日}（ページ単位で統一） |

---

## 2. 手書きノートの一括登録

### 手順
1. ノート画像をCoworkに添付する
2. 以下のように指示する：
```
「添付のノートをNews DBに反映してください。
  日付は〇〇にしてください。
  タグと業界はニュース内容を踏まえて付与してください。
  最後に①新規追加件数 ②補完件数 ③重要テーマTop10 をレポートしてください。」
```
3. Coworkが以下を自動実行：
   - ノート内容を解析・構造化
   - 既存記事との重複確認
   - 新規記事をバッチ追加（5〜8件単位）
   - 既存記事の背景・示唆を補完
   - 最終レポートを提示

---

## 3. 既存記事の補完・修正

### 特定記事の補完
```
「〇〇に関する記事の背景と示唆を補完してください」
「〇〇の記事に業界・タグを追加してください」
```

### 一括補完
```
「業界・タグが未設定の記事を探して補完してください」
```

---

## 4. 記事の検索・分析

### キーワード検索
```
「〇〇に関する記事を全て探してください」
「生成AIタグの記事一覧を見せてください」
```

### テーマ分析
```
「自動車業界の記事をまとめてトレンドを教えてください」
「M&Aに関する最近の動向を整理してください」
```

### 比較分析
```
「EVに関する日本と中国の動向を比較してください」
```

---

## 5. スキーマ変更（業界・タグ追加等）

### 原則
- **スキーマ変更は必ず事前承認が必要**
- 変更後は `schema/taxonomy.md` と `schema/changelog-schema.md` を更新する

### 新しいタグ・業界を追加したい場合
```
「〇〇というタグを追加してください」
```

Coworkが以下のフローで対応：
1. 影響範囲（既存記事への影響）を提示
2. ユーザー承認を得る
3. `schema/taxonomy.md` を更新
4. Notion MCPで ALTER COLUMN を実行
5. `schema/changelog-schema.md` に記録
6. `CHANGELOG.md` に追記

### 禁止事項
- 既存タグ・業界の削除（代わりに廃止フラグを立てる）
- 承認なしのスキーマ変更

---

## 6. 週次サマリ生成

```
「今週のNews DBサマリを作成してください」
```

保存先: `logs/week-YYYY-WNN.md`  
テンプレート: `templates/weekly-digest-template.md`

---

## 7. タグ整合性チェック

```
「News DBのタグ・業界の整合性をチェックしてください」
```

`tests/validation_rules.md` のルールに基づき確認。

---

## 8. GitHub運用手順

### 初回セットアップ
```bash
cd "/Users/longbreathstudio/Claude/Projects/News Knowledge Base"
git init
git remote add origin https://github.com/{username}/news-knowledge-base.git
git add .
git commit -m "chore: 初回コミット - News Knowledge Base v0.1.0"
git push -u origin main
git tag v0.1.0 -m "v0.1.0: 初期構成"
git push origin --tags
```

### 日常的なコミット
```bash
git status
git diff
git add {ファイル名}   # 個別指定推奨
git commit -m "{type}({scope}): {内容}"
git push
```

### commit message形式
```
{type}({scope}): {内容（日本語可）}

type: feat / fix / docs / refactor / chore
scope: claude / schema / ops / req / template / log / design
```

### コミットのタイミング（推奨）
| タイミング | 内容 |
|---|---|
| スキーマ変更後 | `feat(schema): 〇〇タグを追加` |
| ルール変更後 | `docs(claude): 〇〇フローを更新` |
| テンプレート追加後 | `feat(template): 〇〇テンプレートを追加` |
| 週次サマリ生成後 | `docs(log): week-2026-WNN サマリを追加` |

### Branch運用
| ブランチ | 用途 |
|---|---|
| `main` | 安定版（個人利用のため直接コミット可） |
| `feature/{機能名}` | 大きな変更時（任意） |

---

## 9. Cowork利用ルール

### Coworkに任せてよいこと
- 記事の追加・補完・検索
- スキーマ変更（承認後）
- ログファイルの作成
- CHANGELOG・REQUIREMENTS・VERSIONの更新
- Commit Messageの提案

### Coworkに任せてはいけないこと
- 個人名・会社名（機密A）をGitHubにプッシュすること
- スキーマ変更の承認なし実行
- `schema/taxonomy.md` 未定義の業界・タグの使用

### セッション開始時の推奨声がけ
```
「News Knowledge Baseの作業を続けます。
  現在のバージョンはVERSION.mdを確認してください。
  前回の変更はCHANGELOG.mdを確認してください。」
```

---

## 更新履歴

| 日付 | 更新内容 |
|---|---|
| 2026-06-28 | 初版作成 |
