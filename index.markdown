---
layout: default
---

This article is part of a series exploring how to use Ollama, from installation and API integration to running experiments such as a simple Retrieval-Augmented Generation (RAG) setup and more.

{% for post in site.posts reversed %}
1. [{{ index }} {{ post.title }}]({{ site.baseurl }}{{ post.url }}){% endfor %}
