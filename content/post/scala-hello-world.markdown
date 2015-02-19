---

title: "Scala 入門しとく"
date: 2013-08-17T13:48:00+09:00
comments: true
categories: ["Scala"]
---

[Scala](http://www.scala-lang.org/) はじめるはじめる詐欺をしていたので、はじめることにしました。

### インストール

Gentoo Prefix で

```
emerge dev-lang/scala
```

と、しましたが、ビルドに失敗しました。
ドキュメントまわりでエラーのようなのでちょっと工夫すればビルドできそうですが、諦めてバイナリインストールを試みました。

`$EPREFIX/etc/partoge/pacakeg.use` に以下を加えました。

```
=dev-lang/scala-2.9.2 binary
```

### はじめてみる

[Getting Started - Scala](http://www.scala-lang.org/documentation/getting-started.html) を参考にはじめてみました。
基本のハローワールドです。ようこそ Scala の世界へ。

`scala` コマンドで対話環境が起動できます。

```
$ scala
```

そこに以下のコードを打ち込んでみました。

```scala
object HelloWorld {
  def main(args: Array[String]) {
    println("Hello, world!")
  }
}
```

僕の Ruby 的な感覚では HelloWorld というオブジェクトを作り特異メソッド main を定義している感じに見えます。
このあたりは、Java のエントリポイントが static な main メソッドになるのを知っていれば、納得感はあります。

対話環境には

```
defined module HelloWorld
```

と表示されているのが気になります。
「オブジェクトは別にメソッドが集めるところが用意されてるのかな？」という印象を受けました。

その後、追加で、

```scala
HelloWorld.main(null)
```

と入力しますと、`Hello, World` と出力されました。

Haskell を学習してる感覚からいくと 第1引数は null より空のリストを渡したいですね。
「null が渡せて気持ち悪い。」的な感じがしました。
オブジェクトは null をはじめから許容していると考えれば、大丈夫そうです。
リストではなく配列という種類のオブジェクトなのでしょう。

終了するには `:q` を入力するようです。 `C-d` も終了できました。

### コンパイルしてみる

さきほどは、対話環境で入力して確認してみましたが、次はコンパイルしてみるようです。
プログラムのソースコードの拡張子は `scala` を使うようです。
`scalac` コマンドでコンパイルできるそうです。

なので、`HelloWorld.scala` というファイルを作成してさきほどのソースコードを貼りつけて、

```
scalac HelloWorld.scala
```

としました。

作成されたファイルは

* HelloWorld$.class
* HelloWorld.class

でした。

実行には

```
scala HelloWorld
```

とするようです。

試しに、

```
java HelloWorld
```

とすると `scala/ScalaQbject がみつからない` というエラーになりました。
上手いこと 指定してやれば実行できるそうですね。
そもそも `scala` コマンドはシェルスクリプトで java を上手いこと起動してる感じのようでした。

あと `Hello.scala` でも問題なくコンパイルできるようです。
こういうことをしていると余計なトラブルに巻き込まれる感がありそうです。
ついでに、`sacala Hello.scala` としても実行できました。しかし、遅い。

### まとめ

Scala で遊ぶ準備ができました。
次はエディタ環境を整えたいです。

sacala と typo しやすい。
対策を練りたい。

もうちょっと遊びたい。次は何を読みながら試すのがよいのだろうか。

### 追記

謎の暗号が飛んできた。

<blockquote class="twitter-tweet"><p>App traitやでひむひむ</p>&mdash; 航空母艦 (@razon) <a href="https://twitter.com/razon/statuses/368746540321361922">August 17, 2013</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>


```scala
object HelloWorld extends App {
  println("Hello, world!")
}
HelloWorld.main(null)
```

とかいても良いらしい。

Ruby 脳で考えると、クラスコンテキストに書いた部分がまるまる main メソッドの中にはいっている感。
どういう仕組みになっているのか調べるのには何を見ればいいのだろうか。

<blockquote class="twitter-tweet"><p><a href="http://t.co/bqr5OaJoIt">http://t.co/bqr5OaJoIt</a></p>&mdash; 航空母艦 (@razon) <a href="https://twitter.com/razon/statuses/368747744162086912">August 17, 2013</a></blockquote>

的確なヒントが飛んでくる。

[http://www.scala-lang.org/api/current/index.html#scala.App](http://www.scala-lang.org/api/current/index.html#scala.App)

[https://github.com/scala/scala/blob/v2.10.2/src/library/scala/App.scala#L59-L61](https://github.com/scala/scala/blob/v2.10.2/src/library/scala/App.scala#L59-L61)
[https://github.com/scala/scala/blob/v2.10.2/src/library/scala/App.scala#L71](https://github.com/scala/scala/blob/v2.10.2/src/library/scala/App.scala#L71)

あたりが怪しい。

```scala
  override def delayedInit(body: => Unit) {
    initCode += (() => body)
  }

  def main(args: Array[String]) = {
    this._args = args
    for (proc <- initCode) proc()
    if (util.Properties.propIsSet("scala.time")) {
      val total = currentTime - executionStart
      Console.println("[total " + total + "ms]")
    }
```

あらかじめ main メソッドを定義しておいて、処理を追加できる感じがします。

delayedInit がどうやって hook されるかは DelayedInit を追えばなにか分かる臭がしますね。

引数は args で取れそう。

```scala
object HelloWorld extends App {
  println(args(0))
}
HelloWorld.main(Array("hoge"))
```

まだまだ、わからないことがたくさんあるけど、魔法みたいなことができることを知る。
