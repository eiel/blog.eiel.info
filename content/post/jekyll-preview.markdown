---

title: "Github Page で公開する サイトを ローカルで preview するのに使ってる方法"
date: 2013-05-29T21:52:00+09:00
comments: true
categories: [jekyll, github]
---

[すごい広島 #2](http://great-h.github.io/events/event-002.html) でしたことを書きます。

[日記のほうはこちら](http://eielh-life.tumblr.com/post/51639356116/2)

2013年8月23日追記。下記の方法を改良したものがあります。

* [ローカルサーバ起動と同時にブラウザで開く。 - Jekyll とかで。](/blog/2013/08/28/browse-open-when-rake-preview/)

以上、追記終了。

私は、Jekyllを使用したサイトをプレビューする際に、jekyll のインターフェイスが変化しても、または、jekyll 以外のものを使用しているときのことも考えて、 `rake preview` でサイトのプレビューをできるようにしています。

「Octopressでも、Hakyll でも Jekyll でも `rake preview` にしたいんだ!!」

具体的には以下のような、Rakefile を作成しました。

```ruby
desc 'preview する。 http://localhost:4000/'
task :preview do
  sh 'bundle exec jekyll serve --watch'
end
```

Jekyll は v1.0.0 で preview するためのコマンド名が変わりました。

あとは、他の人が gem のインストールのを軽減するために、Gemfile も書きました。

```ruby
source 'https://rubygems.org'

gem 'jekyll',     '=1.0.2'
gem 'liquid',     '=2.5.0'
gem 'redcarpet',  '=2.2.2'
```

これで、ruby と bundler さえ入っている人は `bundle instnall` というコマンドを実行すれば、サイトのプレビューができるようになります。

bundler は `gem install bundler` でインストールしておきましょう。

[具体的なコミットはこちらに](https://github.com/great-h/great-h.github.io/pull/31)

### 関連

* [ローカルサーバ起動と同時にブラウザで開く。 - Jekyll とかで。](blog/2013/08/28/browse-open-when-rake-preview/)
