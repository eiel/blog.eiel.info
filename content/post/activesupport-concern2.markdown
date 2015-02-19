---

title: "ActiveSupport::Concern - Railsのソースとか読みはじめた 2"
date: 2012-11-18T23:42:00+09:00
comments: true
tags: ["rails","ruby","activesupport"]
---

[ActiveSupport::Concern - Railsのソースとか読みはじめた](/blog//2012/11/14/activesupport-concern/)の続きになるのですが、

@netwillnet さんに。

> ActiveSupport::Concernはモジュールが少しだけ書きやすくなるというメリットよりも、複数のモジュール同士に依存関係があったときにモジュール内でその依存関係をうまく解消させられるところに真価があるのでは
>
> [https://twitter.com/netwillnet/status/270150759335723008](https://twitter.com/netwillnet/status/270150759335723008)

と素敵な突っ込みを頂いたので、rdocとソースコードと睨めっこしてきました。

睨めっこした結果の結論を書きたいと思います。

というわけで

    A -> B -> C

という依存性があるモジュールを考えます。A には B が必要で。 B には C が必要という意味です。

```ruby
module C
  def c
    "c"
  end
end

module B
  include C

  def b
    "b" + c
  end
end

class A
  include B

  def a
    "a" + b
  end
end
```

`A.new.a` と実行すると `"abc"` と出力されます。
これと同じことをクラスメソッドで実現しようとしてみます。

```ruby
module C2
  def c
    "c"
  end
end

module B2
  def b
    "b" + c
  end
end

class A2
  extend C2 # ここにかきたくない
  extend B2

  class << self
    def a
      "a" + b
    end
  end
end
```

`extend C2` を `module B2` の中で書きたいのですが、 A2に書かなければ動作させることができません。(がんばればできるけど、がんばりたくない)

こういうときに `ActiveSupport::Concern` を利用すると下記のように書けました。

```ruby
module C3
  extend ActiveSupport::Concern

  module ClassMethods
    def c
      "c"
    end
  end
end

module B3
  extend ActiveSupport::Concern
  include C3 # ここにかける

  module ClassMethods
    def b
      "b" + c
    end
  end
end

class A3
  include B3

  class << self
    def a
      "a" + b
    end
  end
end

```

`include C3`を `module B3`の内側でかくことができました。

Concernという名前は依存性の悩みから解消されるということなんでしょうか？まだよくわからないです。
