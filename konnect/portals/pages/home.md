---
title: "Gorilla Store Dev Portal"
description: "Developer portal for Gorilla Store."
page-layout:
  sidebar-left: sidebar
---

::page-hero
---
title-color: "var(--kui-color-text-inverse)"
description-color: "rgba(255, 255, 255, 0.9)"
background: "linear-gradient(135deg, #3F51B5 0%, #673AB7 100%)"
border-radius: "24px"
padding: "clamp(40px, 6vw, 80px) clamp(30px, 5vw, 60px)"
text-align: "center"
vertical-align: "center"
title-tag: "h1"
title-font-size: "clamp(42px, 5.5vw, 64px)"
title-line-height: "clamp(50px, 6vw, 76px)"
title-font-weight: "800"
description-font-size: "clamp(18px, 2.5vw, 24px)"
description-line-height: "clamp(28px, 3vw, 36px)"
description-font-weight: "400"
margin: "0 0 var(--kui-space-80) 0"
styles: |
  .page-hero {
    box-shadow: 0 20px 60px rgba(63, 81, 181, 0.3);
  }
---

#title
🦍 Gorilla Store Dev Portal 🦍

#description
ゴリラのように強いAPIプラットフォーム。<br>
APIの検索、テスト、統合を一箇所で管理します。<br>
開発者の体験を最大化します。

#actions
  :::button
  ---
  appearance: "primary"
  size: "large"
  to: "/guides/getting-started"
  ---
  始める
  :::

  :::button
  ---
  appearance: "primary"
  size: "large"
  to: "/apis"
  ---
  APIを探す
  :::

::

::page-section
---
full-width: false
padding: "var(--kui-space-80) var(--kui-space-50)"
---

## 🎯 主な機能

<div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(300px, 1fr)); gap: var(--kui-space-70); margin-top: var(--kui-space-60);">

::card
---
title: "📚 APIカタログ"
---
利用可能なすべてのAPIを一箇所で検索・閲覧できます。詳細なドキュメントと共に提供されます。

[APIを探す →](/apis)
::

::card
---
title: "⚡ 高速な統合"
---
OpenAPI仕様に基づいた明確なドキュメントで、迅速な開発とスムーズな統合を実現します。

[Getting Started →](/guides/getting-started)
::

::card
---
title: "🔐 セキュアな認証"
---
OAuth 2.0、API Key、JWTなど、エンタープライズグレードの認証方式をサポートします。

[ガイドを見る →](/guides/getting-started)
::

</div>

::

::page-section
---
full-width: false
padding: "var(--kui-space-80) var(--kui-space-50)"
background: "var(--kui-color-background-neutral-weakest)"
---

## 🌟 提供中のAPI

現在利用可能な主要APIサービス

<div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(300px, 1fr)); gap: var(--kui-space-70); margin-top: var(--kui-space-60);">

::card
---
title: "🛍️ Catalogue API"
---
商品カタログの管理と検索機能を提供します。商品情報の取得、フィルタリング、在庫確認が可能です。

**バージョン:** v1.0.0
**認証:** API Key必須

[詳細を見る →](/apis)
::

::card
---
title: "🛒 Cart API"
---
ショッピングカート機能を提供します。カートアイテムの追加、更新、削除などの操作が可能です。

**バージョン:** v1.0.0
**認証:** API Key必須

[詳細を見る →](/apis)
::

::card
---
title: "📦 Order API"
---
注文処理と管理機能を提供します。注文の作成、ステータス確認、履歴閲覧が可能です。

**バージョン:** v1.0.0
**認証:** API Key必須

[詳細を見る →](/apis)
::

</div>

::

::page-section
---
full-width: false
padding: "var(--kui-space-80) var(--kui-space-50)"
---

## 🚦 クイックスタート

::alert
---
appearance: "info"
show-icon: 
message: "はじめての方へ: APIを使い始めるには、まず開発者アカウントを作成し、API Keyを取得してください。"
---
::

### 3ステップで始める

1. **アカウント登録**
   開発者ポータルにサインアップし、プロフィールを設定します。

2. **API Keyを取得**
   ダッシュボードから新しいアプリケーションを作成し、API Keyを発行します。

3. **APIを呼び出す**
   ドキュメントを参照して、最初のAPIリクエストを送信します。

::button
---
appearance: "primary"
size: medium
display: "inline-flex"
to: /getting-started
href: "/getting-started"
---
詳細なガイドを見る →
::

::

::page-section
---
full-width: false
padding: "var(--kui-space-80) var(--kui-space-50)"
background: "var(--kui-color-background-neutral-weakest)"
---

## 💡 サポートとリソース

<div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(280px, 1fr)); gap: var(--kui-space-70); margin-top: var(--kui-space-60);">

::card
---
title: "📖 ドキュメント"
---
各APIの詳細な仕様、サンプルコード、ベストプラクティスを提供しています。

[ドキュメントを見る →](/guides/getting-started)
::

::card
---
title: "🔄 APIライフサイクル"
---
API開発のライフサイクル、バージョニング、廃止ポリシーについて説明します。

[ライフサイクルガイド →](/guides/lifecycle)
::

</div>

::

::page-section
---
full-width: false
padding: "var(--kui-space-60) var(--kui-space-50)"
---

::snippet
---
name: "footer-support"
---
::

::
