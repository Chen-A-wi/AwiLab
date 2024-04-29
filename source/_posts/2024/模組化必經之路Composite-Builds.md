---
title: 模組化必經之路Composite Builds
banner_img: >-
  https://res.cloudinary.com/deu7aohfe/image/upload/v1714005461/202404243075098463/ywcawp8irobv8dhhlhlx.webp
index_img: >-
  https://res.cloudinary.com/deu7aohfe/image/upload/v1714005461/202404243075098463/ywcawp8irobv8dhhlhlx.webp
categories:
  - Android
tags:
  - Android
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

在專案規模日漸增長的情況下，dependencies的維護管理會隨著專案的複雜性與模組化使得管理越來越艱鉅，為解決這個問題Gradle 7.0推出了新的 `Catalog` 協助開發者來以更好的方式去維護管理dependencies的版本，且在新版的IDE也新增預設 catalog 這個選項了。

本篇會以兩大主題為主軸 `Catalog & Composite Builds`，可搭配官方文件一同食用此外一並附上 Sample Code，未來有機會可以搭配Plugin整合CI/CD自動化更新Dependencies版本，如果是營運的專案推薦還是手動管理為優，穩定性與風險較為可控。

為了更進一步優化專案，示範了複合式建構(Composite Builds)的方式來建置專案，幫助了解其中的差異及提升Build time的關鍵，讓你在模組化的康莊大道上走得更為愜意。

當然模組化不是一個必要項目是個選項，但會走到需要優化build time這步田地的專案一定也具備相當的規模了，如果是先天不良後天又失調的專案如何在有限資源改善目前狀況變成為非常重要的課題，不然醫美近年也不會這麼夯了。

# Version Catalog
在 BuildSrc 時期我們會寫一個 object 來管理分類 dependencies 版本及類別概念就與現在的 catalog 相似，但少了IDE support很難看到版本更新提示需要開發者自己注意版本的更迭，當dependencies數量一多時會是非常困擾的問題。

Catalog很大程度地解決這個問題，IDE會幫你在download時去check版本，Group相當於我們做的分類再搭配bundles group來做使用也可以達到我們自定義類別或是寫extension來分類的效果，如前言現在IDE已經支援create default了，但如果是現行專案還是需要migrate的來動手實作吧！

## 1. Create catalog file
首先切到 Project 在這邊我們找到 gradle 這個資料夾，並 create 一個 file 檔名就叫 `libs.versions.toml`。

