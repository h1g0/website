---
title: "[Rust] Bevyでブラウザ上で動くライフゲームを作った"
date: 2021-12-31T15:14:21+09:00
categories: [ "Rust" ]
tags: [ "Rust", "Bevy", "wasm", "game" ]
draft: false
---
Rust用ゲームエンジンである[Bevy](https://bevyengine.org/)を使って、ブラウザ上で動く[ライフゲーム](https://ja.wikipedia.org/wiki/%E3%83%A9%E3%82%A4%E3%83%95%E3%82%B2%E3%83%BC%E3%83%A0)を作ってみた。

「続きを読む」以下に実際に動作するデモが置いてある。

<!--more-->

## 動作デモ

{{<rawhtml>}}
<iframe id="gameOfLife"
    title="Conway's Game of Life"
    width="640"
    height="480"
    src="./pkg/index.html">
</iframe>
{{</rawhtml>}}

## ソースコード

[Github](https://github.com/h1g0/conways_game_of_life.rs)