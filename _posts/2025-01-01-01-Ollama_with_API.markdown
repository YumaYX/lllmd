---
layout: post
title: "Ollama with API"
category: ""
---

# Ollama with API

## environments

- host: `localhost`
- port: `11434`
- model: `gemma3`

### by chat

`/api/chat`

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
```

### chat with history

`/api/chat`

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

### by prompt

`/api/generate`

```sh
curl http://localhost:11434/api/generate \
  -H "Content-Type: application/json" \
  -d '{
    "model": "gemma3",
    "prompt": "write a simple sample code with ruby.",
    "stream": false
  }' | jq
```

## program with ruby

```ruby
require 'net/http'
require 'uri'
require 'json'

# 送信先のURL
url = URI.parse('http://localhost:11434/api/chat')

# 送信するJSONデータ
data = {
    "model": "gemma3",
    "prompt": "write a simple sample code with ruby.",
    "stream": false
}

# HTTPリクエストを作成
http = Net::HTTP.new(url.host, url.port)
http.use_ssl = (url.scheme == "https")

request = Net::HTTP::Post.new(url.path, {
  'Content-Type' => 'application/json'
})

# JSONデータをボディに設定
request.body = data.to_json

# リクエスト送信 & レスポンス取得
response = http.request(request)

# レスポンス出力
puts "Status: #{response.code}"
puts "Body: #{response.body}"
```

### Commands

#### コードブロックからの抽出

スクリプト、プログラムのみ指定しても、それ以外を出力することが多くある。コードブロックを抽出する必要がある。

```
awk '/^```/{in_block = !in_block; next} in_block' file.md
```
