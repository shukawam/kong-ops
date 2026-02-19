# 注文 API

## エンドポイント

### POST /orders

- **概要:** 新しい注文を作成します。
- **リクエストボディ:**
  - **コンテンツ:** `application/json`
    - **スキーマ:** `Order`
- **レスポンス:**
  - `201 Created`: 注文が作成されました。
    - **コンテンツ:** `application/json`
      - **スキーマ:** `Order`

### GET /orders/{orderId}

- **概要:** ID で注文を取得します。
- **パラメータ:**
  - `orderId` (パス, 必須): 注文の ID。
- **レスポンス:**
  - `200 OK`: 単一の注文。
    - **コンテンツ:** `application/json`
      - **スキーマ:** `Order`

## スキーマ

### Order

| 名前    | 型     | 説明                       |
| ------- | ------ | -------------------------- |
| orderId | string | 注文の ID。                |
| items   | array  | 注文に含まれる商品の配列。 |
| total   | number | 注文の合計金額。           |

### Item

| 名前      | 型      | 説明         |
| --------- | ------- | ------------ |
| itemId    | string  | 商品の ID。  |
| quantity  | integer | 商品の数量。 |
| unitPrice | number  | 商品の単価。 |
