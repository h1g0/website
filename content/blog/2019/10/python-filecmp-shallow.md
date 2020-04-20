---
title: "[Python] filecmpの引数shallowの細かい仕様について"
date: 2019-10-06T00:00:00+09:00
draft: false
categories: [ "Python" ]
tags: [ "Python" ]
---

Pythonの`filecmp`は引数に`shallow = True`
を付ける（デフォルト）と「浅い」ファイル比較を行う。この際に何を参照して「同じファイル」と判断するのか調べる必要があったのでメモ。
なお、この記事はPython 3.7 時点での情報である。

<!--more-->

結論から言うと、ファイルサイズと最終更新日時が同一ならば同一のファイルとして判断される。

解説として、「浅い」ファイル比較はファイルごとにシグネチャを生成し、それらが同じならば同じファイルであると判断する。そしてその際の生成コードが

```python
 _sig(st):
    return (stat.S_IFMT(st.st_mode),
            st.st_size,
            st.st_mtime)
```

となっている[^1]。

[^1]: [https://github.com/python/cpython/blob/3.7/Lib/filecmp.py](https://github.com/python/cpython/blob/3.7/Lib/filecmp.py)

引数の`st`は`os.stat_result`オブジェクトであるので、`st_mode`、 `st_size`、 `st_mtime`の3つから同じファイルか判断していることになる。

このうち、 `st_mode`はファイルタイプとファイルモード[^2]の判定であるので、実質的には`st_size`、つまりファイルサイズと、`st_mtime`、つまり最終更新日時で判定していることになる。

[^2]:ディレクトリか？ファイルか？シンボリックリンクか？etc.

なお、`shallow = False`として「深い」ファイル比較を行うと、文字通り **バイト単位でファイルの同一性をチェック**する。当然ながら「浅い」ファイル比較よりも処理に時間がかかるが、より確実性を求める場合はこちらを使用することも検討すべきかもしれない。

{{<rawhtml>}}
<iframe style="width:120px;height:240px;" marginwidth="0" marginheight="0" scrolling="no" frameborder="0" src="//rcm-fe.amazon-adsystem.com/e/cm?lt1=_blank&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=h1g0-22&language=ja_JP&o=9&p=8&l=as4&m=amazon&f=ifr&ref=as_ss_li_til&asins=4873117380&linkId=3cf7dcb6b89b0132bc21502a73f207f2"></iframe>
{{</rawhtml>}}