---

title: "Ruby の とあるクラスの実装がどこにあるかわからない時の方法 - コードリーディング"
date: 2013-04-03T19:34:00+09:00
comments: true
categories: ["ruby", "gentoo", "emacs"]
---

[広島Ruby勉強会](http://hiroshimarb.github.com/blog/2013/04/06/hiroshimarb-31/) のネタ探しをしていて、「実装見てなにか面白いネタないかなー。」
って時に使ったコマンドを紹介。

例は Method クラスの実装を探す。

まずは git でソースコードをとってきている場合。
```
$ git grep 'rb_define_class("Method"'
proc.c:    rb_cMethod = rb_define_class("Method", rb_cObject);
```

`git grep` を使います。`git grep` はサクサクなのでメインの探索方法です。
Emacs上から使うとジャンプも楽チン。

「git リポジトリじゃねーよ」って時は `ag` を使います。

```
$ ag 'rb_define_class\("Method"'
proc.c
2331:    rb_cMethod = rb_define_class("Method", rb_cObject);
```

こちらは行番号とかでます。

`(` のエスケープがいります。

`ag` というのは [the silver searcher](http://geoff.greer.fm/2011/12/27/the-silver-searcher-better-than-ack/) のことです。こちらも emacs から使う [helm-ag](http://d.hatena.ne.jp/syohex/20130302/1362182193) というのがあるのでオススメします。

使える正規表現も違うような気がするので凝ったことがしたいならこちらを。

`rb_define_class` というのは Ruby の C API で クラスオブジェクトを作成します。このあたりに メソッドをクラスに紐づける処理なんかがあったりします。

それでは Happy な ソースコードリーディングを。
