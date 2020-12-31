---
title: "LaTeXでビジネス文書を書く"
date: 2018-11-05T00:00:00+09:00
draft: false
categories: [ "LaTeX" ]
tags: [ "LaTeX", "business", "document" ]
---

LaTeXでビジネス文章を書くためのテンプレート的な，ちょっとしたスタイルファイルを書いた[^1]．

[^1]: 正確に言えば以前必要に迫られて書いたものを，せっかく始めたブログのネタにでもと思いスタイルファイルとして独立させた．

スタイルファイルの詳細やダウンロード等は[GitHub]( https://github.com/h1g0/latex_jbusinessdoc )へ．

## 使用例

```tex
\documentclass[uplatex,a4j,fleqn,12pt]{jsarticle}
\usepackage[top=35.01truemm,bottom=30truemm,left=30truemm,right=30truemm]{geometry}
\usepackage{jbusinessdoc}
%\documentnumber{文章番号第42号}
\date{平成yy年mm月dd日}
\destination{○○株式会社\\ご担当者様}
\author{××株式会社\\ほげほげ}
\title{ほげほげのご案内}
\begin{document}
\maketitle

\begin{haikei}
時下ますますご清栄のこととお喜び申し上げます。
平素はひとかたならぬお引き立てを賜り、厚く御礼申し上げます。

さて、このたび弊社ではほげほげがふがふがとなりました。

つきましては、ほげほげの程よろしくお願い申し上げます。
\end{haikei}

\begin{kigaki}
\begin{itemize}
    \item 日時：いつか
    \item 会場：どっか
\end{itemize}
\end{kigaki}
\end{document}
```

こんな感じでLaTeX文書を書くと

{{< img src="latex_jbuisinessdoc_sample.jpg" title="ビジネス文章っぽい"/>}}

こんな感じで出力される．

## 使い方


1. 普通のLaTeX文章を用意する（pLaTeXもしくはupLaTeXでjsarticleを使用することを想定してるので，それ以外だと上手く動かないかも）．
2. [ここ](https://github.com/h1g0/latex_jbusinessdoc)から`jbusinessdoc.sty`をダウンロードする
3. `jbusinessdoc.sty` をそのLaTeX文書のビルドが通る場所（例えば`.tex`ファイルと同じディレクトリ）に置く
4. プリアンブルに`\usepackage{jbusinessdoc}`と書く
5. `\title`，`\author`，`\date`のほかに，宛先を書くための`\destination`を書く
6. 必要があれば文章番号となる`\documentnumber`も書く（省略可）
7. 普通のLaTeXファイルと同じようにビルドする

## その他

「こういうビジネス文書を作りたいけどWordは使いたくない，できればLaTeXで書きたい」って人は（自分の他にも）意外と多いんじゃないかと思ってググったけど，それらしいノウハウがあまり出てこなかったので，そういう人の役に立てば．

## 参考図書
{{<rawhtml>}}
<iframe style="width:120px;height:240px;" marginwidth="0" marginheight="0" scrolling="no" frameborder="0" src="//rcm-fe.amazon-adsystem.com/e/cm?lt1=_blank&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=h1g00d-22&language=ja_JP&o=9&p=8&l=as4&m=amazon&f=ifr&ref=as_ss_li_til&asins=4774187054&linkId=efe733c7964ae7f22fff62c9ccefc29e"></iframe>
{{</rawhtml>}}