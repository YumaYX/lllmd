---
layout: post
title: "MCP-to-OpenAPI Proxy Serverの使用"
category: ""
---

# MCP-to-OpenAPI Proxy Server

以前の記事では、特定のLLMのモデルしか使用できなかった。それ以外のモデルを使用したいため、環境を構築する。

ほとんど、リファレンスを参考にした。

##### Reference

[Open WebUIとMCPOでローカルLLMにMCPツールを使ってもらう](https://zenn.dev/pkkudo/articles/mcpo-for-local-llm)

---

### 構成

構成を簡単に表現する。すべて同じホスト上に組み立てる。

`ホスト(クライアント)` <=> `Open webui` <=> `MCPO` => `MCPサーバー`

---

## 0. ファイアウォール設定

```sh
systemctl start firewalld
firewall-cmd --permanent --zone=public --add-port=11434/tcp # for ollama
#firewall-cmd --permanent --zone=public --add-port=8080/tcp # for open-webui

firewall-cmd --reload
```

- `Open WebUI` -> `MCPO`経由の`8000`は、ポートを開けなくても疎通する（事実）。
  - 外部の物理IPからアクセスしたときは、Open WebUIまでしか辿りつかない。

---

## 1. MCPサーバーのインストール

MCPサーバーは、以前の記事にて、インストールを行う。もちろん他の方法もあるが、実績があるため、同じものを使用する。

そのため、本稿では説明をスキップ。

---

## 2. MCPOサーバーのインストール

#### MCPOサーバーとは？

MCPOは、MCP-to-OpenAPI Proxyのこと。MCPOサーバーであるmcpoについて、

> Mcpo is a simple proxy server that bridges the gap between MCP (Model Context Protocol) servers and OpenAPI, enabling seamless integration with LLM agents and applications that expect standard RESTful APIs.

プロクシに仲介してもらわないと、MCPサーバーのツールがうまく動作しない。

##### References

- <https://github.com/open-webui/mcpo>
- <https://juniarto-samsudin.medium.com/building-custom-mcp-tool-and-integrate-it-to-your-self-hosted-llm-through-openwebui-part-3-3268c4fcac6e>


### MCPOサーバーのインストールコマンド

参考先のコマンドをほとんど参考にしている。

```sh
mkdir -p ~/svc/mcpo; cd ~/svc/mcpo
python3 -m venv .venv && source .venv/bin/activate && pip install -U pip
pip install mcpo ; pip install -U mcpo
```

### MCPOサーバー起動

MCPサーバーのコンフィグを指定する。

```sh
mcpo --port 8000 --host 0.0.0.0 --config /tmp/mcpconfig.json
```

---

## 3. Open WebUI

プロンプト入力画面として、Open WebUIを採用する。

できる限りGUIを避けたかった、コンテナの使用を避けたいが、シンプルな構成（構築など必要が無いというところ）で、構成を作れると考え、採用。

\# コンテナを使ってしまうと、何が起きてるか追いにくくなる、コンテナ依存で、オーソドックスな手法が使えなくなる可能性があるため、とても嫌う。

以下のコマンドを実行して、Open WebUIを、動作させる。もちろんCPU onlyモードのため、環境に合わせる。podmanのインストールは[こちら](https://yumayx.github.io/Workshop/%E3%82%B3%E3%83%B3%E3%83%86%E3%83%8A/2024/01/01/06-8-Podman%E3%82%B3%E3%83%9E%E3%83%B3%E3%83%89-%E4%BD%BF%E7%94%A8%E4%BE%8B.html)を参考にする。

```sh
podman stop open-webui
podman rm open-webui

podman run -d --network=host -v open-webui:/app/backend/data -e OLLAMA_BASE_URL=http://127.0.0.1:11434 --name open-webui --restart always ghcr.io/open-webui/open-webui:main
```

#### References

- <https://github.com/open-webui/open-webui>

### 起動と設定手順/項目

1. `localhost:8080`にアクセス
1. get started、新規登録。適当に入力。おそらく外部のデータベースとは繋がっていないように見える（見えるだけ）。
1. Ollamaで使用するモデルをダウンロードする。
    1. ホストのollamaを使用するようにしたため、`ollama pull gemma3:latest`などで、pullする。
1. MCPツールの設定
    1. ユーザーの`settings`から`tools`を選択
    1. "+"をクリックしてサーバ追加
        - tool名が`fetch`なら、`http://localhost:8000/fetch`を入力。
        - 更新ボタンを押して、問題ないか確認できる。
        - 使用するツール分だけ追加する。

#### その他設定

- `ctx_num`を大きくすると良いのかも。

---

### 確認

以下の設定が済んだら、確認する。

1. MCPサーバー
1. MCPOサーバー
1. Open WebUI

以下のプロンプトなどで、確認できる。

```
access <URL>, and summarize it.
```

---

## サーバー構成確認

1. ホストから、Open webuiにアクセスする。プロンプトを入力する。
1. Ollamaが使用される。（このOllamaはどこ？ => ホストのOllama）
    1. 必要に応じて、Ollamaのモデルが、エージェント経由で、MCPOのAPIを叩く。
    1. MCPサーバーのツールが動く。
    1. Open webuiに返す。

---

Ollamaのモデルを使って、基本的なMCPサーバーを操作することができるようになった。
