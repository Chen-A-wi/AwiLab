---
title: Blog輕鬆建
banner_img: >-
  https://res.cloudinary.com/deu7aohfe/image/upload/v1706794394/202402012108647692/uqseh2404hwm0neznqto.webp
index_img: >-
  https://res.cloudinary.com/deu7aohfe/image/upload/v1706794394/202402012108647692/uqseh2404hwm0neznqto.webp
categories:
  - Hexo
tags:
  - 部落格建置
  - Blog
  - Github Page
  - Hexo
abbrlink: 2108647692
date: 2024-02-01 21:14:00
---
單純寫文章記錄的話選擇非常的多，不管是HackMD、Medium都是非常不錯的選擇，像HackMD可以用Markdown來編寫對於習慣Markdown的我來說可以無痛轉移。

Medium則是在我自架Blog前就使用了，主打以文章作為社群媒介我滿喜歡這個idea，前期有非常強大的SEO可以大大提高文章的能見度，可惜後續調整了SEO整個改爛了，也能理解為了營利推廣訂閱會員方案，但目前的推銷方式已經多到讓人厭惡的地步了，也時常因沒有訂閱會員就無法閱讀熱門文章。

思考後像是Medium、HackMD那些都依賴於第三方平台如果哪天像是無名小站一樣漸漸式微的話，當營運方維護費用已經高於盈利絕對會選擇關閉，便得出自架是唯一選擇後剩下是怎麼達成這個目的而已於是踏上這條不歸路。

## Github Page選擇？
建置Github Page選擇百百種，可以藉由下方的條列選項來評估想使用哪種靜態頁面來做開發，如果以原生來說的話Github本身是支援Jekyll的，簡單方便的話可以直接套用Jekyll的主題即可。以上這些工具全都是開源工具，目的只是為了快速產出靜態網站且使維護上更為容易，可以依據需求選擇最適合你的方式。

### Jekyll
Jekyll是由Ruby Gem所組成的[^1]，RubyGems是Ruby的Plugin管理器由此可知基底是Ruby。

#### 優點
- 從Medium移植過來很方便，力推 ZhgChgLi 所製作的 Plugin
- Github爸爸原生支援
- 因為存在非常久了，所以教學文件及資源非常豐富

#### 缺點
- 隨著時間的推移內容較多時，編譯的過時會變得非常慢，這是Ruby先天語言的特性
- Jekyll有段時間沒有更新了

