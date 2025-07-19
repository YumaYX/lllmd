---
layout: post
title: "Install Ollama"
category: ""
---

# Ollama の Linux へのインストールと基本使用法

## インストール手順

以下のコマンドで、Ollama を Linux にインストールできます。
```sh
# 公式サイト: https://ollama.com
curl -fsSL https://ollama.com/install.sh | sh
```

インストール後、以下のようにしてモデルを実行可能です。
`ollama run gemma3 "hello"`
このコマンドで gemma3 モデルを使って "hello" を送信します。

## 使用前に知っておくべきTips（前提事項）

1. APIはバックグラウンドでserveが動いていれば利用可能
  `ollama run`を実行すると、自動的にバックグラウンドで serve が起動します。
  プロンプトを抜けても serve は動き続け、API経由でモデルを呼び出せる状態が維持されます。
1. systemctl でサーバー状態の操作が可能
  Ollama サーバーの状態を制御するには以下のコマンドを使用します。
  `sudo systemctl status ollama   # 状態の確認` `sudo systemctl start ollama    # サーバーの起動` `sudo systemctl stop ollama     # サーバーの停止`

1. モデルは事前に pull しておく必要がある
  使用したいモデルは、事前にローカルにダウンロード（pull）しておきます。
  `ollama pull gemma3`
  pull 済みのモデルであれば、APIやCLIからすぐに利用可能です。

1. モデルはハードウェアと用途に応じて選択する
  大規模モデル（例: llama3）は、メモリやGPU性能を多く消費します。
  小規模モデル（例: gemma、mistral）は、比較的軽量でローカル実行しやすいです。
  使用用途（チャット、コード補完など）や、PCスペックに応じてモデルを選びましょう。

1. APIレスポンスはデフォルトで stream モード
  デフォルトではストリーミング（逐次出力）形式で応答します。
  一括出力が欲しい場合は、APIリクエストの body に以下のように設定します。
```sh
{
  "prompt": "こんにちは",
  "stream": false
}
```
1. APIの基本的な使い方
  APIは HTTP POST でアクセスし、JSON 形式の body を送信します。
  例：
```sh
curl http://localhost:11434/api/generate -d '{
  "model": "gemma3",
  "prompt": "こんにちは",
  "stream": false
}'
```

# Reference

- <https://ollama.com>
- <https://developer.mamezou-tech.com/blogs/2025/02/20/ollama_local_llm/>
