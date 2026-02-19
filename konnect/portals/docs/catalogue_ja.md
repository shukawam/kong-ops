# 商品カタログ API

## エンドポイント

### GET /products

- **概要:** すべての商品を取得します。
- **レスポンス:**
  - `200 OK`: 商品のリスト。
    - **コンテンツ:** `application/json`
      - **スキーマ:** `Product`の配列

### GET /products/{productId}

- **概要:** ID で商品を取得します。
- **パラメータ:**
  - `productId` (パス, 必須): 商品の ID。
- **レスポンス:**
  - `200 OK`: 単一の商品。
    - **コンテンツ:** `application/json`
      - **スキーマ:** `Product`

## スキーマ

### Product

| 名前        | 型     | 説明             |
| ----------- | ------ | ---------------- |
| id          | string | 商品の ID。      |
| name        | string | 商品名。         |
| description | string | 商品の説明。     |
| price       | number | 商品の価格。     |
| imageUrl    | string | 商品画像の URL。 |
