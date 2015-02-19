---

title: "debugger-ruby_core_source に Ruby 1.9.3-p327 のヘッダ追加してみた"
date: 2012-11-10T16:58:00+09:00
comments: true
categories: [ruby]
---

2012/11/17 追記
```
1.1.5がリリースされて基本的に以下の作業は不要です。
以下の記事は興味がある方だけどうぞ。
```

昨日になりますが、 [Ruby 1.9.3-p327](http://www.ruby-lang.org/ja/news/2012/11/09/ruby-1-9-3-p327-is-released/) のリリースがありました。早速インストールして開発環境で試してみてます。
前回のリリースのときもそうだったのですが、rbenv と ruby-build を使用していると、 `debugger-linecache` のインストールにこけてしまいます。この子をインストールするには ruby の ヘッダが必要になります。(他の環境でもなるかもしれませんけども)
この gem は `debbuger` を利用している場合必要になります。

そのヘッダを提供する [debbuger-ruby_core_source](https://rubygems.org/gems/debugger-ruby_core_source) というgemがあるので、この子を git clone で取得してごにょごにょすればごまかせます。

前回はごにょごにょしたものがすでにあったので `git clone` して `gem build` `gem install` のコンボで済んだのですが、自分でやってみました。[Pull Request](https://github.com/cldwalker/debugger-ruby_core_source/pull/7) も出してるのでそのうち gem が更新されるのでそんなに気にする必要はないかもしれません。

### すぐにインストールしたい人向け

とにかく、動かしたい人で `debugger-ruby_core_source.gem` が欲しい人むけ

```
$ git@github.com:eiel/debugger-ruby_core_source.git
$ gem build debugger-ruby_core_source.gemspec
$ gem instll debugger-ruby_core_source-1.1.5.gem
```

あとは bundle install などやりなおしましょう。

### どうやって更新するか知りたい人向け

READMEみればわかるのですが簡単に更新できます。

```
$ gem install archive-tar-minitar
$ rake add_source VERSION=1.9.3-p327
```

add_source という rake task が用意されてるので簡単です。
この実行には `archive-tar-minitar` が必要になります。
bundler に対応されてないので直接いれました。
ネットワークみてみると対応されてるのがありましたが取り込まれてないようです。
あとは commit を作ればOKです。
最近の gem は gem を生成する際に git ls-tree の情報が使用されるので commit しないとハマります。

では happy programming !

おまけ

[crediteにアカウント名が残りました。ヤホーイ。](https://github.com/cldwalker/debugger-ruby_core_source/commit/f68d267844f8d385498a8a80c1590ba77141bd5a)
