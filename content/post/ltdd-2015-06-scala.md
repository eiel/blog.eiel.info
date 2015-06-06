---
date: 2015-06-06T13:00:00+09:00
title: Scalaをはじめる α & β - #LT駆動 15
tags: ["scala","LT駆動"]
---

[LT駆動開発15](https://github.com/LTDD/Sessions/wiki/LT%E9%A7%86%E5%8B%95%E9%96%8B%E7%99%BA15)に参加してきました。

最近、Scalaを触っているので、仕事していると放置しがちな基本的な使い方を調べたりしています。

今回はα世界線とβ世界線の二本でお送りいたします。

* αはsbtを使わずにJARを使う方法を調べました
* βはAppトレイトについて調べました


なんのJARを使うのか非常に悩んだのですがScalazにしておきました。
Scalazを選ぶまでは早かったのですが、Scalazをつかった良いサンプルがつくれずに苦戦しました。

Appトレイトのほうは、[Scala関西 Summit 2015](http://summit.scala-kansai.org/)のLTに応募しようかと一瞬思ったのですが、まだ2ヶ月あるので止めておきました。

<script async class="speakerdeck-embed" data-id="6c59ed6f79234ddd96c99375a9947125" data-ratio="1.33333333333333" src="//speakerdeck.com/assets/embed.js"></script>

<script async class="speakerdeck-embed" data-id="15aa34c211ee43268a0767d0ef29d13d" data-ratio="1.33333333333333" src="//speakerdeck.com/assets/embed.js"></script>

まあ、一番苦戦したのは scala HogeZ -cp '*.jar:.' とかいて、動かなかったことである。

### まとめ1

JARを使うときは classpath を指定すればよい。
sbt使うと楽チン。

### まとめ2

```
object Hoge {
  //コード
  def hoge() = //コード
}
```

みたいなのはJavaでいうと

```
class Hoge {
  static {
   // コード
  }
  public staic void hoge() { /* コード */ }
}
```

みたいな感じになるらしい。

https://github.com/scala/scala/blob/540b18fb68a0210b187e595622c31f20b2c6f581/src/compiler/scala/tools/nsc/transform/Constructors.scala#L474

こういった特殊なものがあるならもっとしりたい


### 参考文献

* [はじめての関数型プログラミング教室 // Speaker Deck](https://speakerdeck.com/takahiro/hazimetefalseguan-shu-xing-puroguramingujiao-shi)
* [sbtで、プロジェクト内のライブラリ依存関係を調べる - CLOVER](http://d.hatena.ne.jp/Kazuhira/20130320/1363791795)
* [App - Scala Standard Library 2.11.6](http://www.scala-lang.org/api/current/index.html#scala.App)
* [DelayedInit - Scala Standard Library 2.11.6](http://www.scala-lang.org/api/current/index.html#scala.DelayedInit)
* [scala/DelayedInit.scala at v2.11.6 · scala/scala · GitHub](https://github.com/scala/scala/blob/v2.11.6/src/library/scala/DelayedInit.scala)
* [scala/App.scala at v2.11.6 · scala/scala · GitHub](https://github.com/scala/scala/blob/v2.11.6/src/library/scala/App.scala)

### コード

<script src="https://gist.github.com/eiel/77ffba73998ababb06b7.js"></script>

### 関連

* [ScalaのMapの使い方がよくわからないので遊んだときのメモ | そんなこと覚えてない](http://blog.eiel.info/blog/2015/05/13/scala-map/)
* [Scala 入門しとく | そんなこと覚えてない](http://blog.eiel.info/blog/2013/08/17/scala-hello-world/)
* [エンジニアの採用情報 | チャットワーク（ChatWork）](http://recruit.chatwork.com/ja/developer.html)
* [syrup16g - 冷たい掌 (MV) - YouTube](https://www.youtube.com/watch?v=gk02WpyN_Qo)
