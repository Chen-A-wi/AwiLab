---
title: GitHub Actions 優雅部署 Hexo
banner_img: https://res.cloudinary.com/deu7aohfe/image/upload/v1710925423/20240320564916095/cfdjsyquwggf2czxtxoz.webp
index_img: https://res.cloudinary.com/deu7aohfe/image/upload/v1710925423/20240320564916095/cfdjsyquwggf2czxtxoz.webp
categories:
  - Blog
tags:
  - Blog
  - GitHub Actions
  - CI/CD
  - Deploy
  - Continuous Deployment
  - Hexo
abbrlink: 564916095
date: 2024-03-20 11:39:45
---
各位在上一篇學廢了怎麼套用Hexo的theme及一些工具幫助撰寫Blog，文章中也有提到怎麼安裝deploy的plugin，想了解可以點[為Blog添點顏色](https://awilab.com/202402233632500070/)。

我心目中理想的方案是使用Obsidian來整合整個文章的發佈，達到將整理好的筆記推至Blog中且在本地端瀏覽起來也方便，但是深入研究之後Obsidian要Push需要訂閱如果想sync的話也需要訂閱加總起來的話價格也非常的可觀，對一個北漂求生的工程屍來說也是一筆負擔，後續會嘗試使用Notion應該可以達成理想中的撰寫方式，成功的話再整理成文章記錄下來當然如果有更好的方式也歡迎留言提供。

本篇會多著墨於自動化部屬可以簡化整個寫作發文的流程也有助於CI/CD的學習，希望本章有幫上一點忙。

## Continuous Deployment
Continuous Deployment縮寫為CD中文可以翻作續部署，依照需求可以與持續整合（Continuous integration, CI）配合使用，就是大家口中所述的CI/CD。本篇實作並沒有整合自動化單元測試，因只有一人撰寫也未走完整的git flow在我認知中稱不上完整的CI，所以會多著墨於CD的部分。

說了這麼多什麼是Continuous Deployment? IBM是這麼解釋的。

> Continuous deployment is a strategy in software development where code changes to an application are released automatically into the production environment.

大意就是在軟體開發中自動將更動的程式Build Release並提交至Production環境中，以本篇的例子就是Main branch只要有新commit GitHub Action就會自動Build一個靜態網頁並推至gh-pages這個Branch。

## GitHub Actions
在[GitHub Universe 2018](https://githubuniverse.com/)，GitHub發佈了GitHub Actions可以透過它實作CI/CD、分支管理、issue分類等。

關於計費[官網](https://docs.github.com/en/billing/managing-billing-for-github-actions/about-billing-for-github-actions)寫每個月有2000分鐘/500MB的使用上限，使用分鐘數每個月會重置但是500MB的儲存空間是不會重置的。在機器上的

<style>
.center 
{
  width: auto;
  display: table;
  margin-left: auto;
  margin-right: auto;
}
</style>

<p align="center"><font face="黑体" size=2.>表1 示例表格</font></p>

<div class="center">

|   序号   |   内容  |        描述         |
|  :---:  |  :---:  |  :---------------:  |
|    1    |    l    |  大写字母L的小写字母l |
|    2    |    I    |      大写字母I       |
|    3    |    1    |        数字1       |

</div>

## GitHub deploy key

## Yaml設定

```yaml
# Github action name
name: Deploy

# 觸發條件
on:
  push:
    branches:
      - main # 監聽的Branch，只要有commit就會觸發

# 變數宣告
env:
  GIT_USER: CHIAN-WEI, CHEN
  GIT_EMAIL: google@gmail.com
  DEPLOY_REPO: CHIAN-WEI, CHEN/Repository Name
  DEPLOY_BRANCH: gh-pages

jobs:
  build:
    name: Build on node ${{ matrix.node_version }} & ${{ matrix.os }}
    runs-on: ubuntu-latest # 建置任務的容器，有下列選擇：ubuntu-latest, windows-latest, macos-latest
    strategy:
      matrix:
        os: [ ubuntu-latest ]
        node_version: ['20.x']

    steps:
      - name: Checkout source
        uses: actions/checkout@v4

      # 設定Deploy branch
      - name: Checkout deploy repo
        uses: actions/checkout@v4
        with:
          repository: ${{ env.DEPLOY_REPO }}
          ref: ${{ env.DEPLOY_BRANCH }}
          path: .deploy_git

      - name: Setup Node.js ${{ matrix.node_version }}
        uses: actions/setup-node@v3
        with:
          node-version: ${{ matrix.node_version }}

      - name: Configuration environment
        env:
          HEXO_DEPLOY_PRI: ${{secrets.HEXO_DEPLOY_PRI}}
        run: |
          mkdir -p ~/.ssh/
          echo "$HEXO_DEPLOY_PRI" > ~/.ssh/id_rsa
          chmod 600 ~/.ssh/id_rsa
          ssh-keyscan github.com >> ~/.ssh/known_hosts
          git config --global user.name ${{ env.GIT_USER }}
          git config --global user.email ${{ env.GIT_EMAIL }}

      # Hexo環境建置
      - name: Install dependencies
        run: npm ci

      - name: Setup Hexo environment
        run: |
          npm install -g hexo-cli

      - name: Hexo clean
        run: |
          hexo clean

      - name: Hexo generate
        run: |
          hexo generate

      - name: Hexo deploy
        run: |
          hexo deploy
```

## 總結

## 参考
- Banner Photo by <a href="https://unsplash.com/@phillipglickman?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash">Phillip Glickman</a> on <a href="https://unsplash.com/photos/green-and-multicolored-robot-figurine-2umO15jsZKM?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash">Unsplash</a>
  
  
