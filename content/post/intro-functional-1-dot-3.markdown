---

title: "読書メモ 関数プログラミング入門 Haskellで学ぶ原理と技法 1.3 値"
date: 2013-01-15T01:17:00+09:00
comments: true
categories: ["Haskell","読書メモ","関数プログラミング入門"]
---

[関数プログラミング入門 Haskell で学ぶ原理と技法](http://www.amazon.co.jp/gp/product/427406896X/ref=as_li_ss_tl?ie=UTF8&camp=247&creative=7399&creativeASIN=427406896X&linkCode=as2&tag=eiel-22) の読書メモです。

本節は **値** について。

値と式の関係。値と正格関数、非正格関数。がメインです。

値は 式を用いて表現できます。値を表現する式はひとつだけではなく、複数存在し、評価機が出力する表現は **標準表現** が利用されます。基本的には評価可能な式が表示されるということのようです。Rubyでいうと pメソッドの出力結果に近そうです。
また、値を表示しようとすると停止しない可能性が存在します。このような状態になる関数を正格関数と呼びます。具体的にシンプルに定義されてるのでわかりやすいです。正格でない関数は非正格なりますが、これは遅延評価でないと定義できないそうです。

練習問題は正格と非正格について考える問題でした。

## 関連

* [メモ用のリポジトリ 1章](https://github.com/eiel/Introduction-to-Functional-Programming-using-Haskell/blob/master/1/index.org)

* [1.3の練習問題](https://github.com/eiel/Introduction-to-Functional-Programming-using-Haskell/blob/master/1/1.3.md)
