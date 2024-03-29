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

說了這麼多什麼是Continuous Deployment? [IBM](https://www.ibm.com/topics/continuous-deployment)是這麼解釋的。

> Continuous deployment is a strategy in software development where code changes to an application are released automatically into the production environment.

大意就是在軟體開發中自動將更動的程式Build Release並提交至Production環境中，以本篇的例子就是Main branch只要有新commit GitHub Action就會自動Build一個靜態網頁並推至gh-pages這個Branch。

## GitHub Actions
在[GitHub Universe 2018](https://githubuniverse.com/)，GitHub發佈了GitHub Actions可以透過它實作CI/CD、分支管理、issue分類等。

關於計費[官網](https://docs.github.com/en/billing/managing-billing-for-github-actions/about-billing-for-github-actions)寫每個月有2000分鐘/500MB的使用上限，使用分鐘數每個月會重置但是500MB的儲存空間是不會重置的。機器分鐘的計算方式如下方表格，每個系統分鐘數是不一樣的如果有必要的話需要對專案的建置時間來去做優化。

|   系統    | 分鐘 |
|:-------:|:--:|
|  Linux  | 1  |
| Windows | 2  |
|  macOS  | 10 |

本篇是用ubuntu的OS就是Linux，所以分鐘數不用倍數計算。

## GitHub deploy key
就如同使用Git GUI推本地端異動到Repository一樣，只是這個情境換到GitHub的Server而已Repository要讀寫第一關一定是認證機制也一樣會需要公私鑰來去認證。

### Generator Key
1. 打開Terminal輸入下方的command，`e-mail`的部分可以替換成自己的mail，接著就一路Enter下去。
```properties
ssh-keygen -t ed25519 -C "e-mail" -f deploy
```
2. 一般來說gen出來的key會在`.ssh`的資料夾內，如果沒有特別設定路徑會在`User/.ssh`，這個資料夾預設是隱藏的可以壓`command + shift + .`就可以看到隱藏的資料夾了，內部有兩個檔案`XXX.pub` & `XXX`，沒有更改檔名預設會是deploy。
   {% note info %}
   - XXX.pub：公鑰
   - XXX：私鑰
   {% endnote %}

### 設定專案公私鑰
#### 公鑰
{% note info %}
Setting > Deploy keys > Add deploy key
{% endnote %}

Title可以隨個人喜好設定，沒有像branch有特別命名方式；Key就copy XXX沒有副檔名的檔案內容。
![Add deploy key](https://res.cloudinary.com/deu7aohfe/image/upload/v1711506022/20240320564916095/dbsuhsecjxothzsyce8u.webp)

#### 私鑰
{% note info %}
Setting > Secrets and variables > Actions > New repository secret
{% endnote %}

Title 可以隨個人喜好設定只要Yaml內容有寫對的話機器就不會找不到Key了。

![Add repository secret](https://res.cloudinary.com/deu7aohfe/image/upload/v1711506159/20240320564916095/v3nz73xmz4l0phqehvum.webp)

## Yaml設定
新增GitHub Action執行所需的Yaml方式有兩種：
1. 在`.github`目錄下方新增`workflows`資料夾，並在裡面加入`deploy.yml`的檔案可以針對所需開發自己需要的job。
2. 透過Web設定，路徑如 `Actions > New workflow > Simple workflow`就會創建一個yml並放在相對應路徑中方便許多，可以參考下方圖片步驟。

![Step 1. New workflow](https://res.cloudinary.com/deu7aohfe/image/upload/v1711508264/20240320564916095/eamt0czo431wsga49vdk.webp)

![Step 2. Create simple workflow](https://res.cloudinary.com/deu7aohfe/image/upload/v1711508499/20240320564916095/dkd879bfpgn1t0ettfb0.webp)

![Step 3. Add Jobs](https://res.cloudinary.com/deu7aohfe/image/upload/v1711508649/20240320564916095/gomia0xnwabytkegyuhx.webp)

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
          HEXO_DEPLOY_PRI: ${{secrets.HEXO_DEPLOY_PRI}} # 須與GitHub上Repository secrets內的secret title一樣
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
以前撰寫文章總是要經歷四步驟clean、generate、server沒問題後還要再deploy非常繁瑣，工程師都知道`科技始終來自於惰性`要讓過程簡化的話一定需要自動化，也讓大家撰寫部落格上更加的輕鬆，CI/CD也是求職上加分的項目所以學會寫yaml也蠻重要的。

希望本篇文章對你有所幫助，如果有問題或需更正歡迎留言！

## 参考
- Banner Photo by <a href="https://unsplash.com/@phillipglickman?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash">Phillip Glickman</a> on <a href="https://unsplash.com/photos/green-and-multicolored-robot-figurine-2umO15jsZKM?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash">Unsplash</a>
- [About billing for GitHub Actions](https://docs.github.com/en/billing/managing-billing-for-github-actions/about-billing-for-github-actions)
- [What is continuous deployment?](https://www.ibm.com/topics/continuous-deployment)
- [Hexo + GitHub Actions 部屬網站遷移全紀錄](https://blog.yangjerry.tw/2022/04/19/hexo-github-actions-deploy/)
