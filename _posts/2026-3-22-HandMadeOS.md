---
layout: post
title: "Hello,World!を表示するシンプルなOS"
date: 2026-03-22
categories: [OS開発]
tags: ["C++", GRUB, OS, Program]
---

# "C++"とGRUBで最小構成のOSを作る
![demo](https://raw.githubusercontent.com/rimskyyamatov-lgtm/blogimages/main/HOS.png)
今回は、**"C++" + GRUB + NASM**を使って、
画面に「Hello, World!」を表示するだけの超シンプルなOSを作りました。
githubでコードを公開予定です。

構成はかなりミニマルで、

* boot.s（ブートコード）
* kernel.cpp（カーネル本体）
* linker.ld（リンカスクリプト）

の3つだけです。

---

## 全体の流れ

OSの起動はこんな感じで進みます：

1. GRUBがOSをロード
2. boot.sが実行される
3. スタックを初期化
4. C++の `kernel_main` を呼び出す
5. 画面に文字を表示

---

## boot.s の役割

```asm
MBOOT_MAGIC    equ 0x1BADB002
MBOOT_FLAGS    equ 0x03
MBOOT_CHECKSUM equ -(MBOOT_MAGIC + MBOOT_FLAGS)
```

これは**Multibootヘッダ**です。
GRUBに「このOSはMultiboot対応だよ」と伝えるためのもの。

---

```asm
_start:
    mov esp, stack_top
    call kernel_main
```

ここが一番重要👇

* `esp` にスタックの位置を設定
* C++の `kernel_main` を呼び出す

つまり、

👉 **アセンブリ → C++ に処理をバトンタッチしてる**

---

```asm
cli
.hang:
    hlt
    jmp .hang
```

* `cli`：割り込み禁止
* `hlt`：CPU停止（省電力）

無限ループでOSを止めてる状態

---

## kernel.cpp の中身

```cpp
unsigned short* terminal_buffer = (unsigned short*) 0xB8000;
```

ここめちゃ重要👇

👉 `0xB8000` はテキストモードのVRAM

---

```cpp
terminal_buffer[i] =
    (unsigned short) str[i]
    | (unsigned short) 0x07 << 8;
```

これは1文字分のデータを作ってる：

* 下位8bit → 文字
* 上位8bit → 色（0x07 = 白）

つまり、

👉 「文字 + 色」をまとめてVRAMに書いてる

---

```cpp
const char* str = "Hello,World!";
```

この文字列をループで書き込むことで、

👉 画面に直接表示される

---

## 重要ポイントまとめ

このOSの本質👇

* OSなのにprintfもstdも使ってない
* 画面出力は全部メモリ直書き
* GRUBが起動を担当
* C++でもOSは作れる

---

## ここから発展できること

この状態からいくらでも進化できる👇

* カーソル制御
* キーボード入力
* シェル作成
* スクロール機能
* GUI化（かなり難しい）

---

## 感想

めちゃくちゃシンプルだけど、

👉 「OSってこうやって動いてるんだ」

ってのが体感できる構成。

特に

* VRAM直書き
* スタック初期化
* C++呼び出し

このあたりはOS開発の基礎としてかなり重要。

---

## おまけ

QEMUで動かすと普通に動くけど、

👉 実機で動かすと挙動が変わることもある

---

## まとめ

最小構成のOSはこんな感じ：

* boot.s → 起動と橋渡し
* kernel.cpp → 処理本体
* GRUB → ロード担当

これだけで「OSっぽいもの」は作れる。

---
