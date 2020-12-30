---
title: "sudachi.rsを使って遊んでみる（ための下準備）"
date: 2020-04-16T23:52:11+09:00
categories: [ "Rust" ]
tags: [ "Rust","自然言語処理","sudachi" ]
draft: false
---

[sudachi.rs](https://github.com/sorami/sudachi.rs)という、Rustで実装された形態素解析器がある。それをライブラリとして何か書いて遊ぶための下準備的なメモ。

<!--more-->

## sudachi.rsとは

[Sudachi](https://github.com/WorksApplications/Sudachi)という形態素解析器（実装はJava）のRustによる非公式クローン。
詳しくは[作者の方のブログエントリ](https://qiita.com/sorami/items/7934fec2074c493c0f7d)を参照してほしい。

Sudachiには他にも[Pythonによる実装（公式）](https://github.com/WorksApplications/SudachiPy)や[Goによる実装（非公式）](https://github.com/msnoigrs/gosudachi)があるようだ。

## 実際に遊んで見る

`sudachi.rs`はまだ[crates.io](https://crates.io/)に登録されていない[^1]。
そのため、自分でソースコードを`clone`しておく必要がある。

[^1]: 作者の方もissueとして認識してはいるようである。

```bash
$ git clone https://github.com/sorami/sudachi.rs.git
```

また、辞書を[こちら](https://github.com/WorksApplications/SudachiDict)からダウンロードして展開しておく。

辞書の配置ディレクトリは（プログラムから呼べる場所であれば）どこでも良いが、ここではsudachi.rsのREADMEに従って`sudachi.rs/src/resources/system.dic`とした。

次に、先程`sudachi.rs`をクローンしたのと同じディレクトリで

```bash
$ cargo new --bin sudachi_test
```

こんな感じで、適当な名前で新しいプロジェクトを作る。

そしたら中の`Cargo.toml`に

```toml
[dependencies.sudachi]
path = "../sudachi.rs/"
```

こんな感じでsudachi.rsへの相対パスを教えてやる。

そして`src/main.rs`を開き、

```rust
extern crate sudachi;
use sudachi::tokenizer::Mode;
use sudachi::tokenizer::Tokenizer;

fn main() {
    let txt = "東海の小島の磯の白砂にわれ泣きぬれて蟹とたはむる";

    let bytes = include_bytes!("../../sudachi.rs/src/resources/system.dic");
    let tokenizer = Tokenizer::new(bytes);

    let morpheme_list = tokenizer.tokenize(&String::from(txt), &Mode::C, false);

    for morpheme in morpheme_list{
        println!("{}（{}）",morpheme.surface(),morpheme.reading_form() );
    }
}
```

こんな感じでコードを書いてやる[^2]。

[^2]: 辞書ファイルへのパスが長すぎるので、置く場所を自プロジェクトのディレクトリ内にしてもよかったかもしれない。

`cargo run`で実行すると、

```bash
東海（トウカイ）
の（ノ）
小島（コジマ）
の（ノ）
磯（イソ）
の（ノ）
白砂（ハクシャ）
に（ニ）
われ（ワレ）
泣きぬれ（ナキヌレ）
て（テ）
蟹（カニ）
と（ト）
たはむる（タハムル）
```

と、実に簡単に分かち書きと読みがなの付与ができた[^3]。

[^3]: 「白砂（しらすな）」を「はくしゃ」としているが、これは十分許容範囲であるように思われる。

## 利用できる関数

分割された各形態素に対して利用できる関数は次の通り[^4]。

- `surface`:分割された後の形態素
- `pos`:その形態素に関する情報
- `normalized_form`: 正規化された形
- `reading_form`: 読みがな
- `dictionary_form`: 辞書形

[^4]: [https://github.com/sorami/sudachi.rs/blob/develop/src/morpheme.rs](https://github.com/sorami/sudachi.rs/blob/develop/src/morpheme.rs)


## ここまでの感想

わずか数行のコードで、とても簡単にRustでの自然言語処理の第一歩を踏み出すことができた。
ここから更に色々遊べそうである。

## 追記

[sudachiclone-rs](https://github.com/Yasu-umi/sudachiclone-rs)という、別の方が作成したSudachiのRustクローン[^5]もあるらしい。
そしてこちらは[crates.io](https://crates.io/)に[登録](https://crates.io/crates/sudachiclone)されているようだ。

[^5]:正確にはSudachiPyのRustクローン

{{<rawhtml>}}
<iframe style="width:120px;height:240px;" marginwidth="0" marginheight="0" scrolling="no" frameborder="0" src="//rcm-fe.amazon-adsystem.com/e/cm?lt1=_blank&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=h1g00d-22&language=ja_JP&o=9&p=8&l=as4&m=amazon&f=ifr&ref=as_ss_li_til&asins=4297105594&linkId=2adddcd9dd378709637c18e7cb460a8f"></iframe>
<iframe style="width:120px;height:240px;" marginwidth="0" marginheight="0" scrolling="no" frameborder="0" src="//rcm-fe.amazon-adsystem.com/e/cm?lt1=_blank&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=h1g00d-22&language=ja_JP&o=9&p=8&l=as4&m=amazon&f=ifr&ref=as_ss_li_til&asins=B077NSY211&linkId=bb9a7faf176e76c579c2aad4e12eed8f"></iframe>
{{</rawhtml>}}