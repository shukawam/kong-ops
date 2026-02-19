---
title: "API Usage Guide"
description: "API利用ガイド - ベストプラクティスとパターン"
page-layout:
  sidebar-left: sidebar
---

::page-section
---
full-width: false
padding: "var(--kui-space-80) var(--kui-space-50)"
---

# ⚡ API利用ガイド

Toshogu APIを効果的に活用するためのベストプラクティス、パターン、ヒントを紹介します。

---

## 🔐 認証

### API Keyの管理

::alert
---
type: "warning"
---
**セキュリティ第一:** API Keyは絶対に公開しないでください。
::

#### 環境変数の使用

**✅ 推奨:**

```bash
# .env ファイル
API_KEY=your_api_key_here
API_BASE_URL=https://api.toshogu.example.com
```

```javascript
// アプリケーションコード
const apiKey = process.env.API_KEY;
```

**❌ 非推奨:**

```javascript
// コードに直接記述しない
const apiKey = "abc123def456";
```

#### API Keyのローテーション

定期的なローテーションを推奨：

1. ダッシュボードで新しいKeyを生成
2. アプリケーションを新しいKeyに更新
3. 旧Keyが使用されていないことを確認
4. 旧Keyを削除

**推奨頻度:** 3-6ヶ月ごと

---

## 🚀 リクエストのベストプラクティス

### HTTPメソッドの使い分け

| メソッド | 用途 | べき等性 | キャッシュ | ボディ |
|---------|------|---------|----------|-------|
| **GET** | データの取得 | あり | 可能 | なし |
| **POST** | 新規リソースの作成 | なし | 不可 | あり |
| **PUT** | リソースの完全更新 | あり | 不可 | あり |
| **PATCH** | リソースの部分更新 | なし | 不可 | あり |
| **DELETE** | リソースの削除 | あり | 不可 | なし |

### ヘッダーの設定

**必須ヘッダー:**

```http
X-API-Key: your_api_key
Content-Type: application/json
Accept: application/json
```

**推奨ヘッダー:**

```http
User-Agent: MyApp/1.0.0
Accept-Language: ja
X-Request-ID: unique-request-id
```

---

## 📊 ページネーション

大量のデータを効率的に取得する方法：

### カーソルベースのページネーション

**推奨方式** - パフォーマンスが高く、リアルタイム性がある

```bash
# 最初のページ
GET /v1/products?limit=20

# 次のページ
GET /v1/products?limit=20&cursor=eyJpZCI6MTIzfQ
```

**レスポンス:**

```json
{
  "data": [...],
  "pagination": {
    "next_cursor": "eyJpZCI6MTQzfQ",
    "has_more": true
  }
}
```

**実装例:**

```javascript
async function fetchAllProducts() {
  let allProducts = [];
  let cursor = null;

  do {
    const url = cursor
      ? `${API_BASE}/products?limit=20&cursor=${cursor}`
      : `${API_BASE}/products?limit=20`;

    const response = await fetch(url, {
      headers: { 'X-API-Key': API_KEY }
    });
    const data = await response.json();

    allProducts = allProducts.concat(data.data);
    cursor = data.pagination.next_cursor;
  } while (cursor);

  return allProducts;
}
```

### オフセットベースのページネーション

**簡単だが大量データには不向き**

```bash
# 1ページ目
GET /v1/products?limit=20&offset=0

# 2ページ目
GET /v1/products?limit=20&offset=20
```

---

## 🔍 フィルタリングとソート

### クエリパラメータ

**フィルタリング:**

```bash
# 単一条件
GET /v1/products?category=electronics

# 複数条件
GET /v1/products?category=electronics&inStock=true

# 範囲指定
GET /v1/products?minPrice=1000&maxPrice=5000
```

**ソート:**

```bash
# 昇順
GET /v1/products?sort=price

# 降順
GET /v1/products?sort=-price

# 複数フィールド
GET /v1/products?sort=category,-price
```

**フィールド選択:**

```bash
# 必要なフィールドのみ取得（パフォーマンス向上）
GET /v1/products?fields=id,name,price
```

---

## ⚡ パフォーマンス最適化

### 1. キャッシング

**HTTPキャッシュヘッダー:**

```http
Cache-Control: public, max-age=3600
ETag: "33a64df551425fcc55e4d42a148795d9f25f89d4"
```

**実装例:**

```javascript
const cache = new Map();

async function fetchWithCache(url) {
  // キャッシュチェック
  if (cache.has(url)) {
    const cached = cache.get(url);
    if (Date.now() - cached.timestamp < 3600000) { // 1時間
      return cached.data;
    }
  }

  // APIコール
  const response = await fetch(url, {
    headers: { 'X-API-Key': API_KEY }
  });
  const data = await response.json();

  // キャッシュ保存
  cache.set(url, {
    data,
    timestamp: Date.now()
  });

  return data;
}
```

