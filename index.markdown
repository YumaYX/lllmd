---
layout: default
---

This series guides you through using Ollama, covering everything from installation and API integration to running experiments like a simple Retrieval-Augmented Generation (RAG) setup. Additionally, it includes instructions for related tools and environments, such as zram, Node.js, MCP Server, and MCP Agent

{% for post in site.posts reversed %}
1. [{{ index }} {{ post.title }}]({{ site.baseurl }}{{ post.url }}){% endfor %}
