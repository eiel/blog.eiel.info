---

title: "Rails4 beta1 がリリースされたのでアップグレードしてみた"
date: 2013-02-27T14:32:00+09:00
comments: true
tags: ["rails4","rails"]
---

[Rails4 beta1がリリースされてます。](http://weblog.rubyonrails.org/2013/2/25/Rails-4-0-beta1/)

というわけで、アップグレードしてもよさそうなプロジェクトをアップグレードしてみました。(リリースしてないし)

config以下の書き換えなどは自己責任で。

## やったこと

bundler 1.3 が必要になるので、まだいれていない場合アップデートします。

```bash
$ gem install bundler
```

Gemfileを修正します。
```ruby
gem 'rails', '4.0.0.beta1'

group :assets do
  gem 'sass-rails', '~> 4.0.0.beta1'
  gem 'coffee-rails', '~> 4.0.0.beta1'

  # See https://github.com/sstephenson/execjs#readme for more supported runtimes
  # gem 'therubyracer', :platforms => :ruby

  gem 'uglifier', '>= 1.0.3'
end
```

## 必要に応じてgemの設定

### protected_attributes
strong_parameters に移行してない場合
```ruby
gem 'protected_attributes'
```

### devise
devise がまだ対応 gem がないので
```ruby
gem 'devise', github: 'plataformatec/devise', branch: 'rails4'
```
githubから直接。

### simple_form
```ruby
gem 'simple_form', '3.0.0.beta1'
```

simple_form が rails4 に対応したものが beta なので

```ruby
gem "simple_form", "3.0.0.beta1"
```

## config をモダンに？

```bash
$ rake rails:update
```

地道に diff をみながら調整しました。自分で変更したところははっきりと分離しておいたほうがよさそうでした。

`routes.rb` は確実に変更していると思います。
がっつり変更されるのでコミットをした上で試しましょう。


## 参考にしたりとか

* [upgrading rails 4](http://www.upgradingtorails4.com/)<br>
  Topics Covered あたりをみながら、必要そうなものを追加したり、修正したりしました。

* [rubygems.org](https://rubygems.org/)<br>
  リリースされてるgemの確認とか

あとは各gem のリポジトリを見にいったりして確認しました。
gem関連はリリースされるころには rails のバージョンを指定をするだけで済むかもしれないペースで対応が進んでいそうですね。
