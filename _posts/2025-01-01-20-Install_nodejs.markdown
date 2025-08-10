---
layout: post
title: "Install nodejs"
category: ""
---

## Check and Clean Env.

```sh
dnf list nodejs npm

dnf remove -y nodejs npm
```

## Check Modules

```sh
dnf module list nodejs
```

## Install Node.js

```sh
dnf module -y enable nodejs:22

dnf install -y nodejs npm
```

- <https://qiita.com/daichi_pd/items/2c83423cb387dc87df36>

