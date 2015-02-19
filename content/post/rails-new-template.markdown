---

title: "rails new した時の追加処理をかく"
date: 2013-08-01T18:00:00+09:00
comments: true
tags: ["rails", "railtie"]
---

この記事はメモです。
[広島Ruby勉強会 #33](http://hiroshimarb.github.io/blog/2013/08/03/hiroshimarb-33/) で LT するのに下調べしたことを書いてるだけです。

`rails new` した時に `--template file_or_url` というオプションがあります。省略形は `-m`。
これを使うと、`rails new` した時に追加処理ができます。
「テンプレート機能」と呼びたいと思います。

何がしたいかというと rails new した時点で `pry` とか `rspec` とか `cucumber` とかいつも使うのを設定した状態にしたいのです。
ウェブサービスが思いついたら直ちに開発をしたいのです。

このテンプレート機能を使うものとしては [Rails Composer](https://github.com/RailsApps/rails-composer) というものがあります。
これを使うと、どのライブラリを利用するか質問されるので、回答していくと、雛形ができます。

これを自分でカスタマイズしたいので、いろいろ調べました。

役に立つかもしれないウェブページ

* [Rails Application Templates - Rails Guide](http://guides.rubyonrails.org/rails_application_templates.html)
* [Creating and Customizing Rails Generators & Templates - Rails Guide](http://guides.rubyonrails.org/generators.html)
* [RailsのApplication templateを使って開発の初速をあげよう！](http://qiita.com/tachiba@github/items/26b2e9dc271bd8e6907d)

ひとつめの「Rails Application Templates」は Rails Guide ですが、Rails Guide のトップにリンクがありません。
だいぶ調べた後に気づきました。泣きたい。

ふたつめも Rails Guide ですが、機能的には Generator と同じなので参考になります。

みっつめは日本語記事。面白くまとめてあります。このメモを書いてLTをつくった後にみつけた。

つづいて、関係してきそうなクラスやモジュールを整理します。

* Rails::AppBuilder
* Rails::Generators::AppGenerator
* Rails::Generators::AppBase
* Rails::Generators::Base
* Rails::Generators::Actions
* Thor::Actions
* Thor::Group
* Thor::Shell
* Thor::Invocation
* Thor::Base

`--template` に指定したファイルは `Rails::Generators::AppGenerator` インスタンスのコンテキストで実行されます。
Rails::Generators::AppGenerator が継承してるクラスやミックスインしているモジュールのメソッドが使えると考えてください。
「`instance_eval` される」と書いたほうがわかりやすい人もいるとかもしれません。

`rails new` や `rails generator` には、[Thor](http://whatisthor.com/) というライブリが使用されています。
bundler や Vagrant でも利用されているそうです。
もっと詳しいことを知りたいのであれば、
[Github のリボジトリ](https://github.com/erikhuda/thor)や[Wiki](https://github.com/erikhuda/thor/wiki)をみるとよさそうです。
Thor についてはまだ勉強中でまだよくわかりませんが、コマンドラインから実行するプログラムを作成するのを支援するようです。

継承関係は、
```
Rails::Generators::AppGenerator
  < Rails::Generators::AppBase
    < Rails::Generators::Base - [Thor::Actions, Rails::Generators::Actions]
      < Thor::Group - [Thor::Base, Thor::Shell, Thor::Invocation]
```

となっていて、 `<` は継承しているところで `-` はミックスインしているところです。

`Rails::AppBuilder` には `Rails::Generators::AppGenerator` で使用するレシピが書いてある感じになっています。

`Rails::Generators::AppGenerator`に定義されているメソッドは `Thor::Group` の規約により、定義された順番に実行されるようになっています。
`--template`に指定したファイルが実行されるのは最後から二番目になります。
最後は`run_bundle` が実行されます。これは `bundle install` が実行されます。

`rails generator` に対応したクラスのを多くは `Rails::Generators::NamedBase` を継承しています。
継承関係は

```
Rails::Generators::NamedBase < Rails::Generators::Base
```

となっているので、両者に共通するものが `Rails::Generators::Base` になることがわかりました。
`rails new` する時の処理をまるまる変更したいのであれば `Rails::AppBuilder` を継承して`::AppBuilder` を作成すればできそうな感じでした。

ベースになるクラスは `railties/lib/rails/generators` にあります。
generatorとして利用できるものは、ここに配置されているディレクトリに配置されています。
ないものもありますが、`rails g --help` に表示されるものと概ね対応しています。

```
find -maxdepth 2 -type d
.
./css
./css/assets
./css/scaffold
./erb
./erb/controller
./erb/mailer
./erb/scaffold
./js
./js/assets
./rails
./rails/app
./rails/assets
./rails/controller
./rails/generator
./rails/helper
./rails/integration_test
./rails/migration
./rails/model
./rails/plugin_new
./rails/resource
./rails/resource_route
./rails/scaffold
./rails/scaffold_controller
./rails/task
./test_unit
./test_unit/controller
./test_unit/helper
./test_unit/integration
./test_unit/mailer
./test_unit/model
./test_unit/plugin
./test_unit/scaffold
./testing
```

```
$ rails g --help
(中略)
Rails:
  assets
  controller
  generator
  helper
  integration_test
  jbuilder
  mailer
  migration
  model
  resource
  scaffold
  scaffold_controller
  task

Coffee:
  coffee:assets

Jquery:
  jquery:install

Js:
  js:assets

TestUnit:
  test_unit:controller
  test_unit:helper
  test_unit:integration
  test_unit:mailer
  test_unit:model
  test_unit:plugin
  test_unit:scaffold
```

### 試してみる

あまり面白い例ではないですが `rspec` と `pry-byebug` がインストールされた状態になるように挑戦してみました。
折角なので、`rails g scaffold user name:string\ も実行してみました。

スクリプトはこんな感じになりました。

```ruby
gem_group :development, :test do
  gem 'rspec-rails', '~> 2.0'
  gem 'pry-rails'
  gem 'pry-byebug'
end

run_bundle

git :init
git add: '.'
git commit: "-m 'initial commit'"

generate 'rspec:install'

git add: '.'
git commit: "-m 'rspec install'"

generate :scaffold, 'user name:string'
```

手順としては、

* rspec-rails の gem 追加
* pry-byebug の gem 追加
* bundle install
* コミット
* rspec の設定
* コミット
* scaffold

となります。

動作確認は `rake db:migrate` 後に

* rake spec
* rails c

など確認してみてください。

`bundle install` は時間がかかりそうなので、できるだけ回数を減らしたいのです。
なので、gem を設定してから `bundle install` することで2回に抑えました。
コミットの部分はメソッドを用意しても良さそうですね。

これをうまく使っていくと初期設定が楽できそうです。
