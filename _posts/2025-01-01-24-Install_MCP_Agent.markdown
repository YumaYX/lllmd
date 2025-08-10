---
layout: post
title: "Install MCP Agent"
category: ""
---

## Pull MCP Agent

```sh
cd ; mkdir mcpollama && cd mcpollama
python3 -m venv .venv
source .venv/bin/activate
pip3 install --upgrade pip
pip3 install --upgrade ollmcp
```

- <https://github.com/jonigl/mcp-client-for-ollama>

## test run

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

- /home/user/myproject

```sh
mkdir ${HOME}/myproject
touch ${HOME}/myproject/sample{0..9}.txt

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
# -m llama3.2:1b

# > list files in /home/user/myproject
```


## Run MCP Server with qwen2.5:14b

```sh
ollama pull qwen2.5:7b
```

```sh
ollmcp -j /tmp/mcpconfig.json -m qwen2.5:14b
```

