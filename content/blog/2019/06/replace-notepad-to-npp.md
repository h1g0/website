---
title: "Notepad++の64bit版でメモ帳を置き換える際の注意点"
date: 2019-06-22T00:00:00+09:00
draft: false
categories: [ "others" ]
tags: [ "app","notepad++" ]
---

tepad++はv7.5.9より、Windows標準のメモ帳を「置き換える」((正確にはメモ帳を起動しようとするとメモ帳の代わりにNotepad++が立ち上がる))ことが可能になっている。

<!--more-->

参考：

{{<tweet 1052303463156047872>}}

しかし、公式サイトやそれを継承した窓の杜の記事の方法は32bit版のものであり、64bit版を使用している場合は失敗してしまう[^1]ので注意が必要

[^1]: メモ帳を起動しようとすると`notepad++.exe`が見つからない旨のエラーが発生する。

具体的には、公式サイトで紹介されている

```
reg add "HKLM\Software\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\notepad.exe" /v "Debugger" /t REG_SZ /d "\"%ProgramFiles(x86)%\Notepad++\notepad++.exe\" -notepadStyleCmdline -z" /f
```

では、Notepad++のインストール先として32bit版のそれである`Program Files(x86)`フォルダが指定されている。それを64bit版に書き換えて

```
reg add "HKLM\Software\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\notepad.exe" /v "Debugger" /t REG_SZ /d "\"%ProgramFiles%\Notepad++\notepad++.exe\" -notepadStyleCmdline -z" /f
```

とする。
そして上記コマンドを管理者権限のコマンドプロンプトで実行すれば、無事置き換えが完了する。

{{<rawhtml>}}
<iframe style="width:120px;height:240px;" marginwidth="0" marginheight="0" scrolling="no" frameborder="0" src="//rcm-fe.amazon-adsystem.com/e/cm?lt1=_blank&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=h1g0-22&language=ja_JP&o=9&p=8&l=as4&m=amazon&f=ifr&ref=as_ss_li_til&asins=4873113822&linkId=9527b1bf60f5733bf69d4726401ae938"></iframe>
{{</rawhtml>}}