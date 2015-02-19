---

title: "github-pages Gem というのが用意された - Github Page で使う gem のバージョンをあわせてくれる"
date: 2013-08-13T00:54:00+09:00
comments: true
tags: ["github","jekyll"]
---

Github Page は Jekyll プロジェクトを push すれば HTML に変換してくれます。
これを使う場合、Github 側とローカルで確認するときの Gem のバージョンを揃えておきたいです。
そのために Gemfile を記述しますが、`github-pages` という gem が用意されました。
というわけで、試してみた。

```ruby
source 'https://rubygems.org'

gem 'github-pages'
```

`bundle install` とか実行せばあ必要な `gem` が手に入ります。
以前は、

```ruby
source 'https://rubygems.org'
ruby '1.9.3'

gem 'jekyll',     '=1.1.2'
gem 'liquid',     '=2.5.1'
gem 'redcarpet',  '=2.2.2'
gem 'maruku',     '=0.6.1'
gem 'rdiscount',  '=1.6.8'
gem 'RedCloth',   '=4.2.9'
gem 'kramdown',   '=1.0.2'
```

のように書く必要ありました。
とてもすっきりしています。
利用する gem が更新されても `bundle update` ですむので、とても嬉しいですね。

### もっと具体的に

依存している Gem の情報は gemspec にあります。
github-pages gem をインストールに必要な gem が記載されています。

* [github/pages-gem/github-pages.gemspec](https://github.com/github/pages-gem/blob/master/github-pages.gemspec#L17-L24)

この github-pages gem に依存関係を記述されているので、利用者は Gemfile に記述する必要がなくなりました。
Ruby 1.9.3 で使用することになっています。
2.0 で実行すると失敗するので注意しましょう。

### 関連

* [Github で Jekyll を使う時に調べたこと](/blog/2013/02/18/jekyll-on-github/)
* [Github Pages について整理しておきます](/blog/2013/02/17/github-pages/)
* [ローカルサーバ起動と同時にブラウザで開く。 - Jekyll とかで。](blog/2013/08/28/browse-open-when-rake-preview/)
