---
layout: post
title: "Ollama with API"
category: ""
---

# Ollama with API

Ollama をローカル環境で API 経由で使用する方法について説明します。チャット形式やプロンプト形式でのリクエスト、Ruby によるプログラム例、レスポンスからコードブロックだけを抽出する方法などを紹介します。

## 環境

- ホスト: `localhost`
- ポート: `11434`
- 使用モデル: `gemma3`

---

## 1. チャット形式でのAPI呼び出し

### エンドポイント: `/api/chat`

ユーザーとアシスタントのメッセージを `messages` 配列で送信します。

```sh
curl http://localhost:11434/api/chat \
  -H "Content-Type: application/json" \
  -d '{
    "model": "gemma3",
    "messages": [
      { "role": "user", "content": "write a simple sample code with ruby." }
    ],
    "stream": false
  }' | jq
````

---

## 2. 会話履歴付きチャット

会話履歴を保持して文脈のある応答を得る方法です。

```sh
curl http://localhost:11434/api/chat \
  -H "Content-Type: application/json" \
  -d '{
  "model": "gemma3",
  "messages": [
    {"role": "system", "content": "あなたは旅行や季節の行事に詳しい、親切で穏やかなアシスタントです。"},
    {"role": "user", "content": "そろそろ春だね〜、お花見行きたいなあ。"},
    {"role": "assistant", "content": "いいですね！桜の季節は本当にわくわくしますよね。"},
    {"role": "user", "content": "おすすめの桜の名所ってある？できれば日本国内で！"}
  ],
  "stream": false
}' | jq
```

---

## 3. プロンプト形式でのAPI呼び出し

### エンドポイント: `/api/generate`

プロンプトのみで応答を生成します。

```sh
curl http://localhost:11434/api/generate \
  -H "Content-Type: application/json" \
  -d '{
    "model": "gemma3",
    "prompt": "write a simple sample code with ruby.",
    "stream": false
  }' | jq
```

---

## 4. RubyでのAPI呼び出し

Ruby を使って `/api/chat` にPOSTリクエストを送る例です。

```ruby
require 'net/http'
require 'uri'
require 'json'

url = URI.parse('http://localhost:11434/api/chat')

data = {
  model: "gemma3",
  messages: [
    { role: "user", content: "write a simple sample code with ruby." }
  ],
  stream: false
}

http = Net::HTTP.new(url.host, url.port)
request = Net::HTTP::Post.new(url.path, { 'Content-Type' => 'application/json' })
request.body = data.to_json

response = http.request(request)

puts "Status: #{response.code}"
puts "Body: #{response.body}"
```

> 💡 `prompt` ではなく `messages` を使う必要があります（APIが `/api/chat` のため）。

---

## 5. コードブロックの抽出

生成結果からMarkdownのコードブロック（\`\`\`で囲まれた部分）だけを取り出すには、以下のような `awk` コマンドを使います。

````sh
awk '/^```/{in_block = !in_block; next} in_block' file.md
````

このスクリプトは、コードブロックの開始と終了を検出し、その間の行のみを出力します。
