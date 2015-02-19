---

title: "勢いでhiroshimarbというgemを作った。反省する気なんてあんまりない。"
date: 2012-09-02T00:20:00+09:00
comments: true
categories: [ruby]
---

# あらまし

広島Ruby勉強会で Hiroshima.rbでなにか gem を作りたいですよね。という話を前からちょくちょくしてたので、勢いで作成してみた。実際は反省している。

gemを公開するといっても、何か機能がないと寂しいので、

```
$ hiroshimarb open
```
とすることで、[Hiroshima.rbのウェブサイト](http://hiroshimarb.github.com/) を表示するようにしてみました。

インストール方法は
```
$ gem install hiroshimarb
```

[リポジトリはgithub](http://github.com/hiroshimarb/hiroshimarb-gem)にあります。

# gemの作成方法

せっかくなので gem の作成方法というか 本gemを作るにあたって作業内容を書いておきます。

## プログラムの作成

まずはプログラムをかくためにプロジェクトの雛形を作ります。
gemを作りやすい構成になっていると都合がよいです。
[Bundler](http://gembundler.com/)の機能を使うと良い感じの雛形がつくれます。

```
$ bundle gem hiroshimarb
```

そうするると `hiroshimarb` ディレクトリができますので、`README`や `hirosihmarb.gemspec` をかきかえます。gemspecの情報をもとにgemが作成されます。summaryやhomepage、 descriptionを書きかえたりしましょう。もちろん `hiroshimarb` の部分は自分の都合の良い名前にします。

あとは適当にプログラムを作成します。
binディレクトリにコマンドを作っておけばコマンドとしてインストールされます。

## ローカルでためす。

`*.gemspec`をもとにgem を作成するには
```
$ gem build hiroshimarb.gemspec
```
とします。そうすると `hiroshimarb-0.0.1.gem`のようなファイルが作成されます。
あとは
```
$ gem install ./hiroshimarb-*.gem
```
とすればインストールできます。

## rubygems.orgで公開する。

```
$ gem install hiroshimarb
```
で、インストール可能にするために [rubygems.org](http://rubygems.org)にgemを登録します。

まずは、sign upをしてアカウントを作成します。作成がおわったら
```
$ gem push ./hiroshimarb.*.gem
```
で送信することできます。
メールアドレスとパスワードを入力して終了です。

gem をつくるのは簡単です。ぜひぜひ挑戦してみましょう。


## 追記

[もっと楽ができるらしいです。](/blog/2012/09/02/hiroshimarb-gem/)
