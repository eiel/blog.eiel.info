---

title: "Lispインタープリタ勉強会に参加したので - LT駆動開発09"
date: 2014-12-08T15:49:00+09:00
comments: true
tags: ["ltdd","lisp"]
---

[LT駆動開発09](https://github.com/LTDD/Sessions/wiki/LT%E9%A7%86%E5%8B%95%E9%96%8B%E7%99%BA09)でLTをした。

[Lispインタープリタ勉強会](http://great-h.doorkeeper.jp/events/16621)に参加したのでLispを実装しなければいけない気がしたので、勉強したことを参考に作成しはじめてみた。
インタプリタと書くか、インタープリターと書くか、インタープリタと書くかブレまくっていたのは秘密である。

<script async class="speakerdeck-embed" data-id="22a8e5b060470132ec2f1eb00375a1f0" data-ratio="1.33333333333333" src="//speakerdeck.com/assets/embed.js"></script>

書いたコードはGistにあるんだ。

* [Gist - Lisp インタープリタ勉強で学んだことを試した。](https://gist.github.com/eiel/af120e6f44febc875702)

勉強会でlesson0として、用意してあったJavaScriptのコードを参考にした。
これは四則演算がすることができて、
JavaScriptの配列リテラルを使ってLISPのコードをかく。
この配列を評価できる評価機をつくる。

配列リテラルを評価したものが、それ自身がASTなので割と簡単にLISPの評価機が作れる。
ASTなんだけどLispのプログラムのように見えるからLispをつくった気分になる。
そのスタート部分を作成しただけである。
といいつつも、和しか実装していない。

しかし、Haskellのリストは同じ型の値しかリストには入れなれないので実際リストで書いてみると、値をつくる必要があり、LISP感は下がる。やはりパーサを書かねばならないのである。

やることはそんなに難しくないし、手を動かしてみると学ぶことはたくさんある。
勉強会に参加したかどうかはおいておいて、自分だけのLISPを作るべきである。

HaskellでLISPを作るなら良さ気な文献があるのでそちらをやるのもよい。

* [48時間でSchemeを書こう - Wikibooks](http://ja.wikibooks.org/wiki/48%E6%99%82%E9%96%93%E3%81%A7Scheme%E3%82%92%E6%9B%B8%E3%81%93%E3%81%86)
