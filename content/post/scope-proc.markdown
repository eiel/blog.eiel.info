---

title: "ActiveRecord のスコープを書くときは proc を使えよって話"
date: 2014-01-09T16:42:00+09:00
comments: true
tags: ["rails"]
---

ActiveRecord で scope を定義する時は、普通は以下のように書きますよね。

```ruby
scope :hoge, -> { where(name: 'hoge' }
```

歴史的背景で、

```ruby
scope :hoge, where(hname: 'hoge')
```

とか、書いてることがあるかもしれません。

この場合、一応、問題なく動きますが、コンテキストに応じて値が変化するようなものを利用してる場合には問題が起きることがあるので、早めに修正したほうが良いかもしれません。(というか問題が起きた)

前者で書きましょう。


### 具体例

特に日付などを利用してると問題になりやすいかもしれません。
例えば、

```ruby
scope :this_month, where(month: Date.today.month)
```

というのは、月を跨ぐと問題になる可能性が高いです。
常にデプロイした月の結果を返すことになります。

クラスをファイルを読み込みした際に Date.today.month が評価されてしまうので、そこの値が固定されてしまいます。

```ruby
scope :this_month, -> { where(month: Date.today.month) }
```

という風に書き換えておいたほうが良いです。

年とか使ってると気づかない可能性高そうですね。怖い怖い。
