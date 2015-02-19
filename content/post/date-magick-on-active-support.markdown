---

title: "Active Supportの日付演算ってなかなか不思議。"
date: 2013-02-07T12:40:00+09:00
comments: true
tags: ["ruby","active_support"]
---

昨日の記事がかなり反響がありまして、みなさまありがとうございます。
関連のある記事を書きたくなりますが、とりあえず、変わらず淡々とメモも残していきたいと思います。ゆるりとGithub入門記事も書きたいです。


ActiveSupportが拡張する日付操作はとても便利です。よく使います。でも、ちょっと黒魔術だなぁって思ったことがあったので紹介します。

Rubyでは日付や時刻クラスのインスタンスと数値が演算できます。


今から1ヶ月後の日付が知りたいのであれば、以下のように書けます。

```ruby
1.month.since                   # => 2013-03-07 12:51:18 +0900
1.months.since                  # => 2013-03-07 12:51:18 +0900
```

複数形でも単数形でも。


特定の日付からでも同様のことがしたい場合は以下のようになります。
```ruby
DateTime.new(2013).months_since 1   # => Fri, 01 Feb 2013 00:00:00 +0000
DateTime.new(2013) + 1.month        # => Fri, 01 Feb 2013 00:00:00 +0000
```

有名な機能なので、ご存知の方も多いと思います。

別に、一日単位なら `month` メソッドとか使わなくてもできます。
```ruby
DateTime.new(2013) + 1              # => Wed, 02 Jan 2013 00:00:00 +0000
DateTime.new(2013) + 1.day          # => Wed, 02 Jan 2013 00:00:00 +0000
```

さて、本題。

`month`だけでなく`hour`や`day`,`second`などもありますが、戻り値の型はすべて`Fixnum`になっています。
`1` などの数値も`Fixnum`です。

Rubyで日付や時刻を表わすクラスは `DateTime`, `Date`, `Time` などありますが、演算をした場合は、**レシーバによって変化します。**

でも、`month`や`second` メソッドを利用してから演算すると引数によって動作が変化します。**どれも足すのは`Fixnum`なのに。**

というわけで、サンプルコード。

```ruby
require 'active_support/all'

datetime = DateTime.new 2013, 2, 7
date     = Date.new     2013, 2, 7
time     = Time.new     2013, 2, 7

datetime                        # => Thu, 07 Feb 2013 00:00:00 +0000
date                            # => Thu, 07 Feb 2013
time                            # => 2013-02-07 00:00:00 +0900

# レシーバによって動作が変わる          (1)
# 1日先に
datetime + 1                    # => Fri, 08 Feb 2013 00:00:00 +0000
# 1日先に
date     + 1                    # => Fri, 08 Feb 2013
# 1秒先に
time     + 1                    # => 2013-02-07 00:00:01 +0900

# これを防ぐには和をとるものを明示する (2)
datetime + 1.days               # => Fri, 08 Feb 2013 00:00:00 +0000
date     + 1.days               # => Fri, 08 Feb 2013
time     + 1.days               # => 2013-02-08 00:00:00 +0900

# 秒の場合                                (3)
datetime + 1.second             # => Thu, 07 Feb 2013 00:00:01 +0000
date     + 1.second             # => 2013-02-07 00:00:01 +0900
time     + 1.second             # => 2013-02-07 00:00:01 +0900

# Class は どれも Fixnum なのです
1                               # => 1
1.class                         # => Fixnum
1.days                          # => 1 day
1.days.class                    # => Fixnum
1.second                        # => 1 second
1.second.class                  # => Fixnum

# (1) の場合のみレシーバによって動作が変化。動作的には自然だと思う。
# (2), (3) の場合は 引数に応じた動作に。 Dateは演算の結果、型が変化する。
# 使う分には使いやすい。
```

いいたいことはソースコードにもかいた!
なかなか、黒魔術。


## 以下、雑談

なんでこんなことが気になったかというと、

```ruby
n = 100000                  # n にはなんらかの秒数がはいってると仮定
date = DateTime.new         # date には日付っぽいなにかが入る
date.to_date + (n.to_f / 1.days).to_i      # <- 何がやりたかったんだろう
```
的なコードをみつけたからです。日付単位で演算したかったのだと思いますが、カオスです。

表示するための文字列を作る前準備だったので、以下で良い気がします。
もちろん状況によりますけども。
```ruby
n = 100000                  # n にはなんらかの秒数がはいってると仮定
date = DateTime.new         # date には日付っぽいなにかが入る。
date + n.second
```

## せっかくなので

せっかくなので Github と絡めておこう。

サンプルコードを[Gist](https://gist.github.com/)にも置いてみました。

[https://gist.github.com/eiel/4728435](https://gist.github.com/eiel/4728435)

もしかすると、Githubでフォローしてくださった方には News Feedに 私が Gist に投稿したのが流れているかもしれません。

<small><del>だって、仕様ってコロコロ変わるし、サブアカでも作らないと自分で確認できないんだもの…</del></small>


## もうちょっと掘り下げてみる

`months`メソッドなんかは

* [https://github.com/rails/rails/blob/master/activesupport/lib/active_support/core_ext/numeric/time.rb](https://github.com/rails/rails/blob/master/activesupport/lib/active_support/core_ext/numeric/time.rb)
* [https://github.com/rails/rails/blob/master/activesupport/lib/active_support/core_ext/integer/time.rb](https://github.com/rails/rails/blob/master/activesupport/lib/active_support/core_ext/integer/time.rb)

に定義されてます。


`Fixnum` とか自称しながら、実体は `ActiveSupport::Duration` でした。裏切られた気分だ。
