---
layout: post
title: "Experiments"
category: ""
---

# Experiments

## 1. 自動改善ループ実験

### 概要

LLM（大規模言語モデル）を用いた改善ループ、モデルサイズによる実行時間、RAG（Retrieval-Augmented Generation）の限界についての実験記録。

プログラムを自動生成し、それを実行 → エラーや改善提案をプロンプトとして再入力 → 再生成 …という改善ループを試みた。

### 結果

- 同じ出力が繰り返され、改善が収束する現象が確認された。
- 同じプロンプトに対して、同じ出力が返るようになり、ループの効果が頭打ちに。
- 使用したモデルが小さく、モデルサイズが影響している可能性がある。

より大規模なモデルを使用することで、ループ改善能力が向上する可能性あり。

---

## 2. モデルサイズと応答時間

### 実験目的

モデルのサイズと応答時間の関係を観察。

### 実行環境

- **CPU**: Intel(R) Core(TM) i5-8250U @ 1.60GHz（CPUのみ）
- **メモリ**: 7.7GB（Swap: 40GB）

### 使用プロンプト

```text
hello
```

### モデル別応答時間（`gemma3`）

| モデル        | サイズ  | 実行時間               |
| ---------- | ---- | ------------------ |
| gemma3:1b  | 小    | 約 3 秒 (`0:03.31`)  |
| gemma3:4b  | 中    | 約 14 秒 (`0:14.02`) |
| gemma3:12b | 大    | 約 5 分半 (`5:37.88`) |
| gemma3:27b | 非常に大 | 約 1時間半 (`1:33:13`) |

大きなモデルは推論に大幅な時間がかかる。実験には時間リソースと十分な計算資源が必要。

---

## 3. RAGを用いたスクリプト理解の限界

### 試行内容

RAGを用いて、Rubyのドキュメントをベースに、スクリプトの内容を理解・説明させる実験。

### 問題点と観察

* サンプルスクリプトが断片的で、ベクトル検索(FAISS)で十分な類似度が得られなかった。
  * 検索対象が少ない場合は、ヒットしたが、800ファイルにしたときに、ヒットしてほしい特定のテキスト断片(チャンク?)にヒットしなくなった。
* そのため、意図したドキュメントが検索フェーズでヒットせず、誤った説明が生成される場合があった。

### 最終的に

irbで実行させた結果を`gemma3:27b`に食わせて、説明させた。CPU onlyモードであるため、３、４日かかった。

- <https://yumayx.github.io/rubydemodoc/>

---

## 4. MCPクライアント、MCPサーバーの導入

1ショットのプログラムの生成を目指す。

- クライアントに[mcp-client-for-ollama](https://github.com/jonigl/mcp-client-for-ollama)を導入
- [MCPサーバー](https://github.com/modelcontextprotocol/servers)のeverything, file system, fetchを導入

`qwen2.5`, `qwen3`を中心に1ショットのプログラム生成を目指してプロンプトの流し込み実施。モデルの大きさは14b以下。
MCP ServerのToolを使うようにプロンプトを書いても、Toolを使う頻度が少ない。そのためファイル出力や、外部からの情報取得の頻度が少なく、MCPの導入の効果が薄い状態。

改善する箇所としては、

- モデルについて
  - モデルが小さいか、ハードウェアのリソースが必要
  - プロトコルを作っているClaude系でないとうまくいかないのか。
- プロンプトが悪いか
  - 1ショット目指すよりは、小刻みに生成するのか

などが考えられる。ハードウェアリソースが無いため、試行回数限られてくる。

---

## 5. gemma3:27b x MCPサーバー

以下のプロンプトで、実行。

```markdown
- implement python programs and output to files
  - directory is in /home/user/myproject.
  - supply the changes to files by using mcp server tools: write_file, edit_file, create_directory.
  - you can access internet by using mcp server tool: fetch, if you need.
- program behavior
  * Using `https://api.openbd.jp/v1/get?isbn=`, create a function that takes an ISBN, accesses the URL, and retrieves the JSON.
  * Create a function that formats and outputs the retrieved JSON nicely.
  * Using the ISBNs of three books as examples, display the information.
```

- ほとんど指示通りに作成された。３つのサンプルのISBNはハルシーネーションで出鱈目。
- MCPサーバーでの、ファイルは自動で、出力はされなかった。

ハードウェアリソースの限界。
