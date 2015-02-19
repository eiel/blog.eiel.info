---

title: "読書メモ 関数プログラミング入門 Haskellで学ぶ原理と技法 1.1 セッションとスクリプト"
date: 2013-01-13T18:46:00+09:00
comments: true
categories: ["Haskell","読書メモ","関数プログラミング入門"]
---

[関数プログラミング入門 Haskell で学ぶ原理と技法](http://www.amazon.co.jp/gp/product/427406896X/ref=as_li_ss_tl?ie=UTF8&camp=247&creative=7399&creativeASIN=427406896X&linkCode=as2&tag=eiel-22) の読書メモです。

本書は 2002年に出版された[Introduction to Functional Programming using Haskell](http://www.amazon.co.jp/gp/product/0134843460/ref=as_li_ss_tl?ie=UTF8&camp=247&creative=7399&creativeASIN=0134843460&linkCode=as2&tag=eiel-22) の2版の翻訳です。
いまになって日本語に訳されたということはそれなりの名著なのかなー。ということで、Haskellネタを書く機会があまりなかったので、読書メモを書いていこうと思います。


問題を解いてくのに最強の環境をつくろうぜ。と、意訳できる文章ではじまります。本節は Hugs を使用することを想定して セッション、スクリプトといった対話環境だからこそしっくりくる用語を中心に基本用語が解説されています。

スクリプトによって定義を追加していき環境を構築した上で **式を評価** するというのが主軸なのかなあ、と思います。環境/文脈は束縛の集りであるというのは非常にシンプルで実際の文脈の小ささは関数プログラミングの特徴と言えるのではないかな、と思いました。

あとは、定義には関数の定義がかかれ、関数には 型シグネチャ をかけることぐらいかな。

練習問題が関数定義の練習と束縛済みの関数を再利用するのが目的な感じでした。

## 関連

* [メモ用のリポジトリ 1章](https://github.com/eiel/Introduction-to-Functional-Programming-using-Haskell/blob/master/1/index.org)

* [1.1の練習問題](https://github.com/eiel/Introduction-to-Functional-Programming-using-Haskell/blob/master/1/1.1.hs)

<iframe src="http://rcm-jp.amazon.co.jp/e/cm?lt1=_blank&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=eiel-22&o=9&p=8&l=as4&m=amazon&f=ifr&ref=ss_til&asins=427406896X" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>
