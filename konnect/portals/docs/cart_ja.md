# カート API

## エンドポイント

### GET /carts/{cartId}

- **概要:** ID でカートを取得します。
- **パラメータ:**
  - `cartId` (パス, 必須): カートの ID。
- **レスポンス:**
  - `200 OK`: 単一のカート。
    - **コンテンツ:** `application/json`
      - **スキーマ:** `Cart`

### POST /carts/{cartId}

- **概要:** カートに商品を追加します。
- **パラメータ:**
  - `cartId` (パス, 必須): カートの ID。
- **リクエストボディ:**
  - **コンテンツ:** `application/json`
    - **スキーマ:** `Item`
- **レスポンス:**
  - `201 Created`: 商品が追加されました。

### GET /carts/{cartId}/items

- **概要:** カート内のすべての商品を取得します。
- **パラメータ:**
  - `cartId` (パス, 必須): カートの ID。
- **レスポンス:**
  - `200 OK`: 商品のリスト。
    - **コンテンツ:** `application/json`
      - **スキーマ:** `Item`の配列

## スキーマ

### Cart

| 名前   | 型     | 説明                   |
| ------ | ------ | ---------------------- |
| cartId | string | カートの ID。          |
| items  | array  | カート内の商品の配列。 |

### Item

| 名前      | 型      | 説明         |
| --------- | ------- | ------------ |
| itemId    | string  | 商品の ID。  |
| quantity  | integer | 商品の数量。 |
| unitPrice | number  | 商品の単価。 |
