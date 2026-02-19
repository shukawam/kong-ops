---
title: "Getting Started"
description: "Toshogu Dev Portalの使い方ガイド"
page-layout:
  sidebar-left: sidebar
---

::page-section
---
full-width: false
padding: "var(--kui-space-80) var(--kui-space-50)"
---

# 🚀 Getting Started

Toshogu Dev Portalへようこそ！このガイドでは、APIの利用開始から実際の統合まで、ステップバイステップで説明します。

::alert
---
appearance: "success"
show-icon: true
message: "所要時間: 約15分でAPIの呼び出しまで完了できます。"
---
::

---

## 📋 前提条件

開始する前に、以下をご用意ください：

- **開発者アカウント** - ポータルへのアクセス権限
- **開発環境** - curl、httpie、Insomnia、または任意のHTTPクライアント
- **基本的な知識** - REST API、HTTP、JSONの基礎知識

---

## ステップ1: アカウントの作成

### 1.1 サインアップ

1. [サインアップページ](https://portal.toshogu.example.com/signup)にアクセス
2. 必要事項を入力（メールアドレス、会社名など）
3. 確認メールのリンクをクリックして、アカウントを有効化

### 1.2 プロフィール設定

ダッシュボードにログイン後：

- プロフィール情報を完成させる
- 開発チームのメンバーを追加（オプション）
- 通知設定を構成

::alert
---
appearence: "success"
show-icon: true
message: "プロフィールを完成させることで、より良いサポートを受けることができます。"
---
::

---

## ステップ2: アプリケーションの作成

### 2.1 新規アプリケーション

1. ダッシュボードの「**Applications**」タブに移動
2. 「**Create New Application**」をクリック
3. アプリケーション情報を入力：
   - **Name**: アプリケーション名（例: "My Shopping App"）
   - **Description**: 用途の説明
   - **Redirect URLs**: OAuth利用時のコールバックURL

### 2.2 API Keyの取得

アプリケーション作成後、自動的にAPI Keyが生成されます：

```bash
# API Key の例
API_KEY=abc123def456ghi789jkl012mno345pqr678
```

::alert
---
appearance: "danger"
show-icon: true
title: "重要"
message: "API Keyは安全に保管してください。決してクライアントサイドのコードやパブリックリポジトリにコミットしないでください。"
---
::

---

## ステップ3: 最初のAPIリクエスト

### 3.1 Catalogue APIのテスト

最も簡単なエンドポイントから始めましょう：

```bash
curl -X GET "https://api.toshogu.example.com/catalogue/v1/products" \
  -H "X-API-Key: YOUR_API_KEY" \
  -H "Content-Type: application/json"
```

**期待されるレスポンス:**

```json
{
  "products": [
    {
      "id": "prod_001",
      "name": "Sample Product",
      "price": 1999,
      "currency": "JPY",
      "inStock": true
    }
  ],
  "total": 1,
  "page": 1
}
```

### 3.2 認証の確認

HTTPステータスコードを確認：

- **200 OK**: リクエスト成功
- **401 Unauthorized**: API Keyが無効または期限切れ
- **403 Forbidden**: アクセス権限がない
- **429 Too Many Requests**: レート制限に到達

---

## ステップ4: API統合

### 4.1 基本的な統合パターン

**商品の取得 → カートに追加 → 注文作成**

```javascript
// 1. 商品一覧を取得
const products = await fetch('https://api.toshogu.example.com/catalogue/v1/products', {
  headers: {
    'X-API-Key': process.env.API_KEY
  }
});

// 2. カートに商品を追加
const cart = await fetch('https://api.toshogu.example.com/cart/v1/items', {
  method: 'POST',
  headers: {
    'X-API-Key': process.env.API_KEY,
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({
    productId: 'prod_001',
    quantity: 1
  })
});

// 3. 注文を作成
const order = await fetch('https://api.toshogu.example.com/order/v1/orders', {
  method: 'POST',
  headers: {
    'X-API-Key': process.env.API_KEY,
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({
    cartId: cart.id
  })
});
```

### 4.2 エラーハンドリング

```javascript
try {
  const response = await fetch(apiUrl, options);

  if (!response.ok) {
    // HTTPエラーの処理
    const error = await response.json();
    console.error('API Error:', error.message);
    throw new Error(error.message);
  }

  const data = await response.json();
  return data;
} catch (error) {
  // ネットワークエラーなどの処理
  console.error('Request failed:', error);
  throw error;
}
```

---

## ステップ5: ベストプラクティス

### 🔐 セキュリティ

- **環境変数を使用**: API Keyはコードに直接記述しない
- **HTTPS接続**: 常にHTTPSを使用
- **Key Rotation**: 定期的にAPI Keyをローテーション

### ⚡ パフォーマンス

- **キャッシング**: 頻繁に変更されないデータはキャッシュ
- **バッチ処理**: 可能な限り複数のリクエストをまとめる
- **レート制限**: 制限内でのリクエスト送信を計画

### 📊 モニタリング

- **ログ記録**: すべてのAPIリクエストをログ
- **メトリクス**: レスポンスタイム、エラー率を監視
- **アラート**: 異常なパターンを検知

---

## 🆘 トラブルシューティング

### よくある問題

::accordion-group
  ::accordion-panel
  #header
  401 Unauthorized エラーが発生する
  #default
  - API Keyが正しく設定されているか確認
  - API Keyの有効期限を確認
  - ヘッダー名が正しいか確認（`X-API-Key`）
  ::
  ::accordion-panel
  #header
  レート制限に到達した
  #default
  - 現在のレート制限: 1000リクエスト/時間
  - `X-RateLimit-Remaining`ヘッダーで残りを確認
  - 必要に応じてプランのアップグレードを検討
  ::
  ::accordion-panel
  #header
  タイムアウトが発生する
  #default
  - ネットワーク接続を確認
  - APIのステータスページを確認
  - タイムアウト値を調整（推奨: 30秒）
  ::
::

---

## 📚 次のステップ

おめでとうございます！これでAPIを使い始める準備が整いました。

<div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(300px, 1fr)); gap: var(--kui-space-70); margin-top: var(--kui-space-60);">

::card
---
title: "📖 API ドキュメント"
---
各APIの詳細な仕様を確認

[ドキュメントを見る →](/apis)
::

::card
---
title: "🔄 ライフサイクル"
---
APIバージョニングと廃止ポリシー

[ガイドを見る →](/guides/lifecycle)
::

::card
---
title: "📋 利用規約"
---
利用ガイドラインとポリシー

[規約を確認 →](/guides/regulation)
::

</div>

---

::snippet
---
name: "footer-support"
---
::

::

