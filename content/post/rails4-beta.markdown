---

title: "Rails 4.2.0 beta1 ちょっとだけ見たのでメモしとく。"
date: 2014-08-22T15:21:00+09:00
comments: true
categories: ["rails"]
---

Rails 4.2.0.beta1 が出てるよね。

* [Riding Rails: Rails 4.2.0 beta1: Active Job, Deliver Later, Adequate Record, Web Console](http://weblog.rubyonrails.org/2014/8/20/Rails-4-2-beta1/)

betaが出ると試したくなるのでアップデートしても問題ないものをアップデートして遊ぶ。

### 先に結論

詳細とかあとで述べる。

Gemfile に追加とか変更とか。

```
gem 'rails', '4.2.0.beta1'
gem 'sass-rails', '~> 5.0.0.beta1'
gem 'web-console', '~> 2.0.0.beta2'

group :development, :test do
  gem 'byebug'
  gem 'web-console', '~> 2.0.0.beta2'
end

gem 'rails-html-sanitizer', '~> 1.0'
```

`config/application.rb` に追加

```
config.active_record.raise_in_transactional_callbacks = true
```

`config/environments/development.rb` に追加。

```
config.assets.digest = true
```

### rails new したときの違い

rails new してときの生成されるファイルの違い下記の方法で確認した。


```
$ rails _4.2.0.beta1_ new --no-rc hoge
$ mv hoge 4.2.0.beta1
$ rails _4.1.5_ new --no-rc hoge
$ mv hoge 4.1.5
$ diff -ur 4.1.5 4.2.0.beta1/
```

[全文はGistに貼った。](https://gist.github.com/eiel/403e6e473487bb9a9a42)

まず gem 関連

web-console と byebug と rails-html-sanitizer が追加されてる。
その他は、バージョン調整されてるだけ。

debugger はたぶん ruby 2.1 だと動いてないし、web-console を使うようになったので、byebug も追加された感じがする。よくしらん。

[rails-html-sanitaizer](https://github.com/rails/rails-html-sanitizer) は `sanitize` ヘルパーが追加できるらしい。HTMLタグがとりのぞけて独自のルールはScrubberを作ることで調整ができる模様。

以下、それ意外の抜粋。

* `onfig/application.rb`


```diff
+    # For not swallow errors in after_commit/after_rollback callbacks.
+    config.active_record.raise_in_transactional_callbacks = true
```

ちゃんと確認してない。

```
Currently, Active Record will rescue any errors raised within after_rollback/after_create callbacks and print them to the logs. Next versions of rails will not rescue those errors anymore, and just bubble them up, as the other callbacks.

This adds a opt-in flag to enable that behaviour, of not rescuing the errors. Example:
```

とあってトランザクジョン後のコールバックでのエラー rescue されなくなったのかしら。


* `config/boot.rb`

```diff
-require 'bundler/setup' if File.exist?(ENV['BUNDLE_GEMFILE'])
+require 'bundler/setup' # Set up gems listed in the Gemfile.
```

`ENV['BUNDLE_GEMFILE']` 次第だったのが読まれるようになった。
 Gemfile の一覧になってる gem を読む。


`config/environments/development.rb`

```diff
+  # Asset digests allow you to set far-future HTTP expiration dates on all assets,
+  # yet still be able to expire them through the digest params.
+  config.assets.digest = true
```

development 環境でも assets.digest が true になったらしい。

`environments/production.rb`

```
  # Set to :info to decrease the log volume.
+  config.log_level = :debug

-  # Disable automatic flushing of the log to improve performance.
-  # config.autoflush_log = false
```

production での log_levele が debug になったらしい。

`config/initializers/assets.rb`

```
+# Add additional assets to the asset load path
+# Rails.application.config.assets.paths << Emoji.images_path
```

Emoji.images_path を追加する例が増えてる…。
