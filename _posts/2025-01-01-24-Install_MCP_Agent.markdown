---
layout: post
title: "MCP Agentのインストール"
category: ""
---

## Install MCP Agent

MCPエージェントとは、MCPサーバーを通じて外部のデータや機能を利用しながら、LLMの応答や動作を制御する実行主体のことである。

`mcp-client-for-ollama`を使用する。venv上に環境を作る。

```sh
cd ; mkdir mcpollama && cd mcpollama
python3 -m venv .venv
source .venv/bin/activate
pip3 install --upgrade pip
pip3 install --upgrade ollmcp
```

- <https://github.com/jonigl/mcp-client-for-ollama>

## Test Running

MCPサーバーをかませず、実行してみる。`qwen2.5:7b`がデフォルトのため、事前にpullする。

```sh
ollama pull qwen2.5:7b

# start
ollmcp

# > type prompt
# => Warning: No tools are enabled. Model will respond without tool access.
```

- <https://ollama.com/library/qwen2.5>

## Run with MCP Server

### Make a Server Config

- 許可するディレクトリ
  - /home/user/myproject
- 出力するサーバーコンフィグの場所
  - /tmp/mcpconfig.json

```sh
mkdir ${HOME}/myproject
touch ${HOME}/myproject/sample{0..9}.txt
```

```sh
cat <<'EOF' > /tmp/mcpconfig.json
{
  "mcpServers": {
    "file-system": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-filesystem",
        "/home/user/myproject"
      ],
      "disabled": false
    }
  }
}
EOF
```

### Test with MCP Server(File System)

```sh
ollmcp -j /tmp/mcpconfig.json
# > list files in /home/user/myproject
```

`list files`だけでは足りず、ディレクトリの指定を求められた。

## Run MCP Server with qwen2.5:14b

```sh
ollama pull qwen2.5:14b
```

```sh
ollmcp -j /tmp/mcpconfig.json -m qwen2.5:14b
```


### GUI/デスクトップをアプリを使わない理由

理由としては、いくつかある。

- CUI環境で、インストール/動作が不可能になる。それを避けるため
  - 同じステップで、再現性を持たせる
- ラッパーされている箇所をできる限り、シンプルな構成で、構成や挙動を理解するため