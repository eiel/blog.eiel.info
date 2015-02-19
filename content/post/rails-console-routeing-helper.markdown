---

title: "rails console でルーティングが生成した helper を使う"
date: 2014-05-16T15:54:00+09:00
comments: true
categories: ["rails"]
---

Railsで `config/routes.rb` に

```
resources :users
```

とかくと

* users_path
* users_url
* user_path
* user_url
* new_user_path

などなどのViewで利用できるヘルパーが生成される。

このヘルパーを `rails console` で使うには、`app` オブジェクトを経由する。

例

```
app.users_path  # => "/users"
app.users_url  # => "http://www.example.com/records/3.html"
app.user_path(User.first) # => "/users/1"
app.user_path(User.first, :html) # => "/users/1.html"
```

なんとなく関係ない例を混ぜた。

routing helper というらしいのに helper からアクセスできない。

### 関連リンク

* [ActionDispatch ってなんだろう？ - 広島・岡山Ruby交流会01 - そんなこと覚えてない](http://blog.eiel.info/blog/2014/03/30/action-dispatch/)
