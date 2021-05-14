---
layout: 配置Hexo在github page，使用github action自动构建
title: hexo github
date: 2021-05-14 16:52:52
tags: hexo github action CI
---

### 准备

你得先配置好hexo，这些内容我懒得打字，自行百度，要达到的效果是，输入 hexo d的时候，可以构建项目到github page。

### 配置deploy 密钥

输入

```
ssh-keygen -f github-deploy-key
```

来生成密钥。
公钥复制到仓库的Deploy key，私钥复制到Secret，要记住这里设置的名字，比如`HEXO_DEPLOY_KEY `

### 配置Action

我的法子，是一个仓库里，两个分支，master放deploy后的结果，hexo里放源代码。
在源代码的.github/workflows/里新建一个 随便名字.yml
里面写上

```yml
name: hexo auto CI
on:
  push:
    branches:
      - hexo
jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout source
        uses: actions/checkout@v1
        with:
          ref: hexo
      - name: Use Node.js ${{ matrix.node_version }}
        uses: actions/setup-node@v1
        with:
          node_version: '12'
      - name: Setup hexo
        env:
          ACTION_DEPLOY_KEY: ${{ secrets.HEXO_DEPLOY_KEY }}
        run: |
          mkdir -p ~/.ssh/
          echo "$ACTION_DEPLOY_KEY" > ~/.ssh/id_rsa
		  chmod 700 ~/.ssh
          chmod 600 ~/.ssh/id_rsa
          ssh-keyscan github.com >> ~/.ssh/known_hosts
          git config --global user.email "heyaodada@qq.com"
          git config --global user.name "Wilson"
          npm install hexo-cli -g
          npm install
      - name: Hexo deploy
        run: |
          hexo clean
          hexo deploy
```

user.name和email改成你自己的。还有secrets的那个也改成你当时设置的名字，改好后，每次有push到hexo分钟，github action就会自动构建