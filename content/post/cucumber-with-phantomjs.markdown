---

title: "cucumber で PhantomJS を使う"
date: 2013-05-23T01:59:00+09:00
comments: true
categories: [cucumber,phantomjs,poltergeist,ci]
---

[Cucumber](http://cukes.info/) で使うブラウザを [PhantomJS](http://phantomjs.org/) にしたい。

Cucumber -> Capybara -> Poltergeist -> PhantomJS という感じに利用します。

PhantomJS は画面のないブラウザと言うと、伝わりやすいでしょうか。

統合的なテストを行う場合、Rails プロジェクトでは Cucumber がよく使われています。
Cucumberのシナリオに `@javascript` というタグをつけると Selenium を利用して Firefox を制御してテストを行うことができます。
非常に便利なのですが、処理が長かったり、また、X11の起動してない Linux などで動かそうとするとちょっと問題がおきます。
そこで、画面の表示をしないブラウザでテストしたくなります。
また、実際によく使うわれるのはレンダリングエンジンは Webkit です。


そのためのブラウザとしての有力候補が PhantomJS です。
PhantomJS のレンダリングエンジンは Webkit で、必要であればスクリーンショットがとれます。
Travis CI でも利用できるようです。(未確認)

利用までの手順としては

* PhantomJS のインストール
* Rails プロジェクトに Poltergeistを追加
* featrue/support/env.rb を設定

となります。

### PhantomJS のインストール

[http://phantomjs.org/download.html](http://phantomjs.org/download.html) から ダウンロードできます。
Mac であれば Homebrew や Macport でインストール可能なようです。ダウンロードしても bin/phantomjs を 環境変数PATH に入っているところに配置するだけです。

### Rails プロジェクトに Poltergeist を追加

Gemfile に

```ruby
group :test do
  gem 'poltergeist'
end
```

と追記すれば良いです。

### feature/support/env.rb を設定

設定しないと使えません。
`feature/support/env.rb` に以下を追記すればよいでしょう。

```ruby
require 'capybara/poltergeist'
Capybara.javascript_driver = :poltergeist
```

`@javascript` つけるのがめんどくさい! って場合は、

```ruby
equire 'capybara/poltergeist'
Before do
  Capybara.current_driver = :poltergeist
end
```

とする方法もあります。

### トラブルとか

* javascript がちょっとエラーがおきただけで、エラーになる

`js_errors` などを設定すると無視できるようです。
今のところは折角なので全部直しています。

* クリックに失敗することがある。

なにやらふたつの node をクリックしてしまって、エラーのようなものが起きてる箇所がでてます。

```ruby
もし /^"(.*?)"をクリック$/ do |name|
  click_on name
end
```

のような、step を

```
もし /^"(.*?)"をクリック$/ do |name|
  find(:link_or_button, name).trigger('click')
end
```

にすると動く場面もありました。

### もうちょっと詳しく

失敗する利用がわからなくて、pry などで停止させた時に Poltergeist を直接やりとりしたい場合は`page.driver` でオブジェクトにアクセスできます。
PhantomJS と直接やりとりしたい場合は `page.driver.server` でよさそうです。

* [Capybara::Poltergeist::Driver](https://github.com/jonleighton/poltergeist/blob/master/lib/capybara/poltergeist/driver.rb)
* [Capybara::Poltergeist::Server](https://github.com/jonleighton/poltergeist/blob/master/lib/capybara/poltergeist/server.rb)

### 一応登場人物の整理

Capybara はウェブブラウザの違いを吸収してブラウザの操作を記述するDSLです。バックエンドに何を使うとしても同じように書けます。

Poltergeist は CapybaraのDSLによる命令を、PhantomJS の命令に変換し、実行結果をもどす役目をします。違いを吸する部分です。


### まとめ

まだテストがすべて通ってない。JavaScript を使用していない シナリオからの移行はそれなりに大変です。
でも、やるなら速いほうがいいと思います。
