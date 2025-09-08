+++
date = '2025-09-08T11:18:12+08:00'
title = '客製化 Hugo Archetypes 內容樣板'
tags = ["hugo", "tech", "zh"]
+++


## 內容檔案

Hugo 預設網站內容檔案被放在 `content/` 底下，所以建立新文章的最簡單當方法就是在這裡建立 markdown 檔案。
```
content/
└── about.md    # 對應到 https://lywgit.github.io/about/
```

可以包含子資料夾，資料夾結構會對應到內容所在的網址:
 
```
content/
└── posts/    
    └── xxx.md  # 對應到 https://lywgit.github.io/posts/xxx/
```

如果想要在文章內放置圖片，則改使用「資料夾搭配 index.md」 的方式建立，並將相關檔案放在一起，這種方式稱為 bundle：
```
content/
└── posts/    
    └── yyy/    # 對應到 https://lywgit.github.io/posts/yyy/
        ├── index.md  
        └── image_for_yyy.png

# 在 content/posts/index.md 中使用標準的 markdown 語法引用圖片： 
# ![alternative text](image_for_yyy.png "image caption")
```

## 元資料

內容檔（上面例子裡的 about.md, xxx.md, index.md）需要包含相關的元資料（meta data）才能正確地被處理（例如依照時間排序），我使用 toml 格式，故寫在 +++ 區間之內：
```
+++
date = '2025-09-08T11:18:12+08:00'
title = '客製化 Hugo Archetypes 內容樣板'
tags = ["hugo", "tech", "zh"]
+++
... 內容 ...
```

## 使用 hugo new content 指令

除了手動建立檔案之外，Hugo 亦提供使用指令 `hugo new content <content-path>` 的方式建立檔案，好處之一是可以自動帶入元資料

```bash
$ hugo new content content/posts/yyy.md 
# 會產生 content/posts/yyy.md 檔案
# 並且自動帶入 archetypes/default.md 定義的元資料
```

`archetypes/default.md` 定義了 `.md` 檔時預設的樣板格式。
包含自動帶入建立當下的時間（例如 '2025-09-08T11:18:12+08:00'）以及從檔案名稱產生對應的文章標題。

```
+++
date = '{{ .Date }}' 
draft = true 
title = '{{ replace .File.ContentBaseName "-" " " | title }}' 
+++
```

## 客製化 archetypes

我編輯內容時常有固定的步驟：

1. 如果需要插入圖片，要手動建立 bundle 資料夾以及 index.md 檔案
2. 我習慣將內容檔案名稱以日期開頭，但不希望日期出現在文章的標題
3. 習慣將文章加上 tags 元資料欄位

尤其自己建立資料夾是比較麻煩的事，就參考網路文章透過定義自己客製化樣板來偷懶

```
archetypes/
├── default.md    # 原本的預設，我選擇不去動它
├── post.md       # 新增 post 情境
└── bundle/       # 新增 bundle 情境 
    └── index.md
```

其中 post.md 和 bundle/index.md 內容相同，會自動將 YYYY-MM-DD- 前綴字串去除以及加上 tags = []
```
+++
date = '{{ .Date }}'
title = '{{ replace (replaceRE  `^\d{4}-\d{2}-\d{2}-` "" .Name 1)  "-" " " | title }}'
tags = []
draft = true
+++
```

使用時以 `--kind` 來調用：

```bash
# 產生單一 md 檔
$ hugo new content content/posts/2025-09-08-hugo-archetypes.md --kind post

# 產生資料夾 + index.md
$ hugo new content content/posts/2025-09-08-hugo-archetypes --kind bundle
```



## 參考資料

[Maximizing the convenience factor: archetypes in Hugo](https://cloudcannon.com/blog/maximizing-the-convenience-factor-archetypes-in-hugo/)

```
```