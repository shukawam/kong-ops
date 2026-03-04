# エージェント API

## エンドポイント

### POST /agent

- **概要:** エージェントにメッセージを送り、回答を取得します。
- **リクエストボディ:**
  - **コンテンツ:** `application/json`
    - **スキーマ:** `AgentRequest`
    - **備考:** `prompt` または `messages` のいずれかを必ず指定してください。
- **レスポンス:**
  - `200 OK`: エージェントからの回答。
    - **コンテンツ:** `application/json`
      - **スキーマ:** `AgentResponse`
  - `503 Service Unavailable`: MCPサーバーが利用できません。
  - `500 Internal Server Error`: 予期しないエラーが発生しました。

## スキーマ

### AgentRequest

| 名前     | 型     | 必須 | 説明                                                      |
| -------- | ------ | ---- | --------------------------------------------------------- |
| prompt   | string | -    | エージェントへの入力テキスト。                            |
| messages | array  | -    | 会話履歴。`Message` の配列。`prompt` の代わりに使用可能。 |

### Message

| 名前    | 型     | 説明                                            |
| ------- | ------ | ----------------------------------------------- |
| role    | string | メッセージの送信者。`user` または `assistant`。 |
| content | string | メッセージの本文。                              |

### AgentResponse

| 名前     | 型     | 説明                     |
| -------- | ------ | ------------------------ |
| response | string | エージェントからの回答。 |
