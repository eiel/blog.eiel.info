---

title: "scalaz に入門する"
date: 2013-08-23T23:06:00+09:00
comments: true
tags: ["scala","scalaz"]
---

Scalaz をおすすめされた。

<blockquote class="twitter-tweet"><p>ひむひむはScalazでいいと思うの</p>&mdash; 航空母艦 (@razon) <a href="https://twitter.com/razon/statuses/370906437104324608">August 23, 2013</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

とりあえず、入門してみよう。

### scalaz って何

Scala がより関数型言語の力を得るらしい。よくわからん。
モナドとか使える。

[https://github.com/scalaz/scalaz](https://github.com/scalaz/scalaz)

### sbt を入れる

sbt ってのを使っていれば…、て記述がたくさんある。
ruby でいうと bundler に近いものなのでしょうか。ちょっと違うけど。
makefile みたいなもんだろうか。これもちょっと違うよーな。

インストールですが、Gentoo Prefix で入れたい。
Gentoo なら Overlay がある。

[https://github.com/whiter4bbit/overlays](https://github.com/whiter4bbit/overlays])

layman します。

```
layman -f --overlays https://github.com/whiter4bbit/overlays/raw/master/layman.xml --add gentoo-scala-tools
```

設定できたら、

```
emerge sbt-bin
```

でいけた。

### sbt をつかって scalaz

プロジェクトフォルダを用意して build.sbt を作成する。

[Hello, World - はじめる sbt](http://scalajp.github.io/sbt-getting-started-guide-ja/hello/) を参考にした。

```
name := "scalaz-helloworld"

version := "1.0"

scalaVersion := "2.9.2"

libraryDependencies += "org.scalaz" %% "scalaz-core" % "7.0.3"
```

name は プロジェクトの名前。 version はプロジェクトの成果物のバージョン番号。
scalaVersion は利用する scala のバージョン。
最後のは scalaz のための設定。

ここまできたら scala の対話環境を起動できるみたい

```
sbt
console
```

### scalaz 遊んでみる。

どこから手をつけたらいいのかさっぱりわからない。
なにをすればいいかわからない。

最近 Haskell してないので、なにをしたらいいのかも浮かばない。
仕方ないので Listモナド で遊ぶことにした。


```
scala> import scalaz._
scala> import std.list._

sacala> List(1,2,3) >> List(1,2,3)
res0 List[Int] = List(1, 2, 3, 1, 2, 3, 1, 2, 3)
```

Haskell でかくと以下と等価。

```haskell
[1..3] >> [1..3] -- => [1,2,3,1,2,3,1,2,3] :: [Intger]
```


### まとめ

とりあえずインストールして、Hello Worlrd 的なことができた。

Scala の文法がそもそもわからなので暗号だらけである。
やっぱり先に基本文法を抑えないとどうしようもないような気がします。
