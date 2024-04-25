---
title: 模組化必經之路Composite Builds
banner_img: >-
  https://res.cloudinary.com/deu7aohfe/image/upload/v1714005461/202404243075098463/ywcawp8irobv8dhhlhlx.webp
index_img: >-
  https://res.cloudinary.com/deu7aohfe/image/upload/v1714005461/202404243075098463/ywcawp8irobv8dhhlhlx.webp
categories:
  - Android
tags:
  - Composite Builds
  - BuildSrc
  - 模組化
  - Modularization
  - Plugin
  - Kotlin DSL
  - Optimize build time
  - 優化建構時間
abbrlink: 3075098463
date: 2024-04-24 22:30:14
---

在專案規模日漸增長的情況下，dependencies的維護管理會隨著專案的複雜性與模組化使得管理越來越艱鉅，因此為解決這個問題Gradle 7.0推出了新的 "Catalog" 協助開發者來以更好的方式去維護管理dependencies的版本，且在新版的IDE也新增預設 catalog 這個選項了。

本篇會以兩大主題為主軸 `Catalog & Composite Builds`，可以搭配官方文件一同食用此外也一並附上 Sample Code，未來有機會也可以搭配Plugin整合CI/CD自動化更新Dependencies版本，如果是營運的專案推薦還是手動管理為優穩定性與風險較為可控。

為了更進一步優化專案，也示範了複合式建構(Composite Builds)的方式來建置專案，幫助了解其中的差異及提升Build time的關鍵，讓你在模組化的康莊大道上走得更為愜意。

當然模組化不是一個必要項目是個選項，但會走到需要優化build time的狀況專案一定也具備一定的規模了，如果是先天不良後天又失調的專案如何在有限資源改善目前狀況變成為非常重要的課題，不然醫美近年也不會這麼夯了。

## Gradle Scan
這邊附上 Demo 的 gradle scan，如果要優化總是需要一份報告書作為佐證可以使用下方的command來產出這份報告，從報告的time line也可以讓人更瞭解初始化的差別。

```properties
./gradlew --scan
```

- [純Kotlin DSL](https://scans.gradle.com/s/p2oj5jhjcimmk/timeline)
- [Composing build](https://scans.gradle.com/s/ek6hefozbzuoe/timeline)

Demo因為規模很小，如果換成大專案省下的時間會非常的可觀，這邊就附上目前專案上模組化後的report time line體感上就會差很多了。

![Project report time line](https://res.cloudinary.com/deu7aohfe/image/upload/v1714012005/202404243075098463/uoctockxctsiwyblvla4.webp)

## 参考
- Banner Photo by <a href="https://unsplash.com/@ilumire?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash">Jelleke Vanooteghem</a> on <a href="https://unsplash.com/photos/toddler-playing-with-two-wooden-blocks-Aqd30KmCc3g?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash">Unsplash</a>
- 
