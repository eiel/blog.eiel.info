---

title: "Rails の自動読み込みの話"
date: 2013-09-07T22:38:00+09:00
comments: true
tags: ["rails","ActiveSupport"]
---

[広島Ruby勉強会 #034](http://hiroshimarb.github.io/blog/2013/09/07/hiroshimarb-34/)で使用したネタの文に書きなおしました。

「Rails の自動読み込み規約を支える技術」と若干煽っておりますが
以下の内容はソースコードを読んで判断したことですべて正しいとは保証できないので参考にする程度にお願いします。

というわけで、Rails の自動読み込みの話をしたいと思います。

<iframe src="http://www.slideshare.net/slideshow/embed_code/25983089" width="427" height="356" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC;border-width:1px 1px 0;margin-bottom:5px" allowfullscreen webkitallowfullscreen mozallowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="https://www.slideshare.net/TomohikoHimura/rails-25983089" title="Rails の自動読み込みを支える技術" target="_blank">Rails の自動読み込みを支える技術</a> </strong> from <strong><a href="http://www.slideshare.net/TomohikoHimura" target="_blank">Tomohiko Himura</a></strong> </div>

### Rails のファイル読み込みの規約

Rails には[設定より規約](http://ja.wikipedia.org/wiki/%E8%A8%AD%E5%AE%9A%E3%82%88%E3%82%8A%E8%A6%8F%E7%B4%84) という設計パラダイムが採用されています。
ファイルの自動読み込みの規約は

> 読み込みされていないクラス/モジュールがあった場合、名前から読み込みするファイルを判断できる

というような規約があります。

例えば

```
Hoge       -> 'hoge.rb' を読み込む
Hoge::Mogu -> 'hoge/mogu.rb' を読み込む
HogeMogu   -> 'hoge_mogu.rb' を読み込む
```

といった感じになっています。
この時、クラス名からファイル名の変換は `ActiveSupport::Inflector.underscore` が利用されます。

Rails では、自動読み込みは `RAILS_ROOT/app/models` のような `RAILS_ROOT/app/` の中のディレクトリに対し行われます。
`RAILS_ROOT/lib` とかに配置しても自動読み込みされません。

これを実現しているモジュールは [ActiveSupport::Dependencies](https://github.com/rails/rails/blob/master/activesupport/lib/active_support/dependencies.rb) になります。

### ActiveSupport::Dependencies

Rails を使わない場合の使い方を紹介します。

```ruby
require 'active_support/dependencies.rb'
ActiveSupport::Dependencies.autoload_paths << 'lib'
```

このようにしておくとクラスやモジュールがない場合 `lib` の中から規約に沿ったファイルを読み込みします。
`autoload_paths` に読み込みの対象となるディレクトリを指定します。
実際に自作ライブラリで利用しないほうが良いです。
こういった用途の場合は後述の `ActiveSupport::Autoload` などで読み込みするのが一般的なようです。
Rails の`app` の中のように DSLを使った記述をシンプルにする場合に使うような印象を浮けました。

Rails での `autoload_paths` の初期値は

* "RAILS_ROOT/app/assets"
* "RAILS_ROOT/app/controllers"
* "RAILS_ROOT/app/helpers"
* "RAILS_ROOT/app/mailers"
* "RAILS_ROOT/app/models"
* "RAILS_ROOT/app/controllers/concerns"
* "RAILS_ROOT/app/models/concerns"

また、自動再読込もこの `autoload_paths` の path のみに適用さます。

#### 仕組み

簡単に仕組みをメモしておきます。

読み込みされていないクラスを使用すると ConstMissing という例外が発生します。
この部分に介入して autoload_paths の中に規約に合うファイルがあるか確認します。
存在する場合は読み込みします。
存在しない場合は `ConstMissing` を発生させます。

この自動読み込みの機能の動きを確認をする場合 Loggerを設定すると便利です。

```ruby
require 'active_support/dependencies.rb'
require 'logger'
ActiveSupport::Dependencies.logger = Logger.new($stderr)
ActiveSupport::Dependencies.log_activity = true
```

とすれば標準エラー出力にログが出力されます。
Rails の場合は Logger の設定がされているので、`log_activety` を設定するだけで大丈夫です。

この自動読み込み機能は、
`require 'active_support/dependencies.rb'` しただけで有効になりますが、
これは `ActiveSupport::Dependencies.hook!` が呼ばれるようになっているためです。

停止したい場合は `ActiveSupport::Dependencies.unhook!` を呼ぶことで停止させることができます。

#### その他知っていると便利かもしれないこと

* ActiveSupport::Dependencies.warnings_on_first_load

この値をtrue にするとはじめて読み込みしたクラスが log レベル warn でメッセージを書き込まみます。
もう一度読まれた場合は Log には出力されません。

* ActiveSupport::Dependencies.history

この機能を使い読み込みしたクラスの一覧が入ります。
Set で保存されているので順番は分かりません。

* ActiveSupport::Dependencies.loaded

この機能を使い読み込みしたクラスの一覧が入ります。
history と違うのは clear されることがあることです。

* ActiveSupport::Dependencies.mechanism

ファイルを読み込みする際に load を利用するか require を利用するかを切り替えできます。
デフォルトは load になっています。シンボルで設定します。
また、環境変数 `NO_RELOAD` を設定しておくと `require` に切り替えられます。
* ActiveSupport::Dependencies.explicitly_unloadable_constants

自動読み込みを行わない定数名を設定できます。

### ActiveSupport::Autoload

`ActiveSupport::Dependencies` はライブラリ上では使われないようです。
似たような機能を持つものとしては [ActiveSupport::Autoload](https://github.com/rails/rails/blob/master/activesupport/lib/active_support/dependencies/autoload.rb) があります。

`lib/active_support.rb` や `lib/active_record.rb` など上げるとキリがありませんが、これらのファイルではたくさんの `autoload` が利用されています。

そもそも ruby には `autoload` が実装されています。
ただし、引数がふたつ必要になります。
この `autoload` を `ActiveSupport::Autoload` で拡張されたものです。

* Kernel.autoload(module, filename)
* Module.autoload(module, filename)
* ActiveSupport::Autoload#autoload(const_name, path = @@at_path)


`autoload` は自動読み込みするきっかけになる名前と読み込みするファイルを渡します。そうです。Rails の規約に添えば2番目の引数が省略できるのです。

ちなみに、Kernel のほうの `autoload` は変化しません。
Module のほうが差し替えられ第2引数が省略できます。

例:

```ruby
require 'active_support/dependencies/autoload'

module Hoge
  extend ActiveSupport::Autoload
  autoload :Mogu
end

Hoge::Mogu
# > LoadError: cannot load such file -- hoge/mogu
```

#### eager_autoload と auto_load!

Rails のソースコードをみていると

```ruby
eager_autoload do
  autoload :Hoge
  autoload :Mogu
end
```

のようになってることがあります。

eager_autoload のブロックにかいておくと `auto_load!` メソッドで一括読み込みができます。
どのような利点があるのかよくわかってません。

しかも、`ActiveSupport::Autoload` って `active_support/dependencies/autload.rb` に定義されてるんですよね。なんでなんだろう。


<iframe src="http://rcm-fe.amazon-adsystem.com/e/cm?lt1=_blank&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=eiel-22&o=9&p=8&l=as4&m=amazon&f=ifr&ref=ss_til&asins=B00P0UR1RU" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>
