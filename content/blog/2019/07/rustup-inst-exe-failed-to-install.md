---
title: "[Rust] rustup-inst.exeでの新規インストール時に「アクセスが拒否されました。(os error 5)」が出る時の対処法"
date: 2019-07-28T00:00:00+09:00
draft: false
categories: [ "Rust" ]
tags: [ "Rust" ]
---

Windows 10に`rustup-inst.exe`を使ってRust[^1]をインストールしようとしたところ、`アクセスが拒否されました。(os error 5)`のエラーが出てインストールできなかった。それを解決した際のメモ。

[^1]: 厳密にはrustup

なお、インストールしようとしたrustupのバージョンは

```
rustup 1.18.3 (435397f48 2019-05-22)
```

である。

<!--more-->

## 解決法

とりあえずOSの再起動等を試してみても効果がなかったのでググってみたところ、githubのプロジェクトページに

[fresh install fails on Windows: "error: could not rename component file from '... 
ust-docs\share/doc/rust/html' ..."](https://github.com/rust-lang/rustup/issues/1912)

というIssueが上がっているのを見つけた。読んでみたところ、自分の問題と同じであった。

この記事を参考にアンチウィルスソフト（自分の場合はAvira）のリアルタイムプロテクションを一時的に切って`rustup-inst.exe`を再試行したところ、今度は正常にインストールできた。

以前`rustup-inst.exe`を使って同環境でセットアップを行った際には問題は発生しなかったので、`rustup-inst.exe`のアップデートによるものかAviraの更新に起因する問題なのかもしれない。

{{<rawhtml>}}
<iframe style="width:120px;height:240px;" marginwidth="0" marginheight="0" scrolling="no" frameborder="0" src="//rcm-fe.amazon-adsystem.com/e/cm?lt1=_blank&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=h1g0-22&language=ja_JP&o=9&p=8&l=as4&m=amazon&f=ifr&ref=as_ss_li_til&asins=4297105594&linkId=2adddcd9dd378709637c18e7cb460a8f"></iframe>
<iframe style="width:120px;height:240px;" marginwidth="0" marginheight="0" scrolling="no" frameborder="0" src="//rcm-fe.amazon-adsystem.com/e/cm?lt1=_blank&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=h1g0-22&language=ja_JP&o=9&p=8&l=as4&m=amazon&f=ifr&ref=as_ss_li_til&asins=B077NSY211&linkId=bb9a7faf176e76c579c2aad4e12eed8f"></iframe>
{{</rawhtml>}}