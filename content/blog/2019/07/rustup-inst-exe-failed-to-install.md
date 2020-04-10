---
title: "[Rust] rustup-inst.exeでの新規インストール時に「アクセスが拒否されました。(os error 5)」が出る時の対処法"
date: 2019-07-28T00:00:00+09:00
draft: false
categories: [ "Rust" ]
tags: [ "Rust" ]
---

Windows 10に`rustup-inst.exe`を使ってRust((厳密にはrustup))をインストールしようとしたところ、`アクセスが拒否されました。(os error 5)`のエラーが出てインストールできなかった。それを解決した際のメモ。

なお、インストールしようとしたrustupのバージョンは

```
rustup 1.18.3 (435397f48 2019-05-22)
```

である。

## 解決法

とりあえずOSの再起動等を試してみても効果がなかったのでググってみたところ、githubのプロジェクトページに

[fresh install fails on Windows: "error: could not rename component file from '... 
ust-docs\share/doc/rust/html' ..."](https://github.com/rust-lang/rustup/issues/1912)

というIssueが上がっているのを見つけた。読んでみたところ、自分の問題と同じであった。

この記事を参考にアンチウィルスソフト（自分の場合はAvira）のリアルタイムプロテクションを一時的に切って`rustup-inst.exe`を再試行したところ、今度は正常にインストールできた。

以前`rustup-inst.exe`を使って同環境でセットアップを行った際には問題は発生しなかったので、`rustup-inst.exe`のアップデートによるものかAviraの更新に起因する問題なのかもしれない。
