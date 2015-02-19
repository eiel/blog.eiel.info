---

title: "Rubyの拡張ライブラリをデバッグしてみた。"
date: 2013-01-31T16:26:00+09:00
comments: true
categories: ["ruby"]
---

結論から言うと解決していないんだけど、学んだことをメモしておきます。


## あらまし

* ruby-2.0.0-rc1 で [debugger](https://github.com/cldwalker/debugger) を動かしたかった。

ruby-2.0.0-rc1 だとコンパイルすらできない!

結果: コンパイルはできるようになったしとりあえず、うごくけどすぐ落ちる。テスト通らない。

https://github.com/eiel/debugger/tree/ruby-2.0/doc


## 拡張モジュールをコンパイルする方法

手順としては `rake compile` するだけでした。

手動でやりたい場合は
* Makefileを生成する
* make する

Makefileするには `ruby extconf.rb` とします。カレントディレクトリに Makefile ができるので make します。

拡張モジュールは`ext`ディレクトリにソースコードがあり、`rake compile` すると `lib`に共有ライブラリ(.so, .bundle, .dll)ができます。


## gem にして installする方法

`gem build *.gemspec` して gemを作ってもいいけど、だいたい `rake gem` で作成できます。
`pkg/` にファイルが生成されるので、`gem intall pkg/*.gem` でインストールします。


## やったこととか

make してエラーが出る部分を rubyのソースコードで `git log -S` して変更内容を確認してひたすら直す。
APIとか拡張されてると思いますが、その辺はわからないので無理。


## gdb を使ってデバッグする方法

`ruby hoge.rb`などでセグメンテーション違反などで落ちる場合は以下の方法でデバッグできます。

```
$ gdb rubyのバイナリを指定[~/.rbenv/versions/ruby-2.0.0-rc1/bin/ruby
gdb> run hoge.rb
```

書いてて気づいたんだけど、lldb でデバッグするべきだった?

## おまけ

ruby-2.0.0からだ clang を使うようになってるみたいで、エラーがすごくわかりやすかった。
