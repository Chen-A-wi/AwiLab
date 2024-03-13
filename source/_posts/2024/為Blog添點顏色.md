---
title: 為Blog添點顏色
banner_img: >-
              https://res.cloudinary.com/deu7aohfe/image/upload/v1708673349/202402233632500070/jsqhpjfpf8yzq9ovelre.webp
index_img: >-
              https://res.cloudinary.com/deu7aohfe/image/upload/v1708673349/202402233632500070/jsqhpjfpf8yzq9ovelre.webp
categories:
  - Hexo
tags:
  - 部落格建置
  - 部落格主題
  - Blog
  - Github Page
  - Theme
  - Fluid
  - Hexo
abbrlink: 3632500070
date: 2024-02-23 15:24:32
---
到目前為止完成了Blog本地端建置，整體看起來非常的素面只達到堪用的程度如果想要增進UX(User Experience)的話勢必要做點什麼改變有兩個方案可以選擇，可以自己刻UI(User Interface)或是套用主題。

但選擇使用Hexo的一定是希望可以降低維護成本的所以會以套用主題的方式來改善UX，好的主題通常都會留有相對應的空間可進行調整，選搭喜歡的設定就有一定程度的客製化，如果想更進一步就需要參考官方文件像是Next是在source > _data > styles.styl內寫CSS即可。

本篇會以Fluid作為主題可透過本文了解到選用Fluid的原因、Fluid的設定、Plugin及圖床選用等。

## 如何挑選主題？
截至目前為止官方主題共有404個有興趣可以至[官網](https://hexo.io/themes/index.html)選擇，個人覺得只要符合以下要點即可。

- Theme的Repository有持續維護，可以觀察Commit的時間。
- 有LiveDemo且使用上正常，如果連LiveDemo或是Readme上的圖片都壞掉了，高機率已經沒人維護了。
- 首頁與內頁等具有一制性且UI簡潔，這部分可以參考[Material Design](https://m3.material.io/)。
- 各個plugin的支援度，以Next做舉例就可以看到[Readme](https://github.com/next-theme/awesome-next#live-preview)內有許多推薦的Plugins。

綜上述要點原先的首選是[Next](https://github.com/next-theme/hexo-theme-next?tab=readme-ov-file)這個主題，試用一陣子後感覺首頁設計比較適用於全文字的設計如果坎入圖片的話會讓整體的排版有說不上來的不協調感，如果想要修正就需要更改主題的程式碼後續只要主題更新就須針對新版本再做一次修正大大增加了維護成本。

後續在尋找的過程中無意間看到[Fluid](https://github.com/fluid-dev/hexo-theme-fluid)符合我想要的Material Design且首頁文章列表加入圖片也不會顯得非常突兀整體上非常合適便選擇了它。

## Fluid設定
1. 最簡單的方式，現在安裝的Hexo版本都是5.0.0以上了，所以可以透過npm直接安裝後續有更新theme也會方便許多。

```properties
npm install --save hexo-theme-fluid
```

如果有高度客製化需求的話建議把theme clone到本地的repository，但如果熟悉git可以把主題獨立一個repository使用submodules的方式掛接回原專案主要有兩個原因，因為這樣可以確保撰寫文章的repository的獨立性，theme有更新時pull下submodule就會更新reset也方便，另一個是抽換theme也可以做到無痛抽換只需更換submodules的連結與sha即可maintain上相對單純。

2. 在根目錄下創建`_config.fluid.yml`的檔案，接著去copy & paste官網中的設定檔theme主要會以這個[yml檔](https://github.com/fluid-dev/hexo-theme-fluid/blob/master/_config.yml)為主，可以依據個人喜好搭配[官方配置指南](https://fluid-dev.github.io/hexo-fluid-docs/guide/#%E5%85%B3%E4%BA%8E%E6%8C%87%E5%8D%97)來去做調整。

![_config.fluid.yml](https://res.cloudinary.com/deu7aohfe/image/upload/v1710251247/202402233632500070/sx5gkovjjinpfb9hwvsn.webp)

## Plugin推薦
這裡必裝2個Plugin如下列。
- [hexo-pangu](https://github.com/vinta/pangu.js)：
  主要是在中英文之間補上空白如果是純中文或純英文的可以省略，但工程師為了準確敘述這個概念時常中英夾雜，這個Plugin可以提高文章的閱讀體驗。
    ```properties
    npm install pangu --save
    ```
  使用方式是在`_config.yml`內補上下面的設定。
    ```yaml
    # https://github.com/vinta/pangu.js
    # Insert a space between Chinese character and English character (中英文之間添加空格)
    pangu:
    enable: true
    field: post # site/post
    ```
- [hexo-deployer-git](https://github.com/hexojs/hexo-deployer-git)：
  推送到github repository需要，未來也須使用這個plugin與github action來做CD(Continuous Deployment)。
  ```properties
  npm install hexo-deployer-git --save
  ```
  設定`_config.yml`內Deploy細節。
  - type: Deploy模式
  - repo: Repository連結
  - branch: 想推上去的分支，`如果是使用github action來deploy，branch就需使用gh-pages`[詳情請看我](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site#troubleshooting-publishing-from-a-branch)

  ```yaml
  # Deployment
  ## Docs: https://hexo.io/docs/one-command-deployment
  deploy:
  type: git
  repo: Repository clone web url or ssh
  branch: gh-pages
  ```

## 角標設定
瀏覽網站時
[Favicon](https://favicon.io/favicon-generator/)
## 圖床選擇
[Cloudinary](https://cloudinary.com/)
## 總結

## 参考
- Banner Photo by <a href="https://unsplash.com/@8moments?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash">Simon Berger</a> on <a href="https://unsplash.com/photos/landscape-photography-of-mountains-twukN12EN7c?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash">Unsplash</a>
- [如何使用 Hexo + GitHub Pages 架設個人網誌](https://hackmd.io/@Heidi-Liu/note-hexo-github)