### 2. バッチリクエスト

複数のリソースを一度に取得：

```bash
# ❌ 個別リクエスト（非推奨）
GET /v1/products/1
GET /v1/products/2
GET /v1/products/3

# ✅ バッチリクエスト（推奨）
GET /v1/products?ids=1,2,3
```

### 3. 並行リクエスト

JavaScriptの例：

```javascript
// 並行実行
const [products, categories, orders] = await Promise.all([
  fetch('/v1/products').then(r => r.json()),
  fetch('/v1/categories').then(r => r.json()),
  fetch('/v1/orders').then(r => r.json())
]);
```

---

## 🔒 レート制限

### 制限値

| プラン | リクエスト/時間 | 1秒あたり | バースト |
|--------|--------------|----------|---------|
| Free | 1,000 | ~0.3 | 50 |
| Pro | 10,000 | ~3 | 200 |
| Enterprise | カスタム | カスタム | カスタム |

### レスポンスヘッダー

```http
X-RateLimit-Limit: 1000
X-RateLimit-Remaining: 999
X-RateLimit-Reset: 1640995200
```

### レート制限のハンドリング

```javascript
async function fetchWithRetry(url, maxRetries = 3) {
  for (let i = 0; i < maxRetries; i++) {
    const response = await fetch(url, {
      headers: { 'X-API-Key': API_KEY }
    });

    if (response.status === 429) {
      // レート制限に到達
      const resetTime = response.headers.get('X-RateLimit-Reset');
      const waitTime = (resetTime * 1000) - Date.now();

      console.log(`Rate limited. Waiting ${waitTime}ms...`);
      await new Promise(resolve => setTimeout(resolve, waitTime));
      continue;
    }

    return response.json();
  }

  throw new Error('Max retries exceeded');
}
```

---

## 🛠️ エラーハンドリング

### HTTPステータスコード

**2xx Success**

- **200 OK**: リクエスト成功
- **201 Created**: リソース作成成功
- **204 No Content**: 成功、レスポンスボディなし

**4xx Client Error**

- **400 Bad Request**: 不正なリクエスト
- **401 Unauthorized**: 認証失敗
- **403 Forbidden**: 権限不足
- **404 Not Found**: リソースが存在しない
- **429 Too Many Requests**: レート制限

**5xx Server Error**

- **500 Internal Server Error**: サーバー内部エラー
- **502 Bad Gateway**: ゲートウェイエラー
- **503 Service Unavailable**: サービス利用不可
- **504 Gateway Timeout**: タイムアウト

### エラーレスポンスの構造

```json
{
  "error": {
    "code": "RESOURCE_NOT_FOUND",
    "message": "The requested product was not found",
    "details": {
      "productId": "123",
      "suggestion": "Check if the product ID is correct"
    },
    "request_id": "req_abc123"
  }
}
```

### エラーハンドリングの実装

```javascript
async function handleApiRequest(url) {
  try {
    const response = await fetch(url, {
      headers: { 'X-API-Key': API_KEY }
    });

    if (!response.ok) {
      const error = await response.json();

      switch (response.status) {
        case 400:
          throw new Error(`Bad Request: ${error.error.message}`);
        case 401:
          throw new Error('Unauthorized: Check your API key');
        case 404:
          throw new Error(`Not Found: ${error.error.message}`);
        case 429:
          throw new Error('Rate limit exceeded');
        case 500:
          throw new Error('Server error: Please try again later');
        default:
          throw new Error(`Error ${response.status}: ${error.error.message}`);
      }
    }

    return response.json();
  } catch (error) {
    console.error('API request failed:', error);
    throw error;
  }
}
```

---

## 💡 実践的なパターン

### リトライロジック（エクスポネンシャルバックオフ）

```javascript
async function fetchWithExponentialBackoff(url, maxRetries = 3) {
  for (let i = 0; i < maxRetries; i++) {
    try {
      const response = await fetch(url, {
        headers: { 'X-API-Key': API_KEY }
      });

      if (response.ok) {
        return response.json();
      }

      // 5xxエラーの場合のみリトライ
      if (response.status >= 500) {
        const delay = Math.pow(2, i) * 1000; // 1s, 2s, 4s...
        console.log(`Retry ${i + 1}/${maxRetries} after ${delay}ms`);
        await new Promise(resolve => setTimeout(resolve, delay));
        continue;
      }

      // 4xxエラーはリトライしない
      throw new Error(`HTTP ${response.status}`);
    } catch (error) {
      if (i === maxRetries - 1) throw error;
    }
  }
}
```

---

::snippet
---
name: "footer-support"
---
::

::
