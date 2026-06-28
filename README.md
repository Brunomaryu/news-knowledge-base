# News Knowledge Base

コンサルタント向けニュース知識資産管理システム。業界ニュースを構造化してNotionに蓄積し、長期的な知識資産として活用する。

**バージョン:** v0.1.0  
**最終更新:** 2026-06-28

---

## 概要

### 目的
- 業界ニュースをファクト・背景・示唆の3層で構造化して蓄積する
- 業界別・テーマ別・キーワード別に後から検索・分析できる状態を維持する
- コンサルティング業務での知識活用・提案品質向上に活かす

### データ格納先
- **Notion DB**（本体）: News Knowledge Base データベース
- **本リポジトリ**（ルール・設計）: スキーマ定義・運用ルール・テンプレート

---

## フォルダ構成

```
news-knowledge-base/
├── CLAUDE.md                    # Cowork AIの行動ルール
├── README.md                    # このファイル
├── OPERATIONS.md                # 日常運用マニュアル
├── REQUIREMENTS.md              # 機能要件・実装状況
├── CHANGELOG.md                 # 変更履歴
├── VERSION.md                   # バージョン管理
├── DATA_POLICY.md               # データポリシー
├── .gitignore
│
├── docs/
│   └── DESIGN.md                # 設計仕様書
│
├── schema/                      # Notionスキーマ定義
│   ├── taxonomy.md              # 業界・タグ体系の正式定義
│   ├── notion-schema.md         # DBフィールド定義
│   └── changelog-schema.md     # スキーマ変更履歴
│
├── templates/
│   ├── article-input-template.md    # 記事登録フォーマット
│   └── weekly-digest-template.md   # 週次サマリフォーマット
│
├── tests/
│   └── validation_rules.md      # 整合性チェックルール
│
└── logs/                        # セッション作業ログ
    └── session-log-template.md
```

---

## クイックスタート

### 記事を追加したい
```
「以下のニュースを追加してください：[記事内容]」
```

### 手書きノートをまとめて登録したい
```
「添付のノートをNews DBに反映してください。日付は〇〇にしてください。」
```

### 過去記事を検索したい
```
「〇〇に関する記事を探してください」
「〇〇業界の最近の動向をまとめてください」
```

### タグ・業界を追加したい
→ OPERATIONS.md「5. スキーマ変更」を参照

---

## Notion接続情報

| 項目 | 値 |
|---|---|
| Data Source ID | `347a6219-f619-80de-a8f9-000b3917fd22` |
| Database ID | `347a6219-f619-80ff-bd5c-c42e18612223` |

---

## 関連ドキュメント

- スキーマ定義 → `schema/taxonomy.md`
- 運用手順 → `OPERATIONS.md`
- 機能要件 → `REQUIREMENTS.md`
- 変更履歴 → `CHANGELOG.md`