### Hexo
Hexo是基於Node.js所開發的，編譯速度也是三者中居於中位原開發者實測54篇文章大概可以在2秒內跑完，有興趣的朋友可以去朝聖開發者的文章[Hexo 颯爽登場！](https://zespia.me/blog/2012/10/11/hexo-debut/)。

#### 優點
- 主題非常的多，截至目前為止有404個[主題](https://hexo.io/themes/)，相較於Jekyll或是Hugo多非常的多
- 套件支援也非常豐富多元
- Hexo開發討論區大多來自中國，中文化程度與資源相較於Jekyll與Hugo更為多元
- 自帶主題，架設好馬上就可以使用了

#### 缺點
- 對非中文語系的人來說會討論區較不直覺一些，但這些翻譯都能解決

### Hugo
Hugo是基於Go語言所開發的，編譯速度是三者之最官網也是直接宣傳[The world’s fastest framework for building websites](https://gohugo.io/)，對於熟悉Go的朋友這個是你的最佳選擇。

#### 優點
- 三者中最快的，每頁的建置速度不超過1ms，建置一個Blog不會超過1秒鐘
- 官網也宣稱轉出來後的頁面是純靜態頁面

#### 缺點
- 主題的選擇相對應Hexo較少，但個人認為較Jekyll新潮一些

## 如何使用Hexo產出靜態頁？
在開始之前要先準備好前置準備，就是Node.js與Git Node.js是Hexo產出靜態網站所需的環境，Git則是推上Github所需要的。歐！還有一個Github帳戶。

### 前置準備
1. 如果是Mac用戶且裝好了Homebrew，那直接輸入下方指令👇。沒有的話可以[點我](https://nodejs.org/en/download/current)去下載安裝Node.js，完成後可以輸入``node -v``確認下版號，如果有出來的話代表安裝成功。

```properties
brew install node
```

{% note info %}
這裡需要注意[Hexo與node相對應的版本](https://hexo.io/zh-cn/docs/index.html#Node-js-%E7%89%88%E6%9C%AC%E9%99%90%E5%88%B6)，別裝錯了。
{% endnote %}

2. 接著再安裝Git，輸入下方指令👇。沒有的話可以參考[官網](https://git-scm.com/book/zh-tw/v2/%E9%96%8B%E5%A7%8B-Git-%E5%AE%89%E8%A3%9D%E6%95%99%E5%AD%B8)來安裝，這裡就不多做贅述了，完成後可以輸入``git -v``確認下版號，如果有出來的話代表安裝成功。

```properties
brew install git
```

### 安裝Hexo
npm安裝完後一樣可以確認一下版本順便確認是否安裝成功。

```properties
npm install hexo-cli -g
```

![Hexo版本](https://res.cloudinary.com/deu7aohfe/image/upload/v1707095598/202402012108647692/yh2yscv9n5hykqw9rzxi.webp)

### 初始化Hexo

先移動到想放Blog的資料夾根目錄中，像我想放在SideProject裡就先移動至這層資料夾內。

![CD至根目錄](https://res.cloudinary.com/deu7aohfe/image/upload/v1707044173/202402012108647692/mhufwy5bw8gfzsolgl5n.webp)

接著初始化，Hexo本身會建立好資料夾並安裝在裡面，{% label primary @這邊需要注意的是安裝Hexo的資料夾必須是空的，所以建議可以不用先創好直接使用command創立 %}。

```properties
hexo init '資料夾名稱'
```

![初始化Hexo](https://res.cloudinary.com/deu7aohfe/image/upload/v1707044288/202402012108647692/atinwqrgpahnbdtatnhw.webp)

### 安裝Hexo所需Plugin
先``cd``進我們創建的資料夾內容中，可以使用tree這個套件去看資料夾內部的結構，沒有的話可以``brew install tree``，我們只是簡單確認所以只進到第一層級應該會如下圖所顯示。

![Blog資料夾樹狀圖](https://res.cloudinary.com/deu7aohfe/image/upload/v1707095215/202402012108647692/a5e0a8tzfgvyduihkeyl.webp)

如果要全顯示的話可以輸入下方的command，或是[參考這篇文章](https://blog.csdn.net/zhuyunier/article/details/119837816)選擇所需的command。
```properties
tree -N
```

確認好資料一樣後可以安裝所需的plugin了，Hexo會依據[package-lock.json](https://yenkos.github.io/2021/04/02/%E5%B7%A5%E7%A8%8B%E5%8C%96/%E4%BB%80%E4%B9%88package-lock.json%20_/)來去做plugin的版本管理也是依據它來安裝。

```properties
npm install
```
![npm install](https://res.cloudinary.com/deu7aohfe/image/upload/v1707096604/202402012108647692/blj0czqvxjhvmfkmfi80.webp)

{% note info %}
注意如果有兩部以上的電腦想要同步plugin的話，可以直接刪掉`node_modules`這個資料夾，再`npm install`就會自動依照json檔來安裝其他電腦裡的plugin了，前提是有push json檔案至Git中。
{% endnote %}

### Build Hexo in local
完成上述步驟之後差幾步就可以在本地端看到靜態頁面了，加油！

先清除Hexo的暫存檔，這邊記得之後重新產出靜態頁面時要先退出local server的狀態，看過有網友直接clean結果把本地端public的資料夾清掉了。

```properties
hexo cl
```
![Hexo clean](https://res.cloudinary.com/deu7aohfe/image/upload/v1707097060/202402012108647692/a0najy5oq127qknfpxhq.webp)

```properties
hexo g
```
![Hexo generate](https://res.cloudinary.com/deu7aohfe/image/upload/v1707097176/202402012108647692/tg3injdv4bypx7ric5vc.webp)

```properties
hexo s
```
![Hexo server](https://res.cloudinary.com/deu7aohfe/image/upload/v1707097266/202402012108647692/w0qgymnla11ivknbuoy0.webp)

最後可以copy localhost或是command + 滑鼠左鍵即可跳轉到本地端的網頁了，初始畫面應該會像圖中所示，到這邊恭喜成功嘍！！🎉

![](https://res.cloudinary.com/deu7aohfe/image/upload/v1707097457/202402012108647692/b4gq89xg3ypulgh9kvff.webp)

## 總結
Blog的前期準備其實蠻花時間的，各個方面全都要自己來如果需要大量客製化的話，可能就需要把主題拉下來接著去修改裡面的css內容等，就會需要懂程式語言畢竟需要自行維護可客製化的內容。

我也會記錄下我做了哪些更動或是安裝了哪些Plugin從利用Github Action來自動化deploy、Domain設定、SEO等，希望是採小章節短文章的形式避免一次太快跟不上，如果有興趣的朋友可以跟著建置一個屬於自己的Blog，如果有任何問題也歡迎隨時提問。

## 参考
- Banner Photo by <a href="https://unsplash.com/@saj_shafique?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash">Saj Shafique</a> on <a href="https://unsplash.com/photos/silhouette-of-crane-during-sunset-jCJpn7zlyCo?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash">Unsplash</a>
- [靜態網站產生器大比拚](https://raychiutw.github.io/2019/Static-Site-Generator-Comparison/)
[^1]: [Jekyll安裝](https://www.jekyll.com.cn/docs/installation/#requirements)
  
