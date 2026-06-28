# Notion DBスキーマ定義

**作成日:** 2026-06-28  
**最終更新:** 2026-06-28

---

## 接続情報

| 項目 | 値 | 用途 |
|---|---|---|
| Data Source ID | `347a6219-f619-80de-a8f9-000b3917fd22` | `notion-create-pages` の parent に使用 |
| Database ID | `347a6219-f619-80ff-bd5c-c42e18612223` | DB参照・クエリ用 |
| DB URL | `collection://347a6219-f619-80de-a8f9-000b3917fd22` | notion-update-data-source 用 |

> ⚠️ **重要**: `notion-create-pages` 呼び出し時は `data_source_id` を使うこと（`database_id` では動作しない）

---

## フィールド定義

| フィールド名 | Notion型 | 必須 | 備考 |
|---|---|---|---|
| Fact | title | ✅ | ページタイトル・検索のメインキー |
| 背景 | rich_text | 推奨 | HTMLタグ `<br>` で改行可 |
| 示唆 | rich_text | 推奨 | HTMLタグ `<br>` で改行可 |
| 業界 | multi_select | 推奨 | taxonomy.md の業界カテゴリから選択 |
| タグ | multi_select | 推奨 | taxonomy.md のタグから選択 |
| 日付 | date | 推奨 | プロパティキー: `date:日付:start` |
| Summary | rich_text | 任意 | レガシーフィールド |
| 業界／国 | rich_text | 任意 | レガシーフィールド（廃止予定） |

---

## Notion MCP 呼び出し仕様

### 記事追加（notion-create-pages）

```json
{
  "parent": {
    "type": "data_source_id",
    "data_source_id": "347a6219-f619-80de-a8f9-000b3917fd22"
  },
  "pages": [
    {
      "properties": {
        "Fact": "記事タイトル（核心ファクト）",
        "背景": "背景の文章",
        "示唆": "示唆の文章",
        "業界": "[\"自動車・モビリティ\", \"テクノロジー・AI\"]",
        "タグ": "[\"EV\", \"M&A\"]",
        "date:日付:start": "2025-08-01"
      }
    }
  ]
}
```

### 記事更新（notion-update-page）

```json
{
  "page_id": "{ページID}",
  "command": "update_properties",
  "properties": {
    "背景": "更新後の背景",
    "示唆": "更新後の示唆",
    "タグ": "[\"生成AI\", \"OpenAI\"]"
  }
}
```

### スキーマ変更（notion-update-data-source）

タグ追加の例:
```sql
ALTER COLUMN "タグ" SET MULTI_SELECT(
  "生成AI" COLOR "purple",
  "新しいタグ" COLOR "blue"
  -- 既存タグも全て含める必要あり
)
```

> ⚠️ ALTER COLUMN は既存選択肢を全て上書きするため、既存タグを全て含めた状態で実行すること

---

## 有効なNotionカラー

`default` / `gray` / `brown` / `orange` / `yellow` / `green` / `blue` / `purple` / `pink` / `red`

> ❌ `teal` は無効（使用不可）

---

## 更新履歴

| 日付 | 更新内容 |
|---|---|
| 2026-06-28 | 初版作成 |
