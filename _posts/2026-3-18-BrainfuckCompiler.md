---
layout: post
title: "BrainfuckCompilerについて"
date: 2026-03-18
categories: [programming]
tags: [langage]
---

# NASM(NetwideAssembler)で作成されたBrainfuckCompiler

さて、前回紹介したデモです。

![demo](https://raw.githubusercontent.com/rimskyyamatov-lgtm/blogimages/main/brf.gif)

このデモです。ファイルの拡張子からなんとなく察せたかと思われますが、これはBrainfuckのプログラムです。  
[Brainfuckとは](https://ja.wikipedia.org/wiki/Brainfuck)

このように、元々がコンパイラサイズを小さくするために作られた言語であるため、インタプリタやコンパイラが非常に作りやすいのです。

---

## デモコード

デモに使ったプログラムは[ここ](https://github.com/rimskyyamatov-lgtm/Linux-x86_64-nasm-ELF-Brainfuck-Compiler)です。  
これはx86_64 Linuxのnasmで書かれています。  

[nasmとは](https://ja.wikipedia.org/wiki/Netwide_Assembler)

これは非常に低レベルな言語で、OSなど様々な要素に依存するので一部の環境では動きません。

---

## 説明

このプログラムはコマンドライン引数で入力された `.bf` ファイルを、拡張子を取り除いた名前で実行ファイルとして保存します。  
（フォルダによってなぜか動かない場合があるのは原因不明です）

簡単な解説だけしておきます（エントリーポイントなどは省略）。

---

## syscallについて

nasm (x86_64 Linux) では、表示や入力には `syscall` を使います。

### 表示の例（Hello, World!）

```asm
section .text
global _start
_start:
mov rax,1
mov rdi,1
lea rsi,[rel msg]
mov rdx,13
syscall

mov rax,60
xor rdi,rdi
syscall

msg db "Hello,World!",10
```
この様に、syscallを使うと表示や入力、さらにはファイルの読み書きなどが使えます。
(このコードではsys_writeとsys_exit)

syscallは機械語で
```
0F 05
```
とします。
これを使えば、Brainfuckの"."と","を再現できます

その他にも、nasmには
"+"に対応する"add"や、"-"に対応する"sub"などがあります。

これらは低レベルであるがゆえ、コンパイラの工夫次第で実行ファイルを極限まで小さくできます。

例えばHello World!はC++で16.5kbなのに対し、このコンパイラではたったの414byteです
## まとめ

Brainfuckは非常にシンプルな言語

そのためコンパイラを小さく作れる

syscallを使えば低レベルに直接アクセスできる

nasmを使うことで極限まで無駄を省いた実装が可能
## 今後

今回は簡単な紹介だけでしたが、
今後は(気が向いたら)以下についても解説する予定です。

各命令（> < [ ]）の実装

ループ処理の仕組み

実際のコンパイル処理の流れ
