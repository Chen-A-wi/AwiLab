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

但選擇使用Hexo的一定是希望可以降低維護成本的，所以會以套用主題的方式來改善UX好的主題通常都會留有相對應的空間可進行調整，選擇搭配自己喜歡的設定即可如果想更進一步客製化，就需要參考官方文件像是Next是在source > _data > styles.styl內寫CSS即可。

本篇會以Fluid作為主題可透過本文了解到選用Fluid的原因、Fluid的設定、Plugin及圖床選用等。

## 如何挑選主題？
截至目前為止主題共有404個有興趣可以至[官網](https://hexo.io/themes/index.html)選擇符合自身愛好的主題，個人選擇只要符合以下要點即可。

- Theme的Repository有持續維護，可以觀察Commit的時間。
- 有LiveDemo且使用上正常，如果連LiveDemo或是Readme上的圖片都壞掉了，高機率已經沒人維護了。
- 首頁與內頁等具有一制性且UI簡潔，這部分可以參考[Material Design](https://m3.material.io/)。
- 各個plugin的支援度，以Next做舉例就可以看到[Readme](https://github.com/next-theme/awesome-next#live-preview)內有許多推薦的Plugins。

綜上述要點原先的首選是[Next](https://github.com/next-theme/hexo-theme-next?tab=readme-ov-file)這個主題，試用一陣子後感覺首頁設計比較適用於全文字的設計與排版如果坎入圖片的話會讓整體的排版有說不上來的協調，如果想要修正就需要更改主題的程式碼後續只要主題更新就須針對新版本在做一次修正大大增加了維護成本。

後續在尋找的過程中無意間看到[Fluid](https://github.com/fluid-dev/hexo-theme-fluid)符合我想要的Material Design且首頁文章列表加入圖片也不會顯得非常突兀便選擇了它。

## 總結

## 参考
- Banner Photo by <a href="https://unsplash.com/@8moments?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash">Simon Berger</a> on <a href="https://unsplash.com/photos/landscape-photography-of-mountains-twukN12EN7c?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash">Unsplash</a>

[^1]: aaaa
