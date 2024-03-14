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

但選擇使用Hexo一定是希望可以降低維護成本所以本篇會以套用主題的方式來改善UX，好的主題通常都會留有相對應的空間可進行調整，選搭喜歡的設定就有一定程度的客製化，如果想更進一步就需要參考官方文件像是Next是在source > _data > styles.styl內寫CSS即可。

本篇會以Fluid作為主題可透過本文了解到選用Fluid的原因、Fluid的設定、Plugin及圖床選用等。

## 如何挑選主題？
截至目前為止官方主題共有404個有興趣可以至[官網](https://hexo.io/themes/index.html)選擇，個人覺得只要符合以下要點即可。

- Theme的Repository有持續維護，可以觀察Commit的時間
- 有LiveDemo且使用上正常（如果連LiveDemo或是Readme上的圖片都壞掉了，高機率已經沒人維護了）
- 首頁與內頁等具有一制性且UI簡潔，這部分可以參考[Material Design](https://m3.material.io/)
- 各個plugin的支援度，以Next做舉例就可以看到[Readme](https://github.com/next-theme/awesome-next#live-preview)內有許多推薦的Plugins

綜上述要點原先的首選是[Next](https://github.com/next-theme/hexo-theme-next?tab=readme-ov-file)這個主題，試用一陣子後感覺首頁設計比較適用於全文字的設計如果坎入圖片的話會讓整體的排版有說不上來的不協調感，想要修正就需要更改主題的程式碼，後續只要主題更新就需針對新版本再做一次修正大大增加了維護成本。

後續尋找的過程中無意間看到[Fluid](https://github.com/fluid-dev/hexo-theme-fluid)符合我想要的Material Design且首頁文章列表加入圖片也不會顯得非常突兀整體上非常合適便選擇了它。

## Fluid設定
1. 最簡單的方式，現在安裝的Hexo版本都是5.0.0以上了，所以可以透過npm直接安裝後續有更新theme也會方便許多。
  ```properties
    npm install --save hexo-theme-fluid
  ```
  如果有高度客製化需求的話建議把theme clone到本地的repository。如果熟悉git可以把主題獨立一個repository使用submodules的方式掛接回原專案主要有兩個原因，可以確保撰寫文章的repository的獨立性，theme有更新時pull submodule就會更新reset也方便，另一個是抽換theme也可以做到無痛抽換只需更換submodules的連結與sha即可maintain上相對單純。

2. 在根目錄下創建`_config.fluid.yml`的檔案，接著去copy & paste官網中的設定檔theme主要會以這個[yml檔](https://github.com/fluid-dev/hexo-theme-fluid/blob/master/_config.yml)為主，可以依據個人喜好搭配[官方配置指南](https://fluid-dev.github.io/hexo-fluid-docs/guide/#%E5%85%B3%E4%BA%8E%E6%8C%87%E5%8D%97)來去做調整。

![_config.fluid.yml](https://res.cloudinary.com/deu7aohfe/image/upload/v1710251247/202402233632500070/sx5gkovjjinpfb9hwvsn.webp)

## Plugin推薦
這裡必裝2個Plugin如下列。
- [hexo-pangu](https://github.com/vinta/pangu.js)：
  主要是在中、英文、數字之間補上空白如果是純中、英文需求可以省略，但工程師為了準確敘述這個概念時常中英夾雜，這個Plugin可以提高文章的閱讀體驗。
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
  branch: main
  ```
  最後照先前的順序再多加個deploy即可clean > generate > deploy。
  ```properties
    hexo d
  ```

## 網站圖示設定
使用Browser時一定會發現分頁上有一個小icon，如果沒有的話可能會是出現地球的default icon。在這邊推薦知名的Favicon產生器[Favicon.io](https://favicon.io/)，支援`text > ico`, `png > ico`, `emoji > ico`使用上已經符合大多數情境且是免費的，這邊以text > ico來做舉例。
1. 選擇轉換的類型，這邊以`text > ico`做舉例。

![Favicon Generate](https://res.cloudinary.com/deu7aohfe/image/upload/v1710302230/202402233632500070/jszjbix5m9jtzbx0xlaz.webp)

2. Preview可以預覽icon的畫面，下面可以依照自身喜好去更改背景顏色、自行、字體、字體大小、背景圖案等，調整完畢後可以點選右側的`Download`。

![Setup Favicon](https://res.cloudinary.com/deu7aohfe/image/upload/v1710313413/202402233632500070/nbr3hcoymy7h2dldtxrw.webp)

3. 下載後會有一個zip檔，解壓縮後會得到下列圖片可以選擇所需放入theme取用的資料夾內並在theme的yml檔裡指定路徑，以Fluid舉例就是在`_config.fluid.yml`內指定Favicon路徑。
  - android-chrome-192x192.png
  - android-chrome-512x512.png
  - apple-touch-icon.png
  - favicon-16x16.png
  - favicon-32x32.png
  - favicon.ico
  - site.webmanifest

## 圖床選擇
技術部落格一定會有圖片搭配解說，怎麼處理圖片存取便是很重要的課題，直覺上一定是放在Repository裡即可，這麼做會產生兩個問題。

1. 網頁顯示上的效能問題：
    如果檔案格式沒有調整好的話頁面的Loading也會變得緩慢，因為網頁的檔案變得非常肥大，除非針對圖片去預載。
2. Github Repository存放大小限制：
   在靜態網頁上如果是放在Repository內又沒針對檔案格式去做調整的話，大概沒多久就會達到官方建議的上限5 GB。

要解決這個問題就需要圖床，存放圖片的地方有些人會上傳到`imgur`可以拿到Url但這有個問題，官方明訂禁止把imgur當作圖床因此圖片在這個平台可以存活的時間不可控，圖片解析度也會大大打折。

這裡推薦另一個方案，可以使用[Cloudinary](https://cloudinary.com/)免費就有25GB的空間，如果圖片有轉檔處理過之後大小一定會比起PNG, JPG小上許多，可以將圖片轉成Google新出的圖片格式webp大部分的瀏覽器都支援。Cloudinary除了圖片以外，也支援影片這算是圖床上比較少見的，因為影片佔用的空間相對較大處理上也比較麻煩。

使用上也非常簡單註冊好後點選`Media Explorer`內建就有圖庫了，想要上傳自己的圖片也可以點選右上角`upload`，可以點圖片copy左下角的`Original URL`並依照Markdown的語法貼上連結即可。

![Upload](https://res.cloudinary.com/deu7aohfe/image/upload/v1710342574/202402233632500070/zsqdqmaemczyf1vmfrdf.webp)

![Original URL](https://res.cloudinary.com/deu7aohfe/image/upload/v1710342565/202402233632500070/uyfoxea0wacrz9pjxeib.webp)

另一個重點是免費版就可以知道各圖片的使用報告，對於改善寫作選圖等也是非常有幫助的。

![Cloudinary report](https://res.cloudinary.com/deu7aohfe/image/upload/v1710379976/202402233632500070/mpetnff2f2dc7m0ulc0m.png)

## 總結
到目前為止已經建立好Blog的環境，套用好賞心悅目的主題以及設計了Blog的icon，從前幾篇至目前為止都還沒進入Domain就可以知道前期的準備蠻久的方方面面都得自己處理。相對過程中一定也會遇到問題，但這投資是划算的可以自己優化掌控方向也是一大樂趣。希望大家至目前為止開發順利閱讀完也有些收穫，如果有任何問題也歡迎留言，下篇文章見拉！
## 参考
- Banner Photo by <a href="https://unsplash.com/@8moments?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash">Simon Berger</a> on <a href="https://unsplash.com/photos/landscape-photography-of-mountains-twukN12EN7c?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash">Unsplash</a>
- [如何使用 Hexo + GitHub Pages 架設個人網誌](https://hackmd.io/@Heidi-Liu/note-hexo-github)
- [Favicon.io 最強大的網站圖示產生器，可線上文字製作或以圖片轉換](https://free.com.tw/favicon-io/)
