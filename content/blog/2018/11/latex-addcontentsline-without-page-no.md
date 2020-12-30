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

{{<rawhtml>}}
<iframe style="width:120px;height:240px;" marginwidth="0" marginheight="0" scrolling="no" frameborder="0" src="//rcm-fe.amazon-adsystem.com/e/cm?lt1=_blank&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=h1g00d-22&language=ja_JP&o=9&p=8&l=as4&m=amazon&f=ifr&ref=as_ss_li_til&asins=4774187054&linkId=efe733c7964ae7f22fff62c9ccefc29e"></iframe>
{{</rawhtml>}}