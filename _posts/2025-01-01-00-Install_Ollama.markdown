---
layout: post
title: "Install Ollama"
category: ""
---

# installing ollama with linux

```sh
# https://ollama.com
curl -fsSL https://ollama.com/install.sh | sh
ollama run gemma3 "hello"
```

# 前提のtips

- `serve`が動いていれば、APIを使用できる。
  - runした後、プロンプトを抜けても裏でserveが動いている。
  - `systemctl <control> ollama`で、サーバーの操作が可能
  - モデルをpullしておけば、apiから呼び出せる。
- ハードと用途を考えてモデルを選ぶ。
- streamが初期設定となっている。`stream`を`false`にすると、使いやすいだろう。
- apiを使うには、postで、bodyにjsonをつけて送る。　

# Reference

- <https://ollama.com>
- <https://developer.mamezou-tech.com/blogs/2025/02/20/ollama_local_llm/>
