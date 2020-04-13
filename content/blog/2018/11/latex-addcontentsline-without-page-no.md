---
title: "LaTeXで目次にページ番号のない独自項目を追加する"
date: 2018-11-10T00:00:00+09:00
draft: false
categories: [ "LaTeX" ]
tags: [ "LaTeX" ]
---

一般的にLaTeXで目次に独自の項目を追加するには，`\addcontentsline`を用いるのが一般的である．
しかしこのやり方では，ページ番号が自動的に付加されてしまう．これはページ番号を出力したくない場合には不便である．そういう時の解決法．

<!--more-->

```tex
\usepackage{tocloft}
```

した上で

```tex
\cftaddtitleline{toc}{section}{\numberline{番号}{セクション名}{}
```

すると`\addcontents`するよりも楽．