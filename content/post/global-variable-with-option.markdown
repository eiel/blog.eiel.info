---

title: "Rubyのオプション変数というグローバル変数"
date: 2014-02-03T12:36:00+09:00
comments: true
categories: ["ruby"]
---

Rails のソースコードを読んでいたら

```ruby
before = $-w
$-w = false
require 'action_dispatch/journey/parser'
$-w = before
```

[https://github.com/rails/rails/blob/v4.0.2/actionpack/lib/action_dispatch/journey/router.rb#L6-L9](https://github.com/rails/rails/blob/v4.0.2/actionpack/lib/action_dispatch/journey/router.rb#L6-L9)

というのがあって `$-w` なんだろうと思って調べたら [オプション変数](http://docs.ruby-lang.org/ja/2.1.0/doc/spec=2fvariables.html#global)というグローバル変数があるそうな。

どうやら -w が指定されているかどうかの判断に利用できる模様。


つまり、このコードは

* `-w` がついていたかどうかを保存しておいて
* requireをして
* `-w` をもとの状態にもどす

ということをしているようです。

-w は verbose レベルを設定する機能ですが、 ActiveSupportに一時的に変更する機能があったような気がしつつも、調べてない。


### もう少し詳しく

こういうのは実際に試すのが大事だよね。

指定しない場合

```
$ ruby -e 'p $-w'
false
```

指定する場合

```
$ ruby -w -e 'p $-w'
true
```

ちなみに `-w` は `-W2` と等価らしいので、これも確認してみよう。

```
$ ruby -e 'p $-W'
1
```

```
$ ruby -w -e 'p $-W'
2
```
