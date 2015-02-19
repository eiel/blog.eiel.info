---

title: "内包表記について、すごい合同勉強会で話した"
date: 2014-11-02T12:25:00+09:00
comments: true
categories: ["内包表記","Haskell","Python"]
---

[すごい合同勉強会2014 in 広島](https://github.com/LTDD/Sessions/wiki/%E3%81%99%E3%81%94%E3%81%84%E5%90%88%E5%90%8C%E5%8B%89%E5%BC%B7%E4%BC%9A)でセッションしたので内容を公開しておく。

今回は「私がモナドの内包表記という名前を知った時の感覚を伝えよう」というのが目的でした。
さりげなく「私がモナドに感じている効能を伝える」というのもしているのですが、そこは本当にさりげなく。

<script async class="speakerdeck-embed" data-id="7cb24810446c0132e04e4e24d1028d6d" data-ratio="1.33333333333333" src="//speakerdeck.com/assets/embed.js"></script>

内包表記。その意味を知らずに5年前ぐらいにpythonで利用していて、forやif文字通りにうけとっており、その動作を正しく理解できてないときがありました。
現在とその間にHaskellを学び、その5年前の自分に内包表記を伝えるにはという観点で話を進めました。

まず、リストの内包表記ですが、リストを生成を簡単にしてくれる機能です。

内包表記は、どうやら数学の集合の記法である内包的記法に由来するそうで、「[関数プログラミング入門 ―Haskellで学ぶ原理と技法―](http://www.amazon.co.jp/gp/product/427406896X/ref=as_li_ss_tl?ie=UTF8&camp=247&creative=7399&creativeASIN=427406896X&linkCode=as2&tag=eiel-22)」か何かで読んだ記憶があります。

その対になる記法として外延的記法があります。
これは具体的な中身を列挙する方法で、普段のリテラル表記ともみなすことができます。
リテラルで地道にかくのではなく、てプログラミングで自動生成しようというのが内包表記と言えそうです。

Haskellの内包表記は ジェネレータとガードと呼ばれる真偽値を並べることで作成します。
`x <- [1..9]` の部分がジェネレータです。あと真偽値を返す式がガードになります。

* 参考 [3 Expressions - Haskell 2010](https://www.haskell.org/onlinereport/haskell2010/haskellch3.html#x8-420003.11)

pythonではジェネレータとガードが for と if で表現されています。
直感的だし、キーワードの使いまわしとも言えそうです。
(関係ないけど、C++はキーワードの使いまわしたいへんそうだなぁって思った)

あとはジェネレータを並べた際にどうなるか、というのがわかればリストの内包表記はうまく使えるのではないかと思います。
直積をとる。つまり、全部のパターンをつくる。
あとはフィルタで、致しているものを求めるだけですね。

そういえば、リストモナドでできることですね。複数答えがある場合にリストモナドを使うとすべての回答が得られます。

よく内包表記がmapやfilterと比較されることがありますが、そもそも同一に扱っても面白いことは特にない気がします。
目的しだいではmapやfilterを使うより便利だと考えてよいと思います。

蛇足ですが、モナドの有効性として、コードが斜めに述びる性質がある際に真っ直ぐに伸ばすことができるみたいなイメージを持っています。
それをさりげなく言っていたのですが、後で[はなださん](https://twitter.com/nobkz)のセッションで実例がでてきました。

さて、ここまでくると内包表記とSQLの類似性が簡単に説明できるし、具体例にしやすいので、SQLと絡めた話をしました。
あとはモナドの内包表記へと一般化する話です。具体例のリストから、Maybeへと繋ぎ一般化して終わりです。

Rubyの例でflattenしている部分がありますが、あの辺はリストモナドがいつも勝手にやってくれてるところで、さりげなく強調していたりしますね。

モナドはなんか怖いとか言われますが、それはさておいて内包表記は便利なので知っておいて損はないと思います。

会場はわりとポカーンとしていましたが「誰かの何かに役に立てばいいなぁ」ということでスライドと簡単な解説を残しておきます。

### 登場したコード

コピペしやすいように置いておきます。
主に対話環境用に。

Haskell

```haskell
[(x,y) | x <- [1..9], y <- [1..9], x * y == 24]
```

```haskell
[(x,y,z) | x <- [1..9], y <- [1..9], z <- [1..9], x * y * z == 24]
```

```Haskell
:set -XTransformListComp
[ (x,y) | x <- [1..9], y <- [1..9], x * y == 24, then take 2]
```

```haskell
:set -XTransformListComp
:m GHC.Exts
[ (x,y) | x <- [1..9], y <- [1..9], x * y == 24, then sortWith by y]
```

```haskell
:set -XMonadComprehensions
[ (x,y) | x <- Just 3, y <- Just 8, x * y == 24]
```

Python

```
[(x,y) for x in range(1,10) for y in range(1,10) if x * y == 24]
```

Ruby

```ruby
[*1..9].map do |x|
  [*1..9].map do |y|
    [x,y]
  end
end.flatten(1).select do |x,y|
  x * y == 24
end
```

```ruby
[*1..9].map do |x|
  [*1..9].map do |y|
    [*1..9].map do |z|
      [x,y,z]
    end
  end
end.flatten(2).select do |x,y,z|
  x * y * z == 24
end
```

```ruby
[*1..9].product([*1..9],[*1..9]).select do |x,y,z|
    x * y * z == 24
  end
```

SQL

```sql
SELECT x,y
FROM generate_series(1,9) AS X,
     generate_series(1,9) AS Y
WHERE x * y = 24;
```

```sql
SELECT x,y,z
FROM generate_series(1,9) AS X,
     generate_series(1,9) AS Y,
     generate_series(1,9) AS Z
WHERE x * y * z = 24;
```


### 参考文献

* [内包と外延 - Wikipedia](http://ja.wikipedia.org/wiki/%E5%86%85%E5%8C%85%E3%81%A8%E5%A4%96%E5%BB%B6)
* [内包的記法の出展 - 集合 - Wikipedia](http://ja.wikipedia.org/wiki/%E9%9B%86%E5%90%88)
* [5. データ構造 — Python 2.7ja1 documentation](http://docs.python.jp/2/tutorial/datastructures.html#id6)
* [7.3. 構文的拡張 - 内包表記の拡張](http://www.kotha.net/ghcguide_ja/7.6.2/syntax-extns.html#generalised-list-comprehensions)
* [7.3. 構文的拡張 Monadの内包表記](http://www.kotha.net/ghcguide_ja/7.6.2/syntax-extns.html#monad-comprehensions)
* [Haskell - ghciで言語拡張を有効にする - Qiita](http://qiita.com/uduki845/items/d60dc51ad3a26b9ab430)

### 関連リンク

* [すごい合同勉強会2014 in 広島を開催した - そんなこと覚えてない](blog/2014/11/02/great-study-2014/)
