---
title: "RustでBrainf*ckインタプリタを書いた"
date: 2019-07-21T00:00:00+09:00
draft: false
categories: [ "Rust" ]
tags: [ "Rust" ]
---

最近[Rust](https://www.rust-lang.org/)を学び始めた。そこで、Rustで`Hello, world!`プログラムを書いた。  
といっても、普通に`println!("Hello, world!");`するだけでは何も面白くない。  
なので[Brainf*ck](https://ja.wikipedia.org/wiki/Brainfuck)で、インタプリタと一緒に。

<!--more-->

コードは（今のところ）こんな感じ。そしてideoneによる実行結果は[こんな感じ](https://ideone.com/C2luoc)。ちなみにGithubにも[上げてある](https://github.com/h1g0/rust-brainfxxk-interpreter/)。

```rust
use std::collections::VecDeque;
use std::collections::HashMap;
enum Token {
    Inc,
    Dec,
    IncPtr,
    DecPtr,
    StartLoop,
    EndLoop,
    Input,
    Output,
}
impl Token{
    fn tokenize(c:char) -> Option<Token>{
        match c{
            '+' => Some(Token::Inc),
            '-' => Some(Token::Dec),
            '>' => Some(Token::IncPtr),
            '<' => Some(Token::DecPtr),
            '[' => Some(Token::StartLoop),
            ']' => Some(Token::EndLoop),
            ',' => Some(Token::Input),
            '.' => Some(Token::Output),
             _  => None,
        }
    }
    fn tokenize_from_array(char_array: Vec<char>)->Vec<Token>{
        let mut token_array: Vec<Token> = Vec::new();
        for token in char_array.iter().filter_map(|c| Token::tokenize(*c)){
            token_array.push(token);
        }
        return token_array;
    }
    fn get_loop_token_ptr(token_array:&Vec<Token>)->
                         (HashMap<u32,u32>,HashMap<u32,u32>){
        let mut start_end_map : HashMap<u32,u32> = HashMap::new();
        let mut end_start_map : HashMap<u32,u32> = HashMap::new();
        let mut start_ptr_stack : Vec<u32> = Vec::new();
        let mut ptr : u32 = 0;
        for token in token_array{
            match *token{
                Token::StartLoop => {
                    start_ptr_stack.push(ptr);
                },
                Token::EndLoop => {
                    if let Some(start_ptr) = start_ptr_stack.pop(){
                        start_end_map.insert(start_ptr, ptr);
                        end_start_map.insert(ptr, start_ptr);
                    }else{
                        panic!("Too many ']' tokens detected!");
                    }
                },
                _ => {}
            }
            ptr+=1;
        }
        if ! start_ptr_stack.is_empty(){
            panic!("Too many '[' tokens detected!");
        }
        return (start_end_map, end_start_map);
    }
    //fn for token '+'.
    fn inc_mem_val(memory :&mut Vec<u32>, memory_ptr:u16){
        if let Some(val) = memory.get_mut(memory_ptr as usize) {
            *val += 1;
        }
    }
    //fn for token '-'.
    fn dec_mem_val(memory :&mut Vec<u32>, memory_ptr:u16){
        if let Some(val) = memory.get_mut(memory_ptr as usize) {
            *val -= 1;
        }
    }
    //fn for token '>'.
    fn inc_mem_ptr(memory_ptr:&mut u16){
        *memory_ptr +=1;
    }
    //fn for token '<'.
    fn dec_mem_ptr(memory_ptr:&mut u16){
        *memory_ptr -=1;
    }
    //fn for token '['.
    fn jump_loop_end_token_if_mem_0(mem_val:Option<&u32>, 
                                    loop_start_end_token_ptr_map:&HashMap<u32,u32>, 
                                    token_ptr : &mut u32){
        if let Some(val) = mem_val{
            if *val != 0{return;}
        }else{return;}
        if let Some(end_ptr) = loop_start_end_token_ptr_map.get(token_ptr){
            *token_ptr = *end_ptr;
        }else{
            panic!("no pair ']' token found.");
        }
    }
    //fn for token ']'.
    fn jump_loop_start_token_if_mem_not_0(mem_val:Option<&u32>, 
                                          loop_end_start_token_ptr_map:&HashMap<u32,u32>, 
                                          token_ptr : &mut u32){
        if let Some(val) = mem_val{
            if *val == 0{return;}
        }else{return;}
        if let Some(start_ptr) = loop_end_start_token_ptr_map.get(token_ptr){
            *token_ptr = *start_ptr;
        }else{
            panic!("no pair '[' token found.");
        }
    }
    //fn for token ','.
    fn put_char_from_input_to_mem(input_char_array:&mut VecDeque<char>,
                                  memory :&mut Vec<u32>, memory_ptr:u16){
        if let Some(val) = memory.get_mut(memory_ptr as usize){
            if let Some(c) = input_char_array.pop_front() {
                *val = u32::from(c);
            }
        }
    }
    //fn for token '.'.
    fn join_output_char_to_str(output_char:Option<char>, output_str:&mut String){
        if let Some(c) = output_char{
            output_str.push(c);
        }
    }
}
struct BfInterpreter{
    token_array : Vec<Token>,
    token_ptr : u32,
    memory : Vec<u32>,
    memory_ptr: u16,
    input : VecDeque<char>,
    output : String,
    loop_start_end_token_ptr_map : HashMap<u32,u32>,
    loop_end_start_token_ptr_map : HashMap<u32,u32>,
}
impl BfInterpreter{
    fn init(src: &str, input: &str) -> BfInterpreter {
        let ta = Token::tokenize_from_array(src.chars().collect::<Vec<char>>());
        let (lsetpm,lestpm) = Token::get_loop_token_ptr(&ta);
        BfInterpreter{
            token_array : ta,
            token_ptr : 0,
            //Brainf*ck's number of memory cell is defined to be larger than 30,000.
            //So this program should reserve size of 'u16::max_value()', 
            //which is expected to be 2^16 - 1 = 65,535.
            memory : vec![0 ; u16::max_value() as usize],
            memory_ptr : 0,
            input : input.chars().collect(),
            output : String::from(""),
            loop_start_end_token_ptr_map : lsetpm,
            loop_end_start_token_ptr_map : lestpm,
        }
    }
    fn exec(&mut self){
        let token_array = &self.token_array;
        while let Some(token) = token_array.get(self.token_ptr as usize){
            match *token{
                Token::Inc => {
                    Token::inc_mem_val(&mut self.memory, self.memory_ptr);
                }
                Token::Dec => {
                    Token::dec_mem_val(&mut self.memory, self.memory_ptr);
                }
                Token::IncPtr => {
                    Token::inc_mem_ptr(&mut self.memory_ptr);
                }
                Token::DecPtr => {
                    Token::dec_mem_ptr(&mut self.memory_ptr);
                }
                Token::StartLoop => {
                    Token::jump_loop_end_token_if_mem_0(
                        self.memory.get(self.memory_ptr as usize),
                        &self.loop_start_end_token_ptr_map,
                        &mut self.token_ptr
                        );
                }
                Token::EndLoop => {
                    Token::jump_loop_start_token_if_mem_not_0(
                        self.memory.get(self.memory_ptr as usize),
                        &self.loop_end_start_token_ptr_map,
                        &mut self.token_ptr
                        );
                }
                Token::Input => {
                    Token::put_char_from_input_to_mem(
                        &mut self.input, 
                        &mut self.memory, self.memory_ptr
                        );
                }
                Token::Output => {
                    Token::join_output_char_to_str(
                        self.memory.get(self.memory_ptr as usize)
                        .and_then(|i| std::char::from_u32(*i)),
                        &mut self.output);
                }
            }
            self.token_ptr += 1;
        }
    }
    fn output(&self)->&str{
        return &self.output;
    }
}
fn main (){
    let src: &str = 
        "+++++++++[>++++++++>+++++++++++>+++++<<<-]>.>++.+++++++..+++.>-.
        ------------.<++++++++.--------.+++.------.--------.>+.";
    let input : &str = "";
    let mut  bf = BfInterpreter::init(src, input);
    bf.exec();
    println!("{}",bf.output());
}
```

このコードの実行結果はもちろん

```
Hello, world!
```

となる。

Rustでこの「`Hello, world!`プログラム」を書いた感想として、

1. Rustで難解と言われる所有権やライフタイム、借用などは、横着せずにきちんと設計を考えればそれほど問題にはならない。
2. Rustはコンパイラが出すエラーメッセージが分かりやすいので、コンパイルエラーの修復はむしろ他の言語よりも楽であることが多かった。
3. RustはC++をよりモダンで安全に[^1]した感じの言語だと感じた。その点でC++に多少なりとも慣れ親しんだプログラマ[^2]に向いている言語だと感じた。
4. 結論として、Rustは書いていて愉しい言語であった。

なお、上記のコードではそこから以下の点で高速化を図っている。

1. Brainf*ckコード読み込み時のトークナイズ
2. ループ位置のメモ化[^3]

[^1]:そして少し過保護に
[^2]:例えば私のような
[^3]:上記トークナイズ処理時、`[`トークンの位置と対応する`]`トークンの位置をハッシュテーブルに載せる。そうすることで実行時に対応するループ位置へのジャンプが高速化できることが期待される

## 参考図書

{{<rawhtml>}}
<iframe style="width:120px;height:240px;" marginwidth="0" marginheight="0" scrolling="no" frameborder="0" src="//rcm-fe.amazon-adsystem.com/e/cm?lt1=_blank&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=h1g0-22&language=ja_JP&o=9&p=8&l=as4&m=amazon&f=ifr&ref=as_ss_li_til&asins=4297105594&linkId=2adddcd9dd378709637c18e7cb460a8f"></iframe>
<iframe style="width:120px;height:240px;" marginwidth="0" marginheight="0" scrolling="no" frameborder="0" src="//rcm-fe.amazon-adsystem.com/e/cm?lt1=_blank&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=h1g0-22&language=ja_JP&o=9&p=8&l=as4&m=amazon&f=ifr&ref=as_ss_li_til&asins=B077NSY211&linkId=bb9a7faf176e76c579c2aad4e12eed8f"></iframe>
{{</rawhtml>}}