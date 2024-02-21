--- 
draft : false
date : 2024-02-16T14:37:18+01:00
title : "How to Embed HTML inside Hugo Article"
tags : ["Hugo"]
showTableOfContents : true
type: "post"
---

To embed HTML code inside a Hugo article, just add
```
[markup.goldmark.renderer]
  unsafe = true
```
inside `config.toml`

By default the “unsafe” option in [markup.goldmark.renderer] is set to false.

From the [documentation](https://gohugo.io/getting-started/configuration-markup/#goldmark)

> By default, Goldmark does not render raw HTMLs and potentially dangerous links. If you have lots of inline HTML and/or JavaScript, you may need to turn this on.

