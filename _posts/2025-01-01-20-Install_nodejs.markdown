---
layout: post
title: "Install nodejs"
category: ""
---

## Check and Clean Env.

現在の`nodejs`,`npm`のバージョンを調べる。今回は削除も行う。

```sh
dnf list nodejs npm

dnf remove -y nodejs npm
```

## Check Modules

dnfのモジュールで、利用可能なバージョンをリストする。

```sh
dnf module list nodejs
```

## Install Node.js

最新のバージョンをインストールする。

```sh
dnf module -y enable nodejs:22

dnf install -y nodejs npm
```

- <https://qiita.com/daichi_pd/items/2c83423cb387dc87df36>

