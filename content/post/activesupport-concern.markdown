---

title: "ActiveSupport::Concern - Railsのソースとか読みはじめた"
date: 2012-11-14T11:59:00+09:00
comments: true
categories: ["rails", "ActiveSupport"]
---

Railsのソースをちょろちょろ読むようにしている。読んで学んだことをメモしておきたい。だいたい読んだ当時の最新リリースを参考にします。

[ActiveSupport::Concern](https://github.com/rails/rails/blob/v3.2.9/activesupport/lib/active_support/concern.rb) を読みました。

このモジュールはモジュールの定義を手助けします。
クラスメソッドの定義場所をルール決めして `include` するだけで済むようにしたり、クラスのコンテキストで実行したい処理を書く場所を用意してくれます。

具体的にいきます。
以下のコードがあったとします。

```ruby
class ConcernSample

  attr_accessor :hoge

  def self.mogu
    "mogu"
  end

  def goro
    "goro"
  end
end
```

これを以下のようにするだけで同じ機能を提供できるようにしたいと思います。

```ruby
class ConcernSample
  include Sample
end
```

処理を Sample モジュールにまとめたいということです。
これができると

```ruby
class ConcernSample
  include Sample
end

class ConcernSample2
  include Sample
end

class ConcernSample3
  include Sample
end
```

と、似たような機能をもつクラスを量産できます。
クラスに機能を追加するのが簡単になるという視点を持つとよいでしょう。

さて、Sample はどのように書くかということです。
ここで `ActiveSupport::Concern` を利用します。

```ruby
require 'active_support'

module Sample

  extend ActiveSupport::Concern

  included do
    attr_accessor :hoge
  end

  module ClassMethods
    def mogu
      "mogu"
    end
  end

  def goro
    "goro"
  end
end
```

こんな感じになります。
クラスメソッドの定義の仕方を少し代えて横に並べてみると、非常に変化が少なくて済むのがわかると思います。

![参考](/images/concern.png)


`ActiveSupport::Concern` を利用せずに実装するとこんな感じになります。

```ruby
module Sample
  def self.included(base)
    base.extend ClassMethods
    base.class_exec do
      attr_accessor :hoge
    end
  end

  module ClassMethods
    def mogu
      "mogu"
    end
  end

  def goro
    "goro"
  end
end

```
`def self.included` がなくなり、モジュールの特異メソッドの利用がなくなるのと、`base.extend` が不要になるところが若干便利になります。よく rails のコード内で出てくるので知っておきたいですね。

[@NeXTSTEP2OSさん](https://twitter.com/NeXTSTEP2OSX) から質問があって

> サブクラスじゃダメなの？

と、質問されました。


親クラスにも重複するコードをまとめることができますが、**機能を追加する**という視点で考えた場合、module の場合はいくつも `include` することができます。継承を利用した場合は、親クラスはひとつしか持つことができないため不便です。また、継承は`is-a`の関係ではないところでは使うべきでないとされています。この `Concern` が利用されている部分は `is-a`の関係を持たない部分でコードの重複を避けるためやメソッドが多すぎるクラスでメソッドを分類するために利用されているようです。

利用例としてはたくさんのメソッドが定義されている [ActiveRecord::Base](https://github.com/rails/rails/blob/v3.2.9/activerecord/lib/active_record/base.rb#L685-715) クラスで `include` されてるモジュールなどがあります。
[ActiveRecrod::Persistence](https://github.com/rails/rails/blob/v3.2.9/activerecord/lib/active_record/persistence.rb#L6)などをみてみると利用されています。

`Concern`というクラスの名前の由来がよくわからない。

[つづき](/blog/2012/11/18/activesupport-concern2/)
