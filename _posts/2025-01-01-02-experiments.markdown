---
layout: post
title: "Experiments"
category: ""
---

# Experiments

- はじめに、プログラムを作ってもらい、実行エラーメッセージと改善/リファクタリング/機能追加要望をプロンプトに入れて、食わせるのをループさせれば、永久に改善していくのでは？
  - ファイルの中身が一意に収束する、同じ回答になり、同じプロンプトで、同じ回答ということが繰り返されるだけとなった。
  - 使用したモデルが小さく、大きさに依存しているかもしれない。つまり、モデルを大きくすれば改善するかもしれない。

- 実行時間について、大きいモデルを使用すると完全回答までの時間が変わる。
  - 環境
    - CPU: Intel(R) Core(TM) i5-8250U CPU @ 1.60GHz (CPU ONLY mode)
    - Memory: 7758900 kB (Swap: 40GB)
  - prompt
    - `hello`
  - model
    - `gemma3:1b` => `0:03.31elapsed (just 3 seconds)`
    - `gemma3:4b` => `0:14.02elapsed (around 14 seconds)`
    - `gemma3:12b` => `5:37.88elapsed (about 5 and a half minutes)`
    - `gemma3:27b` => `1:33:13elapsed (about 1 and a half hours)`
- Ragを使ってどこまでできるか。
  - Rubyのドキュメントを食わせて、スクリプトを説明させてみる。
  - 結果：[RubyDemoDoc](https://yumayx.github.io/rubydemodoc/)
  - 流石に間違えを含む。
