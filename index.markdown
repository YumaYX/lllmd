---
layout: default
---

このシリーズでは、Ollama の使用方法をガイドします。インストールや API 統合から、シンプルな RAG（Retrieval-Augmented Generation）セットアップのような実験の実行までを網羅しています。さらに、zram、Node.js、MCP Server、MCP Agent などの関連ツールや環境に関する手順も含まれています。

{% for post in site.posts reversed %}
1. [{{ index }} {{ post.title }}]({{ site.baseurl }}{{ post.url }}){% endfor %}

`lllmd`: Local Large Language Model Documents
