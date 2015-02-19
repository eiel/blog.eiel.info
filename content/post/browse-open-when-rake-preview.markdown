---

title: "ローカルサーバ起動と同時にブラウザで開く。 - Jekyll とかで。"
date: 2013-08-28T20:01:00+09:00
comments: true
tags: ["jekyll"]
---


この記事は [すごい広島 #015](http://great-h.github.io/events/event-015.html) で書いております。

GitHub Pages などを使っていて、プッシュする前に ローカルサーバで確認すると思います。以前、[Github Page で公開する サイトを ローカルで Preview するのに使ってる方法](/blog/2013/05/29/jekyll-preview/) で、その方法を紹介しました。

```
$ rake preview
```

で、ローカルサーバを起動するようにしています。

今回は、この `rake preview` コマンドを実行した時に自動的にブラウザを起動して `http://localhost:4000/` へアクセスするようにしてみました。

`Rakefile` をこんな風に書きました。

```ruby
require 'bundler/setup'
require 'thread'
require 'launchy'

desc 'preview'
task :preview do
  Thread.new do
    sleep 1
    Launchy.open 'http://localhost:4000/'
  end

  sh 'bundle exec jekyll serve --watch'
end
```

別のスレッドで [Launchy](https://github.com/copiousfreetime/launchy) を使って起動しているだけです。

ちなみに `Gemfile` はこんな感じ。

```ruby
source 'https://rubygems.org'

gem 'github-pages'
gem 'launchy'
```

### 関連

* [Github Page で公開する サイトを ローカルで Preview するのに使ってる方法](/blog/2013/05/29/jekyll-preview/)
* [Github-pages Gem というのが用意された - Github Page で使う Gem のバージョンをあわせてくれる](/blog/2013/08/13/github-pages-gem/)
* [Pygements を利用してると Jekyll Serve --watch のファイル生成が遅いらしい](/blog/2013/08/30/jekyll-watch-very-slow/)
