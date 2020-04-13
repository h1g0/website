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