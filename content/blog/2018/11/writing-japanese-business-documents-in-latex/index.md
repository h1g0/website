---
title: "LaTeXでビジネス文書を書く"
date: 2018-11-05T00:00:00+09:00
draft: false
categories: [ "LaTeX" ]
tags: [ "LaTeX" ]
---

LaTeXでビジネス文章を書くためのテンプレート的な，ちょっとしたスタイルファイルを書いた[^1]．

[^1]: 正確に言えば以前必要に迫られて書いたものを，せっかく始めたブログのネタにでもと思いスタイルファイルとして独立させた．

スタイルファイルの詳細やダウンロード等は[GitHub]( https://github.com/h1g0/latex_jbusinessdoc )へ．

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

「こういうビジネス文書を作りたいけどWordは使いたくない，できればLaTeXで書きたい」って人は意外と多いんじゃないかと思ってググったけど，それらしいノウハウがあまり出てこなかったので．