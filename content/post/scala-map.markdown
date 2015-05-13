---
date: 2015-05-13T19:44:21+09:00
title: ScalaのMapの使い方がよくわからないので遊んだときのメモ
tags: ["sacala"]
---

[すごい広島 #104](http://great-h.github.io/events/event-104.html)で遊んだときのメモ。

最近[Scala](http://www.scala-lang.org/)勉強している。

[Map](http://www.scala-lang.org/api/2.11.5/index.html#scala.collection.Map)を使いなれていなかったので少し遊んだ。


参考になりそうなのは下記のサイト

* [Scala Standard Library 2.11.5](http://www.scala-lang.org/api/2.11.5/index.html#scala.collection.Map)
* [Scala Mapメモ(Hishidama's Scala Map Memo)](http://www.ne.jp/asahi/hishidama/home/tech/scala/collection/map.html)


### さて遊ぶ

Mapを作ってみる。emptyでも作れるけど。

```scala
Map("hoge" -> 1)
// scala.collection.immutable.Map[String,Int] = Map(hoge -> 1)
```

値を取り出してみる。

```scala
Map("hoge" -> 1).get("hoge")
// Option[Int] = Some(1)
```

ImmutableなMapに要素をつけたしてみる。新しいMapができる。

```scala
Map("hoge" -> 1) + ("mogu" -> 2)
// scala.collection.immutable.Map[String,Int] = Map(hoge -> 1, mogu -> 2)
```

+でくっつけることができた。`("mogu" -> 2)`ってなんだろう。

```scala
("mogu" -> 2)
// (String, Int) = (mogu,2)
```

どうやらタプルらしい。

```scala
Map(("hoge", 1))
// scala.collection.immutable.Map[String,Int] = Map(hoge -> 1)
```

タップルをつかってつくるには括弧がふたついるようだ。なぜなのかよくわからない。

とりあえず、`(+) :: Map[String,Int] -> (String,Int) -> Map[String,Int]` みたいな感じのようだ。

```scala
Map("hoge" -> 1) + (("mogu", 2))
// scala.collection.immutable.Map[String,Int] = Map(hoge -> 1, mogu -> 2)
```

うまくいく。

```scala
Map("hoge" -> 1) + ("hoge" -> 2)
// scala.collection.immutable.Map[String,Int] = Map(hoge -> 2)
```

-> は演算子で , は違うようだ。他のタプルの生成方法を今度調べよう。

```scala
"hoge".->(1)
// (String, Int) = (hoge,1)
// "hoge".,(1) はできなかった
```

foldで処理できそうな程度に理解した気がする。

### もうちょっと遊ぶ

inheritedにFunction1がいるのでapplyできるようだ。

```scala
Map("hoge" -> 1)("hoge")
// Int = 1
```

関数合成だってできる。

```scala
val one = Map("hoge" -> 1).compose({x:Any => "hoge"})
// Int = 1
one(1)
// Int = 1
one(2)
// Int = 1
one("hoge")
// Int = 1
```

とても楽しめた。

### 関連

* [scala-quiz/01_WordCount - GitHub](https://github.com/chatwork/scala-quiz/blob/master/quiz/01_WordCount.md)
