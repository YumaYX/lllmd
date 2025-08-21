---
layout: post
title: "Install MCP Server"
category: ""
---

# Install MCP Server File System

Node.jsを使用して、MCPサーバーをインストールする。

## MCPサーバーとは

MCPサーバーとは、大規模言語モデル（LLM）が外部のツールやデータソースとやり取りできるようにする仕組みを提供するサーバーのことである。MCP（Model Context Protocol）は、LLMと外部リソースを安全かつ標準的な形式で接続するためのプロトコルとして設計されており、MCPサーバーはその仲介役を担う。開発者は自分のシステムやデータベース、APIなどをMCPサーバーとして公開することで、LLMから自然言語のやり取りを通じて呼び出せるようになる。これにより、LLMは単なるテキスト生成モデルにとどまらず、外部の機能や情報を統合して応答を強化できる。

## Install MCP Server

MCPサーバーをインストールする。

```sh
# as root or with sudo
npm install -g @modelcontextprotocol/server-filesystem
```

- 古いnodejsでは、インストールできなかったが、v22でインストールできた（依存関係でエラーと考えられる）。
- 一般ユーザーでは、パーミッション関連のエラーメッセージが表示された。そのためroot実行(今回はsudo)した。