![Create catalog](https://res.cloudinary.com/deu7aohfe/image/upload/v1714095610/202404243075098463/n3o1yyrh5ycfdhpkke4g.webp)

## 2. 在Catalog建立區塊
在[官方文件](https://developer.android.com/build/migrate-to-catalogs#learn-more)上推薦可以先建立`versions`、`libraries`、`plugins`這三個區塊，分別可以管理版號、dependencies、與專案層級的plugins。

``` toml
[versions]

[libraries]

[plugins]
```

如果想要自定義 group name 也可以，官方推薦的命名樣式是小駝峰命名其中有保留一些關鍵字供系統使用，命名上須多加注意這幾個名字`class, extensions, convention`如果使用到會有問題。

Dependencies的命名中間可以使用`- & _ & .`這三個字符去做區別，gradle會自動轉換成`.`所以不影響使用，範例如：compose-bom 👉 compose.bom。命名上需注意避免下列關鍵字`bundles, versions, plugins`，可以參照官方原文👇

{% blockquote Gradle docs https://docs.gradle.org/current/userguide/platforms.html Gradle 8.1.1 %}
Some keywords are reserved, so they cannot be used as an alias. Next words cannot be used as an alias:

extensions
class
convention

Additional to that next words cannot be used as a first subgroup of an alias for dependencies (for bundles, versions and plugins this restriction doesn’t apply):

bundles
versions
plugins

So for example for dependencies an alias versions-dependency is not valid, but versionsDependency or dependency-versions are valid.
{% endblockquote %}

Sample toml file可以如下面撰寫，name style可以與開發團隊成員一同討論應該是保有相對應的自由度。

``` toml
[versions]
coreKtx = "1.10.0"
appcompat = "1.6.1"
composeBom = "2022.10.00"
kotlin = "1.8.20"


[libraries]
androidx-appcompat = { module = "androidx.appcompat:appcompat", version.ref = "appcompat" }
androidx-core-ktx = { module = "androidx.core:core-ktx", version.ref = "coreKtx" }
compose-bom = { group = "androidx.compose", name = "compose-bom", version.ref = "composeBom" }

[bundles]
androidx = ["androidx-core-ktx", "androidx-appcompat"]

[plugins]
jetbrains-kotlin = { id = "org.jetbrains.kotlin.android", version.ref = "kotlin" }
```

## 3. Setting file path
目前IDE支援可以省略這步，但如果用舊一點的版本就需要手動去關聯檔案，所以需要在專案`setting.gradle.kts`中指定相對路徑，補上後sync即可。

```kotlin
dependencyResolutionManagement {
    repositories {
        mavenCentral()
    }
    versionCatalogs {
        create("libs") {
            from(files("../gradle/libs.versions.toml"))
        }
    }
}
```
## How to implementation?
Gradle 可以參考下方範例來做引用，plugin 使用 alias 來做引用，後續如果自定義Composite Builds的Plugin也是在下面這個區塊使用id來進行引用。

Dependencies的部分就依照命名直接引用就好，IDE也會CodeCompletion可以從提示小窗中選擇需要的dependency，使用上非常方便。
```kotlin
plugins {
    alias(libs.jetbrains.kotlin)
}

dependencies {
    implementation(libs.bundles.androidx)
    implementation(platform(libs.jetbrains.kotlin.bom))
}
```

# Composite Builds
Composite Builds看中文有人把它翻作複合式建構，可以把專案想像成一間集團旗下投資了運動、運輸、餐飲等事業，這些事業群撐起了這整個集團的運作，但是今天單獨營運餐飲事業也是可以營運的，每個事業都有一定程度的相依但耦合度有沒有這麼高。

這就是Composite Builds的概念，也可以看到官網一開頭就寫了這句話`A composite build is a build that includes other builds.`如果還是有點難理解可以看看下面這張架構圖。

![](https://res.cloudinary.com/deu7aohfe/image/upload/v1714117387/202404243075098463/x73g9ghuukqiuuh5oezc.webp)

可以看到my-app & my-utils是build在my-composite這個專案裡面，但又自成一個方圓具備build及settings的gradle檔案，所以如果有需要獨立修改成一個專案是可行的。

## Composite Builds VS BuildSrc
實作 Composing build 之前，可能有看過 buildSrc 在 Gradle 執行時會自動編譯 buildSrc 裡的程式碼，可以將共用程式碼抽取到buildSrc內部，後續只要引用該檔案即可有興趣的話可以看我的[Medium](https://medium.com/工程師求生指南-sofware-engineer-survival-guide/how-to-migrate-kotlin-dsl-b857c153526d)，因為篇幅關係這邊就不多做贅述。

回歸正題，為什麼拋棄BuildSrc ??

可以從官方文件看出一些端倪，文中附上了備註意思就是雖然便於進行維護管理但只要有小更動就會 rebuild 整個專案，如果有需求時可以不要 rebuild 提升開發效率只是別忘了要定期去 rebuild 專案。

{% blockquote Use buildSrc to abstract imperative logic https://docs.gradle.org/current/userguide/organizing_gradle_projects.html#sec:build_sources Gradle 8.1.1 %}
A change in buildSrc causes the whole project to become out-of-date. Thus, when making small incremental changes, the --no-rebuild command-line option is often helpful to get faster feedback. Remember to run a full build regularly or at least when you’re done, though.
{% endblockquote %}

此外 Jose Raska 分享了 buildSrc 需注意的一些要點，要點大意如下。

{% blockquote Josef Raska https://docs.gradle.org/current/userguide/organizing_gradle_projects.html#sec:build_sources Stop using Gradle buildSrc Use composite builds instead %}
- dependency 更新會 rebuild 整個專案。
- cache 失效，不管是 Local build cache 還是 Remote Gradle cache。
- 剩下作者提及的 `Iteration speed is slow` 有點不太知道要如何翻譯，但我這邊理解是重複 build 的速度很慢如果理解有誤也歡迎留言。
  {% endblockquote %}

與 buildSrc 的不同在於 Composing build 是個別獨立的 module 每個都具備完整的 build gradle 並使用 include 方式來去組合一整個專案，所以如[官方](https://docs.gradle.org/current/userguide/composite_builds.html#composite_build_intro)所述可以根據需求獨立或是合併各個 module 這也造就了這兩種build type先天體質上的差異。

## Create include build module
先點選右上角的小齒輪在 Menu 中選擇 Project Structure ，並在 Modules 中選擇 "+" 新增一個 Module 名稱可以隨自己喜好去修改，範例中是使用 plugins 來作為 Module Name，步驟可以跟著下方圖示。

### Step 1.
![Open setting menu](https://res.cloudinary.com/deu7aohfe/image/upload/v1714351361/202404243075098463/jci28erordhelerrhyeq.webp)

### Step 2.
![Project structure](https://res.cloudinary.com/deu7aohfe/image/upload/v1714351627/202404243075098463/f136lm9pnzx2cmdijgf1.webp)


### Step 3.
![Create new module](https://res.cloudinary.com/deu7aohfe/image/upload/v1714351834/202404243075098463/jakyquup1rudciqatxn8.webp)

到這邊 Gradle sync 完就告了個小段落，最後在該 Module create 一個 {% label info@ settings.gradle.kts %} 的檔案即可。
![Create module setting file](https://res.cloudinary.com/deu7aohfe/image/upload/v1714352093/202404243075098463/tbuutjikagi5e4ofsqoj.webp)

## Module gradle
找到 Module 的 build.gradle.kts 加入下方的程式碼基本的設定這樣就可以了，但是我會需要在 Module 寫一些 extension 的東西所以看到 Sample 有加一些 dependencies 正常設定這樣即可。

```toml
gradle-tools-build = { module = "com.android.tools.build:gradle", version.ref = "8.3.2" }
kotlin-gradle-plugin = { module = "org.jetbrains.kotlin:kotlin-gradle-plugin", version.ref = "1.9.23" }
```

```kotlin
plugins {
    `kotlin-dsl`
}

repositories {
    google()
    mavenCentral()
}

dependencies {
    implementation(libs.gradle.tools.build)
    implementation(libs.kotlin.gradle.plugin)
    implementation(files(libs.javaClass.superclass.protectionDomain.codeSource.location))
}
```

## Module settings gradle
Settings的檔案是關聯catalog的，如果是新版IDE的話我記得不加這段依舊是可以吃到這個toml的檔案的。

```kotlin
dependencyResolutionManagement {
    repositories {
        mavenCentral()
    }
    versionCatalogs {
        create("libs") {
            from(files("../gradle/libs.versions.toml"))
        }
    }
}
```

## Gradle extension
因為 kotlinOptions 這個 function 蠻常被使用的所以就寫成 extension 的形式，方便之後在 Plugin class 呼叫。

```kotlin
internal fun CommonExtension<*, *, *, *, *, *>.kotlinOptions(block: KotlinJvmOptions.() -> Unit) {
  (this as ExtensionAware).extensions.configure("kotlinOptions", block)
}
```

## Plugin class
這部分算是 composing build 的核心，在這邊可以定義 plugins、defaultConfig 或是 dependencies 等，可以視情況需求寫一個共用的 plugin 未來開發上也可以省去很大一部分共用的程式碼，也可以避免有沒有修改到的問題。

後續各個 module 引用即可，接著來看看大框架如下透過project target去加入相關設定，需要注意的是 project extension 並沒有 kotlinOptions 所以在這邊必須 import 先前所寫的 CommonExtension，另一個是 configure 的 extension type 也會因應 modules 是 library 或是 application 有所不同。

```kotlin
class LibCommonPlugin : Plugin<Project> {
    override fun apply(target: Project) {
        with(target) {
          // plugins
            
            extensions.configure<ApplicationExtension> {
                // android config
            }

          // dependencies
        }
    }
}
```

這三個區塊怎麼寫呢？下面就簡單提一下可以仿照有個概念，細節可以照著Demo來去實作。

### Plugins
其實跟外面帶入 id 類似，但這邊要注意沒辦法在這邊宣告 `com.android.application` 跟 `com.android.library` 必須在外部 gradle 就宣告好這個 pluginDependencies，如果有更好的方法也歡迎告知我。

```kotlin
plugins.run {
    apply("org.jetbrains.kotlin.android")
    apply("de.mannodermaus.android-junit5")
    apply("quality.ktlint")
}
```

### Android Config
這邊就與 gradle 中 android config 設定差不多，版號的部分也是共用 object 所以可以看到Demo裡有 import 相關的檔案。

```kotlin
import com.awilab.plugins.Versions

defaultConfig {
    compileSdk = Versions.compileSdk
    minSdk = Versions.minSdk
    testInstrumentationRunner = "androidx.test.runner.AndroidJUnitRunner"
}

compileOptions {
    sourceCompatibility = Versions.jdk
    targetCompatibility = Versions.jdk
}

packaging {
    resources.excludes.apply {
        add("META-INF/AL2.0")
        add("META-INF/LGPL2.1")
    }
}

kotlinOptions {
    jvmTarget = Versions.jdk.toString()
}
```

### Dependencies
這部分就很看個人了，可以寫成 DependencyHandlerScope extension function 或是像我這樣宣告一個 libs 的變數統一在 Catalogs 管理，會這麼選擇的原因是因為好像沒有必要特別再拉一個 extension function，catalog 都提供 group 的方式了，且IDE還支援版本檢查。

```kotlin
val libs = the<org.gradle.accessors.dm.LibrariesForLibs>()

dependencies {
    "implementation"(libs.androidx.appcompat)
    "implementation"(libs.androidx.core)
    "implementation"(libs.androidx.lifecycle.runtime)


    //region Junit5
    "testImplementation"(libs.test.junit.api)
    "testRuntimeOnly"(libs.test.junit.jupiter.engine)
    //endregion

    "testImplementation"(libs.test.truth)
    "testImplementation"(libs.test.junit)
    "testRuntimeOnly"(libs.test.junit.vintage.engine)
    "androidTestImplementation"(libs.test.junit.ext)
    "androidTestImplementation"(libs.test.espresso.core)
}
```

## Module build gradle
先前寫好了 Plugins class 我們接著要在 module 中的 build gradle 定義好 plugin id 跟 implementationClass 讓 IDE 知道需關聯哪個 Class 後續就可以在專案中加入客製化的 plugin 了。

```kotlin
gradlePlugin {
    plugins {
        create("AppCommonConfig") {
            id = "plugins.app-common-config"
            implementationClass = "plugins.AppCommonPlugin"
        }
        create("Compose") {
            id = "plugins.compose"
            implementationClass = "plugins.ComposePlugin"
        }
        create("Ktlint") {
            id = "quality.ktlint"
            implementationClass = "quality.KtlintPlugin"
        }
    }
}
```

## Apply customer plugin
Sync完後只要在引用的 modules 宣告 plugins id 即可。

```kotlin
plugins {
    alias(libs.plugins.android.application)
    id("plugins.app-common-config")
    id("plugins.compose")
    id("quality.ktlint")
}
```

# Gradle Scan
這邊附上 Demo 的 gradle scan，如果要優化總是需要一份報告書作為佐證可以使用下方的command來產出這份報告，從報告的time line也可以讓人更瞭解初始化的差別。

```properties
./gradlew --scan
```

- [純Kotlin DSL](https://scans.gradle.com/s/p2oj5jhjcimmk/timeline)
- [Composing build](https://scans.gradle.com/s/ek6hefozbzuoe/timeline)

Demo因為規模很小，如果換成大專案省下的時間會非常的可觀，這邊就附上目前專案上模組化後的report time line體感上就會差很多了。

![Project report time line](https://res.cloudinary.com/deu7aohfe/image/upload/v1714012005/202404243075098463/uoctockxctsiwyblvla4.webp)

# 總結
複合式建構(Composite Builds)和模組化(Modularization)從來都不是必要的項目但是卻是專案到一定規模必須做的項目，像是職涯選擇不一定需要接觸過CI/CD但成為好的Team leader前一定會需要懂，畢竟職能需要資源調度也需要為專案負責。理解哪些工作項目屬於值得花時間的投資項目，畢竟科技始終來自於惰性如何更舒服的上班也是很重要的課題。

專案成長超過一個閾值就會是一個非常值得的項目，因為小專案複雜度不高進行複合式建構及模組化後編譯速度成效非常的有限，就像本文中Demo的專案前後相差2秒體感有限。在我重構公司專案前跑一次起碼是20分鐘，每次debug成本都變得非常昂貴，這時就代表該停下來重新探討下工作項目，或是履歷表更新的時間因為未來的結果是可以預期的。

隨著模組化的精細度越高專案複雜度也會線性增長，所帶來的開發負擔及門檻也會提高，這個又是另一個值得探討的課題了。感謝看到這邊的各位，希望在某些程度上有所幫助[範例程式](https://github.com/Chen-A-wi/ComposingBuildSample)在這邊，下次見！

# 参考
- Banner Photo by <a href="https://unsplash.com/@ilumire?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash">Jelleke Vanooteghem</a> on <a href="https://unsplash.com/photos/toddler-playing-with-two-wooden-blocks-Aqd30KmCc3g?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash">Unsplash</a>
- [Sharing dependency versions between projects](https://docs.gradle.org/current/userguide/platforms.html)
- [Migrate your build to version catalogs](https://developer.android.com/build/migrate-to-catalogs#learn-more)
- [Using Version Catalog on Android projects](https://proandroiddev.com/using-version-catalog-on-android-projects-82d88d2f79e5)
- [Stop using Gradle buildSrc. Use composite builds instead](https://proandroiddev.com/stop-using-gradle-buildsrc-use-composite-builds-instead-3c38ac7a2ab3)
- [How to manage dependencies between Gradle modules?](https://dev.to/aldok/how-to-manage-dependencies-between-gradle-modules-4jih)
- [Gradle doc - composite build](https://docs.gradle.org/current/userguide/composite_builds.html)
- [再见吧 buildSrc, 拥抱 Composing builds 提升 Android 编译速度](https://juejin.cn/post/6844904176250519565)
- [是时候弃用 buildSrc ,使用 Composing builds 加快编译速度了](https://blog.csdn.net/c6E5UlI1N/article/details/129574803)
