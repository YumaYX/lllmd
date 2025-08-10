---
layout: post
title: "Install MCP Server"
category: ""
---

# Install MCP Server File System

```sh
# as root or with sudo
npm install -g @modelcontextprotocol/server-filesystem
```

- 古いnodejsでは、インストールできなかったが、v22でインストールできた（依存関係でエラーと考えられる）。
- 一般ユーザーでは、パーミッション関連のエラーメッセージが表示された。そのためroot実行(今回はsudo)した。

