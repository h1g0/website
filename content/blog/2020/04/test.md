---
title: "Hello, Hugo!"
date: 2020-04-09T00:12:59+09:00
categories: [ "Misc" ]
tags: [ "Misc","WordPress","Hugo", "GitHub" ]
---

このサイトのフレームワークをWordPressから[Hugo](https://gohugo.io/)に乗り換えてみた。

<!--more-->

その理由として、

- Markdownでコンテンツを書ける
- Gitで管理できる
- そのため、GitHub Pagesでホスティングできる
- WordPressはウェブサイトを動的に生成するため、サイトの構成やサーバのスペック等によっては動作が重い。また、脆弱性も懸念点となる
- Hugoは静的サイトジェネレータであるため、WordPressよりも軽量な動作が期待できる

などが挙げられる。

そして[こちらのGitHub Actions](https://github.com/marketplace/actions/github-pages-action)を使うことで、書いたMarkdownをGitHub上のリモートリポジトリに`push`すると自動的にHugoを走らせ、生成物をウェブサイトにデプロイするようにできた。大変便利。

しばらくこれでやってみよう。

{{<rawhtml>}}
<iframe style="width:120px;height:240px;" marginwidth="0" marginheight="0" scrolling="no" frameborder="0" src="//rcm-fe.amazon-adsystem.com/e/cm?lt1=_blank&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=h1g0-22&language=ja_JP&o=9&p=8&l=as4&m=amazon&f=ifr&ref=as_ss_li_til&asins=4873114403&linkId=b9ac9062fce27f4a4372c11138d1814f"></iframe>
{{</rawhtml>}}