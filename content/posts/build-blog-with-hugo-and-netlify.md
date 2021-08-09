---
title: "使用 Hugo ＋ Netlify 建立個人 Blog"
date: 2021-08-08T14:52:23+08:00
draft: true
tags: ["Hugo", "Netlify"]
---
使用`Hugo`建立個人blog，並部署到 `Netlify`。
<!--more-->

## 前言
以前一直有想寫 blog 的念頭，但我的拖延症候群實在太嚴重QQ

總覺得還是需要紀錄一下自己學習過程，這邊選擇 `Hugo` 搭建個人 Blog~

## 建立說明
搭建很簡單，依照`Hugo`官方教學建立即可。
```Bash
hugo new site blog
cd blog
git init
# add theme
git submodule add https://github.com/dillonzq/LoveIt themes/LoveIt
```
修改`config.toml`做簡單設定，根據使用的主題 ，設定欄位會有所不同。

這次我所使用的主題是 [Loveit](https://github.com/dillonzq/LoveIt)，其他主題請參考[官方](https://themes.gohugo.io/)
```TOML {linenos=table,hl_lines=[50]}
baseURL = "http://blog.kenlee.me"
# determines default content language
defaultContentLanguage = "en"
title = "Ken's 學習日誌"
theme = "LoveIt"
# 在文章內使用 emoji，emoji code參考https://www.webfx.com/tools/emoji-cheat-sheet
enableEmoji = true

# 與theme相關設定，這邊先省略
[params]
  ...

[menu]
  [[menu.main]]
    identifier = "posts"
    # you can add extra information before the name (HTML format is supported), such as icons
    pre = ""
    # you can add extra information after the name (HTML format is supported), such as icons
    post = ""
    # 顯示於menu的文字
    name = "文章"
    url = "/posts/"
    # title will be shown when you hover on this menu link
    title = ""
    weight = 1
  [[menu.main]]
    identifier = "tags"
    pre = ""
    post = ""
    name = "Tags"
    url = "/tags/"
    title = ""
    weight = 2
  [[menu.main]]
    identifier = "categories"
    pre = ""
    post = ""
    name = "Categories"
    url = "/categories/"
    title = ""
    weight = 3

# Markup related configuration in Hugo
[markup]
  # Syntax Highlighting (https://gohugo.io/content-management/syntax-highlighting)
  [markup.highlight]
    # false is a necessary configuration (https://github.com/dillonzq/LoveIt/issues/158)
    noClasses = false
    # 能夠在code中highlight部分內容
    codeFences = true
    guessSyntax = true
    # 在code顯示行數
    lineNos = true
    lineNumbersInTable = true

# Author config
[author]
  name = "Ken"
  email = "kenlee0852@gmail.com"
  link = ""

# Permalinks config
[Permalinks]
  posts = ":year/:month/:filename"
```
呈現結果如下：
{{< figure src="/images/hugo-demo.png" title="Hugo demo" class="img-border" >}}

64行是設定文章 url 的 pattern，預設是文章 title，個人喜歡加入時間戳記，呈現結果如下：
`http://localhost:1313/2021/08/post-title/`。

`config.toml`設定完成後，有需要可以在自行加入 favicon ，完成後就可以開始寫文章囉！

使用指令`hugo new posts/post-title.md`建立文章：
``` Markdown
---
title: "Post Title"
date: 2021-08-09T14:14:01+08:00
draft: true
---
```
預設只有 meta data，Markdown 語法這邊不再贅述，另外 Hugo 支援一些 shortcode，參考[官方文件](https://gohugo.io/content-management/shortcodes/)。

使用 `hugo serve` 指令啟動服務，會發現看不到文章，是因為預設不會 render 草稿文章，需要改用 `hugo serve -D` 指令，呈現結果如上圖。

## 使用 Tag 與 Category
Hugo 預設就會產生 `Tag` 與 `Category` 的分類，只需要在文章 meta 中加上即可：
``` Markdown
---
title: "Post Title"
date: 2021-08-09T14:14:01+08:00
draft: true
tags: ["foo", "bar"]
categories: ["Develop"]
---
```
即可在 menu `Tags`、`Categories` 看到文章分類了。

## 部署到 Netlify
Hugo 能夠部署到多個平台，ex: Github pages、AWS Amplify、Firebase、Netlify...，這邊選擇 Netlify，主要是看重其 CD 功能。

至 [Netlify](https://app.netlify.com/) 建立帳號，建立完成，點選**New site from Git**，
{{< figure src="/images/netlify-new-site.png" title="Netlify new site" class="img-border" >}}

選擇 source code repistory 平台，這邊使用 Github，
{{< figure src="/images/netlify-cd-provider.png" title="Netlify new site" class="img-border" >}}