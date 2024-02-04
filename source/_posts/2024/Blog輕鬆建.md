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

## 總結
Blog建置好之後會慢慢補上建置過程，從平台選定、利用Github Action來自動化deploy、Domain設定、SEO等，希望是採小章節短文章的形式避免一次太快跟不上，如果有興趣的朋友可以跟著建置一個屬於自己的Blog，如果有任何問題也歡迎隨時提問。

## 参考
- Banner Photo by <a href="https://unsplash.com/@saj_shafique?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash">Saj Shafique</a> on <a href="https://unsplash.com/photos/silhouette-of-crane-during-sunset-jCJpn7zlyCo?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash">Unsplash</a>
- [靜態網站產生器大比拚](https://raychiutw.github.io/2019/Static-Site-Generator-Comparison/)
[^1]: [Jekyll安裝](https://www.jekyll.com.cn/docs/installation/#requirements)
  
