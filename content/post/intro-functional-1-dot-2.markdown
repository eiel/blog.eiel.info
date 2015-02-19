---

title: "読書メモ 関数プログラミング入門 Haskellで学ぶ原理と技法 1.2 評価"
date: 2013-01-13T21:47:00+09:00
comments: true
categories: ["Haskell","読書メモ","関数プログラミング入門"]
---

[関数プログラミング入門 Haskell で学ぶ原理と技法](http://www.amazon.co.jp/gp/product/427406896X/ref=as_li_ss_tl?ie=UTF8&camp=247&creative=7399&creativeASIN=427406896X&linkCode=as2&tag=eiel-22) の読書メモです。


本節は評価を中心に式の簡約化についての説明です。

正規形という形を目指して式を簡略化していきますが、関数プログラミングにおいては、どのような手順で簡約しても最終結果が一致するのが特徴です。

代入のような破壊的な操作が存在する世界では、順序が影響します。
そこが命令型のプログラミングと違うところでしょう。そのため並列処理させるたべ順序に影響しないため、関数プログラミングが評価されてる部分だと思います。

話が脱線しましたが、簡約の手順には 1つだけでなく複数存在する可能性があります。そのためプログラミング言語によっては簡約の評価戦略が違うようです。
あと、簡約の手順が違う場合、停止しない場合が存在することもあります。

練習問題は評価の順序、停止性について考えるような問題でした。

## 関連

* [メモ用のリポジトリ 1章](https://github.com/eiel/Introduction-to-Functional-Programming-using-Haskell/blob/master/1/index.org)

* [1.2の練習問題](https://github.com/eiel/Introduction-to-Functional-Programming-using-Haskell/blob/master/1/1.2.md)
