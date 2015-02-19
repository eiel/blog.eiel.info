---

title: "rails4 rc1 がリリースされたことだし beta1 からアップグレードしてみた"
date: 2013-05-03T01:36:00+09:00
comments: true
tags: [rails]
---

[Rails 4.0 rc1 がリリース](http://weblog.rubyonrails.org/2013/5/1/Rails-4-0-release-candidate-1/)されてますね。軽いノリでアップグレードしてみたら余裕で起動しませんでした。

アップグレードするには `Gemfile` を以下の編集をしました。

```ruby
-gem 'rails', '4.0.0.beta1'
+gem 'rails', '4.0.0.rc1'

 gem 'pg'

 # Gems used only for assets and not required
 # in production environments by default.
 group :assets do
-  gem 'sass-rails',   '~> 4.0.0.beta1'
-  gem 'coffee-rails', '~> 4.0.0.beta1'
+  gem 'sass-rails',   '~> 4.0.0.rc1'
+  gem 'coffee-rails', '~> 4.0.0.rc1'
```

あとは `bundle update`

そんでもって起動すると下記のエラー。

```
gems/railties-4.0.0.rc1/lib/rails/application/configuration.rb:144:in `const_get': uninitialized consta
nt ActionDispatch::Session::EncryptedCookieStore (NameError)
```

[CHANGELOG.md](https://github.com/rails/rails/blob/4-0-stable/actionpack/CHANGELOG.md)には

> Automatically configure cookie-based sessions to be encrypted if secret_key_base is set, falling back to signed if only secret_token is set. Automatically upgrade existing signed cookie-based sessions from Rails 3.x to be encrypted if both secret_key_base and secret_token are set, or signed with the new key generator if only secret_token is set. This leaves only the config.session_store :cookie_store option and removes the two new options introduced in 4.0.0.beta1: encrypted_cookie_store and upgrade_signature_to_encryption_cookie_store.

とかいてあって、僕には細かいニュアンスは分からないのですが、`config/initializers/session_store.rb` を下記のように修正しました。

```ruby
-RealGame::Application.config.session_store :encrypted_cookie_store, key: '_RealGame_session'
+RealGame::Application.config.session_store :cookie_store, key: '_RealGame_session'
```

そしたら無事に起動するようになりました。
