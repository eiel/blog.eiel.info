---

title: "Jekyll を使ったGithub Pages で関数呼び出し的なことをする"
date: 2013-06-19T21:32:00+09:00
comments: true
categories: ["github","jekyll","liquid"]
---

この記事は [すごい広島 #005](http://great-h.github.io/events/event-005.html) で試したことです。

[Github Pages](http://pages.github.com/) で Jekyll を使う場合は機能拡張などすることが基本的にできません。

関数のように汎用のHTMLを作成して、引数で動作を変えるようなことがしたい。
本来ではあれば [Liquid](http://liquidmarkup.org/) のカスタムタグなどが使えるのですが、[jekyll](http://jekyllrb.com/) が safe モードで動いているので、カスタムタグを作成することができません。

しかし、 liquid の `includeタグ` を利用することでそれっぽいことができます。

流れは

* あらかじめ変数をセットしておく
* include を使う
  * セットしておいた変数で分岐したり、表示内容として利用する

となります。

# 変数

変数をセットするには FrontFormatter を利用するか、liquidの `assignタグ` か `caputerタグ`を利用することになります。

## FrontFormatterを使う

FrontFormatter は ページの先頭に書く yaml の部分です。

例:

```
title:  "すごい広島 #5"
date:   2013-06-19 19:00:00
place: tullys
```

`page.place` という変数を追加して `tullys` という文字列をセットできます。

## assign を使う

Liquid の assign タグを利用して

```
{ % assign place = tullys % }
```

`place` という変数を追加して `tullys` という文字列をセットできます。
FrontFormatter には page のメンバになっていましたが、こちらは直接参照できます。

## capture を使う

capture を用いると長い文字列を変数にセットするのに便利です。

```
{ % capture place % }
tullys
{ % endcapture % }
```

これも同様に place という変数を追加して `\ntullys\n` という文字列をセットできます。(改行を含みます)


# include タグ

これは SSI の include のように外部のファイルを読込みして、その場に挿入できる liquid のタグです。
同様の機能が PHP なんかにもありますね。

include するためのファイルは `_includes` におくことになります。
`PROJECT_ROOT/_includes/place/go`というファイルを作成して、中身を作成します。

```
<b>ある場所に行きます。</b>
```

これをあるページで

```
{ % include place/go % }
{ % include place/go % }
```

と記述すれば

```
<b>ある場所に行きます。</b>
<b>ある場所に行きます。</b>
```

と、出力されます。

# 変数と include を組み合わせる

includeを無理矢理関数のように利用してみます。

上記の `PROJECT_ROOT/_includes/go` を以下のようにします。

```
<b>{{ place }}に行きます。</b>
```

そして、assign で値をセットしてから include します。

```
{ % assign place = "広島" % }
{ % include place/go % }
{ % include place/go % }
{ % assign place = "日本" % }
{ % include place/go % }
{ % include place/go % }
```

結果は以下のようになります。

```

<b>広島に行きます。</b>
<b>広島に行きます。</b>

<b>日本に行きます。</b>
<b>日本に行きます。</b>
```

こうすることで繰返し項目を少しだけ DRY に記述できます。

# 注意点

レイアウトで include する場合、ページから変数を設定するには、FrontFormmater を使用しないとうまくいきません。

レイアウト -> ページ内部 という順番で処理されるためです。

* includeする
* 変数を設定する

という動作になるため、期待した動作になりません。

また、 include の引数に変数を使う方法はみつけることができませんでした。
代わりに `caseタグ` や `ifタグ` で地道にがんばることになります。

Github  Pages を共同編集するわけではないなら、ローカルでHTMLを生成してから push するほうがいろいろ便利そうです。
プログラマ的には slim なども利用できる [Middleman](http://middlemanapp.com/) などが注目を浴びていきそうですね。

# 参考文献

* [Liquid for Designers](https://github.com/Shopify/liquid/wiki/Liquid-for-Designers)

# 関連記事

* [Github Pages について整理しておきます](http://blog.eiel.info/blog/2013/02/17/github-pages/)
* [Github で Jekyll を使う時に調べたこと](http://blog.eiel.info/blog/2013/02/18/jekyll-on-github/)
